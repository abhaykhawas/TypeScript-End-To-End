# Day 30 — Capstone completion and final assessment

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Finish TaskBoard and explain its types as if reviewing a teammate's pull request. The target is useful correctness and maintainability, not the highest number of annotations. Separate input schemas, database records, and public DTOs; share or derive contracts intentionally.

A good final review can trace data from a form through request validation, ownership checks, a service, MongoDB, DTO mapping, response parsing, and UI state. Identify which guarantees are static and which are runtime.

Timebox the project extension. Choose one feature that tests your understanding rather than adding many unfinished features.

## Worked example
```ts
// Design exercise: add priority end to end
export const priorities = ["low", "medium", "high"] as const;
export type Priority = (typeof priorities)[number];
export const priorityLabels = {
  low: "Low", medium: "Medium", high: "High"
} satisfies Record<Priority, string>;
```

Update the runtime schemas, Mongoose schema, response mapper, form,
API parsing, list filter, and tests. Decide how existing records receive
a default and document the migration. This snippet alone does not add
the feature to the app.

## Why this works
The new union creates compiler feedback where priority is required, but the compiler cannot migrate old database records. Runtime defaults, a data migration, and response compatibility require deliberate decisions. This is exactly where TypeScript and application engineering meet.

## Do this today
1. Finish all capstone acceptance checks listed in README.
2. Add priority end to end, including old-record handling.
3. Remove stale mock data, temporary owner logic, dead types, and undocumented assertions.
4. Write PROJECT-REVIEW.md explaining architecture, checks run, and known limitations.
5. Stretch: extract shared Zod contracts into a small package only after duplicate contracts become a real maintenance problem.

## Check your work
- [ ] You can build a typed CRUD feature without following a tutorial.
- [ ] You can explain inference, narrowing, generics, unknown, and runtime validation.
- [ ] You can trace the full request lifecycle and ownership policy.
- [ ] You can change one domain field across the stack without weakening checks.
- [ ] All project checks pass and limitations are documented.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro) · [React TypeScript](https://react.dev/learn/typescript) · [Mongoose TypeScript](https://mongoosejs.com/docs/typescript.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
