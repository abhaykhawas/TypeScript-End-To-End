# Day 17 — Effects, refs, and routing boundaries

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Typed effects still need normal React lifecycle care. Abort outdated fetches, and do not turn an aborted request into a user-visible error. An AbortController supplies a signal; cleanup cancels pending work. Strict Mode may exercise setup and cleanup more than once in development.

A ref to a DOM element can be null before mounting or after unmounting. Route parameters may be absent; validate their presence and format before calling an API. Use the React Router version already chosen for your project and its matching documentation; do not mix v6 and v7 examples.

## Worked example
```tsx
import { useEffect, useRef, useState } from "react";
import { getTasks } from "./services/api";
import type { Task, RequestState } from "./services/api";
export function TaskPage() {
  const [state, setState] = useState<RequestState<Task[]>>({ status: "idle" });
  const searchRef = useRef<HTMLInputElement>(null);
  useEffect(() => {
    const controller = new AbortController();
    setState({ status: "loading" });
    getTasks(controller.signal)
      .then(data => {
        if (!controller.signal.aborted) setState({ status: "success", data });
      })
      .catch((error: unknown) => {
        if (!controller.signal.aborted) setState({
          status: "error",
          message: error instanceof Error ? error.message : "Request failed"
        });
      });
    return () => controller.abort();
  }, []);
  return <>
    <input ref={searchRef} />
    <button onClick={() => searchRef.current?.focus()}>Focus search</button>
    <p>{state.status}</p>
  </>;
}
```

## Why this works
The ref is narrowed by optional chaining. Cleanup runs on effect replacement/unmount. The aborted guard prevents an old promise from committing state. Complete the render branches from Day 16; the status-only example isolates lifecycle and typing.

## Do this today
1. Render task data and an error message using the state union.
2. Add a detail route with a checked id parameter and a not-found view.
3. Change a selected id rapidly and make sure older results cannot replace newer data.
4. Test navigating away during a pending request once a backend is connected.
5. Stretch: distinguish a request error from an empty successful list.

## Check your work
- [ ] Ref access handles null.
- [ ] Cleanup cancels obsolete requests.
- [ ] You do not make the effect callback itself async.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[React TypeScript](https://react.dev/learn/typescript) · [useEffect](https://react.dev/reference/react/useEffect)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
