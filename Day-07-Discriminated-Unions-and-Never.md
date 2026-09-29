# Day 07 — Discriminated unions and exhaustive handling

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
A shared literal property lets TypeScript distinguish variants. Model request state as idle, loading, success-with-data, or error-with-message. This avoids ambiguous combinations such as loading=true with stale error and no data.

never represents an impossible value. An assertNever function helps detect a missing branch when you extend a union. It should throw at runtime too, since unvalidated external input can violate static assumptions.

## Worked example
```ts
type Result<T> =
  | { ok: true; data: T }
  | { ok: false; message: string };
type State =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; count: number }
  | { status: "error"; message: string };
function assertNever(value: never): never {
  throw new Error(`Unexpected state: ${JSON.stringify(value)}`);
}
function describe(state: State): string {
  switch (state.status) {
    case "idle": return "Ready";
    case "loading": return "Loading";
    case "success": return `${state.count} tasks`;
    case "error": return state.message;
    default: return assertNever(state);
  }
}
console.log(describe({ status: "success", count: 3 }));
export {};
```

## Why this works
Only success has count; only error has message. The switch narrows the object, not just its status string. If you add a cancelled variant, the default receives a non-never value and type checking fails until the new case is handled.

## Do this today
1. Model AssignmentState with draft, submitted, graded, and returned variants.
2. Store a numeric score only on the graded variant; store feedback on returned.
3. Write an exhaustive label function and then add a new variant to see the missing-case error.
4. Create Result<Student> and process both branches.
5. Stretch: model authentication as checking, anonymous, or authenticated with a user.

## Check your work
- [ ] Success prints 3 tasks.
- [ ] Adding a variant exposes incomplete handling.
- [ ] Impossible combinations cannot be constructed through the declared union.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Discriminated unions](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#discriminated-unions)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
