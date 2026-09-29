# Day 21 — Typed routes and in-memory CRUD

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Start HTTP CRUD against a Map before connecting MongoDB. Give routes parameter, response, and request-body types where useful, while preserving unknown at the unvalidated body boundary. Return consistent status codes: 201 create, 400 invalid input, 404 missing resource.

Use direct arrays for successful list responses, matching Day 16. Define TaskDto with JSON-safe fields; the initial store owns ids and timestamps. Public endpoints today are a development step; protect task routes on Days 23–24 before treating the app as complete.

## Worked example
```ts
// Replace the minimal app.ts with this setup; retain your health route.
import express from "express";
import { CreateTaskSchema } from "./schemas/task-input.js";
type TaskDto = {
  id: string; title: string; description: string;
  status: "todo" | "doing" | "done"; ownerId: string;
  createdAt: string; updatedAt: string;
};
type ErrorDto = { message: string };
const tasks = new Map<string, TaskDto>();
export const app = express();
app.use(express.json());
app.get("/api/tasks", (_req, res) => { res.json([...tasks.values()]); });
app.post<{}, TaskDto | ErrorDto, unknown>("/api/tasks", (req, res) => {
  const parsed = CreateTaskSchema.safeParse(req.body);
  if (!parsed.success) {
    res.status(400).json({ message: "Invalid task input" });
    return;
  }
  const now = new Date().toISOString();
  const task: TaskDto = {
    ...parsed.data, id: crypto.randomUUID(), status: "todo",
    ownerId: "local-demo", createdAt: now, updatedAt: now
  };
  tasks.set(task.id, task);
  res.status(201).json(task);
});
```

## Why this works
The response generic describes allowed responses; the body remains unknown. Returning after sending a 400 prevents a second response. The generated fields follow the spread, so input cannot overwrite them; the strict schema also excludes them.

## Do this today
1. Implement GET /api/tasks/:id, PATCH /api/tasks/:id, and DELETE /api/tasks/:id.
2. Use UpdateTaskSchema for patch and preserve id, ownerId, and createdAt.
3. Return 204 with no JSON body on successful delete.
4. Exercise routes using Postman or curl with valid, malformed, and missing-id cases.
5. Stretch: validate status and pagination query strings before using them.

## Check your work
- [ ] Malformed body returns 400, missing task 404, create 201, delete 204.
- [ ] A body containing id or ownerId is rejected.
- [ ] List response is an array matching the frontend parser.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Express routing](https://expressjs.com/en/guide/routing.html) · [Zod basics](https://zod.dev/basics)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
