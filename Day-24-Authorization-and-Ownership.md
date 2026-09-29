# Day 24 — Authorization and ownership checks

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Authentication identifies the caller; authorization decides whether the caller may access a resource. Types cannot enforce ownership of a database record. Every task query must scope by authenticated owner, including reads, updates, deletes, and lists.

Do not trust ownerId in a request body. Derive it from req.auth after middleware. An optional property still requires a local guard because TypeScript does not infer middleware execution order. Returning 404 for another user's task avoids disclosing whether that id exists.

## Worked example
```ts
// Controller fragment; wire after requireAuth
import type { RequestHandler } from "express";
import { TaskModel } from "../models/task.js";
export const deleteTask: RequestHandler<{ id: string }> = async (req, res) => {
  if (!req.auth) {
    res.status(401).json({ message: "Authentication required" }); return;
  }
  if (!/^[a-f0-9]{24}$/i.test(req.params.id)) {
    res.status(400).json({ message: "Invalid task id" }); return;
  }
  const deleted = await TaskModel.findOneAndDelete({
    _id: req.params.id, ownerId: req.auth.userId
  }).exec();
  if (!deleted) {
    res.status(404).json({ message: "Task not found" }); return;
  }
  res.sendStatus(204);
};
```

## Why this works
The query itself combines resource identity and ownership, avoiding a separate check-then-delete gap. For updates, also use validated allowed fields and runValidators: true. New tasks get ownerId from auth. Remove all Day 21/22 temporary owner logic.

## Do this today
1. Protect every /api/tasks route with requireAuth.
2. Scope list/detail/update/delete queries by ownerId.
3. Create users A and B; verify B cannot read, update, or delete A's task.
4. Reject ownerId in create and patch bodies.
5. Stretch: add an explicit admin policy rather than automatically treating any role string as permission.

## Check your work
- [ ] Cross-user access returns the documented 404 policy.
- [ ] Create always uses the authenticated owner.
- [ ] No route retains local-demo or temporary owner values.
- [ ] Authorization checks occur at runtime, not through an assertion.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Express middleware](https://expressjs.com/en/guide/using-middleware.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
