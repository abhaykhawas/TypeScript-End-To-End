# Day 22 — Mongoose models, inferred types, and DTO mapping

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Replace the in-memory store with MongoDB. Connect before listening so you do not accept traffic when database initialization has failed. Use a local development database or your own test database; never experiment against production records.

Mongoose can infer raw record types from schemas. A hydrated document has model methods; lean returns plain records. Database ObjectIds and Dates differ from JSON strings, so map explicitly to a TaskDto. A failed lookup returns null and must be handled.

UUID ids from yesterday are not Mongo ObjectIds. Do not pass old local-demo ids to the database API; use tasks created by the new backend.

## Worked example
```ts
// src/models/task.ts
import { Schema, model } from "mongoose";
import type { InferSchemaType } from "mongoose";
const taskSchema = new Schema({
  title: { type: String, required: true },
  description: { type: String, default: "" },
  status: { type: String, enum: ["todo", "doing", "done"], default: "todo", required: true },
  ownerId: { type: Schema.Types.ObjectId, required: true, ref: "User" }
}, { timestamps: true });
export type TaskRecord = InferSchemaType<typeof taskSchema>;
export const TaskModel = model("Task", taskSchema);
```
```ts
// src/task-service.ts — service example
import { TaskModel } from "./models/task.js";
import type { TaskDto } from "./types/task.js";
export async function getTask(id: string): Promise<TaskDto | null> {
  const record = await TaskModel.findById(id).lean().exec();
  if (!record) return null;
  return {
    id: record._id.toString(), title: record.title,
    description: record.description, status: record.status,
    ownerId: record.ownerId.toString(),
    createdAt: record.createdAt.toISOString(),
    updatedAt: record.updatedAt.toISOString()
  };
}
```

## Why this works
Create src/types/task.ts using TaskDto from Day 21. Inline schema inference preserves status values with compatible Mongoose declarations. If your installed version widens a field, inspect its docs and define an explicit raw type or validate during mapping; do not cast blindly. Validate route ids before findById to avoid uncontrolled cast errors.

## Do this today
1. Connect using config.MONGODB_URI, then start listening.
2. Move CRUD into services and replace Map operations with Mongoose queries.
3. Before auth exists, use a temporary test owner ObjectId supplied internally, not by request input.
4. Map all responses explicitly and verify date/id strings.
5. Stretch: learn typed populate and distinguish populated user objects from ObjectId references.

## Check your work
- [ ] Missing valid ids return 404; malformed ids return 400.
- [ ] No raw database document or internal field is sent accidentally.
- [ ] Task data survives a server restart.
- [ ] The frontend accepts the mapped response schema.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Mongoose TypeScript](https://mongoosejs.com/docs/typescript.html) · [Schema inference](https://mongoosejs.com/docs/typescript/schemas.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
