# Day 14 — React state and form events

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
useState infers from its initial value. Give an explicit generic for empty arrays, nullable objects, or unions. A form's input is a string even when you eventually need a number. Convert and validate it at the boundary.

Use ChangeEvent<HTMLInputElement> and FormEvent<HTMLFormElement> for extracted handlers, or inspect inferred event types from inline handlers. currentTarget is the element the handler belongs to. Use functional updates when new state depends on old state.

## Worked example
```tsx
import { useState } from "react";
import type { FormEvent } from "react";
type Task = { id: string; title: string; status: "todo" | "doing" | "done" };
export function TaskForm() {
  const [title, setTitle] = useState("");
  const [tasks, setTasks] = useState<Task[]>([]);
  function submit(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();
    const trimmed = title.trim();
    if (!trimmed) return;
    setTasks(previous => [...previous, {
      id: crypto.randomUUID(), title: trimmed, status: "todo"
    }]);
    setTitle("");
  }
  return <>
    <form onSubmit={submit}>
      <input value={title} onChange={e => setTitle(e.currentTarget.value)} />
      <button>Add task</button>
    </form>
    <p>{tasks.length} tasks</p>
  </>;
}
```

## Why this works
Task[] prevents empty-array inference problems. The new task is checked against Task. submit blocks empty titles. Local state belongs at the common parent when a separate list needs these tasks; lift it from this isolated example in your app.

## Do this today
1. Lift task state to App and make TaskForm call onCreate(title).
2. Implement typed edit and delete handlers using immutable updates.
3. Add selectedTask state as Task | null and render the missing case.
4. Add a numeric estimate input; reject invalid or negative values after conversion.
5. Stretch: distinguish raw form strings from validated submission data.

## Check your work
- [ ] Blank titles create no task.
- [ ] Adding repeatedly never overwrites earlier tasks.
- [ ] No DOM event uses any or an unnecessary assertion.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[React typed hooks and events](https://react.dev/learn/typescript)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
