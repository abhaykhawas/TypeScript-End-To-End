# Day 18 — React checkpoint: a complete local TaskBoard

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Finish a local frontend before adding database concerns. Your capstone Task type includes id, title, description, status, ownerId, createdAt, and updatedAt. For local data, use ISO date strings and a placeholder owner id. Keep create inputs separate from the complete response type.

Do not persist derived filter results. Parse localStorage as unknown; validate it before adopting it. Storage can be unavailable or contain old data, so handle both read and write failures. Use ordinary components, a reducer or normal state, and services/api.ts.

## Worked example
```ts
// src/services/storage.ts
import { z } from "zod";
const StoredTaskSchema = z.object({
  id: z.string(), title: z.string(), description: z.string(),
  status: z.enum(["todo", "doing", "done"]), ownerId: z.string(),
  createdAt: z.string(), updatedAt: z.string()
});
export type Task = z.infer<typeof StoredTaskSchema>;
export function loadSavedTasks(): Task[] {
  try {
    const text = localStorage.getItem("taskboard-v1");
    if (!text) return [];
    const raw: unknown = JSON.parse(text);
    const result = z.array(StoredTaskSchema).safeParse(raw);
    return result.success ? result.data : [];
  } catch {
    return [];
  }
}
```

## Why this works
The parser catches corrupt JSON and outdated shapes. Returning [] is an intentional local-demo fallback; a production UI may also notify the user. A string type alone does not enforce ISO dates; strengthen the schema if those values will be used for date calculations.

## Do this today
1. Implement create, edit, delete, search, and status filters.
2. Render empty, invalid-input, and selected-task-not-found states.
3. Persist tasks with a guarded write and validate on reload.
4. Introduce malformed stored data manually; verify a controlled fallback.
5. Stretch: add keyboard-friendly form labels and focus after task creation.

## Check your work
- [ ] Reload preserves valid tasks.
- [ ] Invalid storage does not crash the app.
- [ ] Filtering never destroys the original task collection.
- [ ] npm run build passes and the UI works in the browser.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[React state structure](https://react.dev/learn/choosing-the-state-structure) · [Zod basics](https://zod.dev/basics)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
