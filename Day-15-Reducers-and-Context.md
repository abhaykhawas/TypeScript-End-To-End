# Day 15 — Reducers and Context without unsafe defaults

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
A reducer models transitions using a typed action union. Each action carries only the payload it needs. Context shares state and actions, but a made-up default user or dispatch function can hide a missing provider.

Use undefined as the context default and check at the consuming component, or write a small checked context accessor if you prefer. This lesson uses the direct check, so no custom hook is required. Treat Redux Toolkit as an optional alternative: derive RootState and AppDispatch from your store rather than manually duplicating them.

## Worked example
```tsx
import { createContext, useContext, useReducer } from "react";
import type { Dispatch, ReactNode } from "react";
type Task = { id: string; title: string };
type Action =
  | { type: "add"; task: Task }
  | { type: "remove"; id: string };
function reducer(state: Task[], action: Action): Task[] {
  switch (action.type) {
    case "add": return [...state, action.task];
    case "remove": return state.filter(task => task.id !== action.id);
  }
}
const TasksContext = createContext<{
  tasks: Task[]; dispatch: Dispatch<Action>;
} | undefined>(undefined);
export function TasksProvider({ children }: { children: ReactNode }) {
  const [tasks, dispatch] = useReducer(reducer, []);
  return <TasksContext.Provider value={{ tasks, dispatch }}>
    {children}
  </TasksContext.Provider>;
}
export function TaskCount() {
  const context = useContext(TasksContext);
  if (!context) throw new Error("TaskCount needs TasksProvider");
  return <p>{context.tasks.length} tasks</p>;
}
```

## Why this works
The switch knows task exists for add and id exists for remove. A provider mistake surfaces immediately. The inferred reducer return is checked as Task[]. Keep persistence and HTTP effects outside a reducer because reducer logic should stay pure.

## Do this today
1. Add rename and changeStatus actions to the capstone reducer.
2. Render the task list through Context and dispatch actions from the form.
3. Temporarily remove the provider and verify the meaningful failure.
4. Keep filter-derived lists out of state; compute from tasks and filter.
5. Stretch: use Redux Toolkit instead, typing actions with PayloadAction and deriving store types.

## Check your work
- [ ] Invalid payloads are rejected.
- [ ] All updates are immutable.
- [ ] Missing provider is handled explicitly.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[React Context and reducers](https://react.dev/learn/typescript)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
