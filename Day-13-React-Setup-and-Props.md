# Day 13 — React setup and component props

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Create a client directory alongside practice using Vite's react-ts template. Check Vite's current Node requirements first. The template supplies React declarations and appropriate compiler settings. Keep the generated tsconfig files.

Use .tsx for JSX and .ts for ordinary modules. Props describe the component's inputs. Functions passed through props should have explicit parameter contracts. ReactNode describes renderable children. You do not need React.FC for ordinary function components.

Start the capstone TaskBoard. Keep domain types in src/types/task.ts and components in src/components. Use normal API files later; no custom-hook architecture is required.

## Worked example
```bash
npm create vite@latest client -- --template react-ts
cd client
npm install
npm run dev
```
```tsx
// src/components/TaskCard.tsx
import type { ReactNode } from "react";
export type Task = {
  id: string; title: string; status: "todo" | "doing" | "done";
};
type Props = {
  task: Task;
  onSelect: (id: string) => void;
  children?: ReactNode;
};
export function TaskCard({ task, onSelect, children }: Props) {
  return <article>
    <h2>{task.title}</h2>
    <button onClick={() => onSelect(task.id)}>Select</button>
    {children}
  </article>;
}
```

## Why this works
The arrow in onClick delays calling onSelect until the click. TaskCard's callback contract guarantees a string id. Missing task or a callback expecting number is rejected. Move the exported Task type to the shared client type file when multiple components need it.

## Do this today
1. Render three TaskCards in App.tsx with stable keys.
2. Add optional onDelete and render the delete button only when supplied.
3. Create a reusable panel accepting title and ReactNode children.
4. Inspect npm run build: verify the generated script includes a type-check step.
5. Stretch: model mutually exclusive link-versus-button props as a discriminated union.

## Check your work
- [ ] Clicking Select passes the correct task id.
- [ ] Missing required props produce useful diagnostics.
- [ ] Client build passes; the dev server alone is not your type check.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[React TypeScript](https://react.dev/learn/typescript) · [Vite setup](https://vite.dev/guide/)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
