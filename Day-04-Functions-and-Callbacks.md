# Day 04 — Functions, callbacks, and asynchronous return types

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
A function's contract includes parameters and its return value. Infer internal returns when obvious; explicit public return types can catch accidental changes. Optional parameters accept undefined. Default parameters supply a value when the caller omits the argument or passes undefined.

Callback types describe what callers can supply. `void` means callers should not use a result; it does not always mean a callback implementation literally returns undefined. Async functions return promises. Annotate the resolved value using `Promise<T>`.

## Worked example
```ts
type Task = { id: string; title: string; completed: boolean };
function filterTasks(
  tasks: Task[],
  predicate: (task: Task) => boolean
): Task[] {
  return tasks.filter(predicate);
}
async function loadTasks(): Promise<Task[]> {
  return [{ id: "t1", title: "Learn TS", completed: false }];
}
const pending = filterTasks(await loadTasks(), task => !task.completed);
console.log(pending.length);
export {};
```

## Why this works
The predicate receives Task and must produce boolean. The callback parameter is inferred at the call site. Awaiting loadTasks yields Task[]. This mock returns data locally; it says nothing about HTTP response validation.

## Do this today
1. Write calculateTotal(items, taxRate = 0) with a declared return type.
2. Write mapNames(users, formatter) where the formatter receives a user and returns a string.
3. Write an async mock getStudent(id) returning Student | null inside a Promise.
4. Try returning a string from the filter predicate and explain the diagnostic.
5. Stretch: write a typed rest parameter function sum(...values: number[]).

## Check your work
- [ ] Pending length is 1.
- [ ] You can distinguish Promise<Student> from Student.
- [ ] Optional parameters follow required parameters.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[More on Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
