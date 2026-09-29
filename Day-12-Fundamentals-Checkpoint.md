# Day 12 — Fundamentals project: an in-memory task service

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Today combines the first eleven lessons. Build a small task service without React or MongoDB so the type design gets your attention. Use explicit create and patch inputs, stable ids, a status union, and discriminated results for expected failures.

Store tasks privately in a Map. Avoid returning mutable internal objects directly; copy records when returning them. TypeScript readonly helps callers, but shallow copies are only enough for this flat model. Missing tasks are expected outcomes, while a programming error can throw.

## Worked example
```ts
type Status = "todo" | "doing" | "done";
type Task = { id: string; title: string; status: Status };
type CreateTaskInput = Pick<Task, "title">;
type UpdateTaskInput = Partial<Pick<Task, "title" | "status">>;
type Result<T> = { ok: true; data: T } | { ok: false; message: string };
const tasks = new Map<string, Task>();
function createTask(input: CreateTaskInput): Task {
  const task: Task = { id: crypto.randomUUID(), title: input.title, status: "todo" };
  tasks.set(task.id, task);
  return { ...task };
}
function getTask(id: string): Result<Task> {
  const task = tasks.get(id);
  return task ? { ok: true, data: { ...task } }
    : { ok: false, message: "Task not found" };
}
console.log(getTask(createTask({ title: "Build typed service" }).id));
export {};
```

## Why this works
Create inputs cannot supply an id or status. The service controls the initial state. The result forces callers to handle absence before reading data. UpdateTaskInput permits only allowed fields but does not enforce a nonempty title or patch.

## Do this today
1. Implement listTasks(status?), updateTask(id, patch), and deleteTask(id).
2. Reject blank titles and empty patches at runtime; define consistent failure messages.
3. Make a demo that creates three tasks, updates one, filters, deletes, and handles a missing id.
4. Create assertions covering empty list, missing id, and allowed transitions.
5. Stretch: add generic pagination without losing Task types.

## Check your work
- [ ] CRUD works without any, unchecked assertions, or forced non-null values.
- [ ] Returned objects cannot mutate the stored records in this flat model.
- [ ] Type errors reject invalid status and forbidden update fields.
- [ ] You can rebuild the service skeleton from memory.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Handbook review](https://www.typescriptlang.org/docs/handbook/intro)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
