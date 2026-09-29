# Day 16 — Typed API calls, validation, and request state

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
An Axios generic or a fetch response assertion describes what you expect; neither checks JSON. At a network boundary, validate before the data reaches UI state. Use Zod here to learn schema-based runtime validation and inferred types.

In client run npm install zod. Put API functions in src/services/api.ts. Keep request state as a discriminated union. Fetch only rejects on network failure, not automatically on 404 or 500, so inspect response.ok.

## Worked example
```ts
// src/services/api.ts
import { z } from "zod";
export const TaskSchema = z.object({
  id: z.string(), title: z.string(),
  status: z.enum(["todo", "doing", "done"])
});
export type Task = z.infer<typeof TaskSchema>;
export async function getTasks(signal?: AbortSignal): Promise<Task[]> {
  const response = await fetch("/api/tasks", { signal });
  if (!response.ok) throw new Error(`Request failed: ${response.status}`);
  const raw: unknown = await response.json();
  return z.array(TaskSchema).parse(raw);
}
export type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; message: string };
```

## Why this works
parse either returns validated data or throws. z.infer derives the static type from the runtime schema. This example expects a direct array; use that same contract on your server. Until Day 21 provides the route, use fixture data for UI practice and document that live fetch is not connected yet.

## Do this today
1. Create a task fixture and validate it with TaskSchema.
2. Validate an invalid status and inspect the failure.
3. Implement a UserSchema without password fields.
4. Create RequestState<Task[]> state and render each branch.
5. Stretch: use safeParse and return a discriminated result instead of throwing.

## Check your work
- [ ] Malformed data never enters success state.
- [ ] HTTP failures and malformed JSON data are both handled.
- [ ] The documented API response shape matches the parser.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Zod basics](https://zod.dev/basics) · [React TypeScript](https://react.dev/learn/typescript)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
