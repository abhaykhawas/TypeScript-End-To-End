# Day 20 — Validate request bodies and environment variables

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
req.body comes from outside your program. Treat it as unknown until runtime validation succeeds. TypeScript's request generics do not validate it. Infer input types from schemas to reduce mismatch.

Normalize and validate configuration once on startup. Environment values are strings or undefined; a non-null assertion hides a missing variable. Number conversion alone can produce NaN or accept unwanted values. Validate port ranges and required secrets. Never print real secrets in logs or commit .env.

## Worked example
```ts
// src/schemas/task-input.ts
import { z } from "zod";
export const CreateTaskSchema = z.object({
  title: z.string().trim().min(1).max(120),
  description: z.string().max(2000).default("")
}).strict();
export const UpdateTaskSchema = z.object({
  title: z.string().trim().min(1).max(120).optional(),
  description: z.string().max(2000).optional(),
  status: z.enum(["todo", "doing", "done"]).optional()
}).strict().refine(value => Object.keys(value).length > 0, {
  message: "Provide at least one editable field"
});
export type CreateTaskInput = z.infer<typeof CreateTaskSchema>;
export type UpdateTaskInput = z.infer<typeof UpdateTaskSchema>;
```
```ts
// src/config.ts — used starting on database/auth days
import "dotenv/config";
import { z } from "zod";
const EnvSchema = z.object({
  PORT: z.coerce.number().int().min(1).max(65535).default(3000),
  MONGODB_URI: z.string().min(1),
  JWT_SECRET: z.string().min(32)
});
export const config = EnvSchema.parse(process.env);
```

## Why this works
strict rejects forbidden keys such as ownerId. The update refinement rejects an empty patch. description defaults at runtime, so the parsed output has a string even when input omitted it. Infer from output types and pass parsed.data onward; do not go back to the original body.

## Do this today
1. Validate valid create input, blank title, numeric title, extra ownerId, and empty patch.
2. Use safeParse to collect an error without throwing.
3. Create .env.example containing placeholders for the three variables.
4. Run a standalone config import with missing variables and inspect startup failure.
5. Stretch: compare z.input<typeof Schema> with z.infer<typeof Schema> when defaults or transforms exist.

## Check your work
- [ ] All five input cases produce the intended results.
- [ ] Missing configuration fails early.
- [ ] Controllers consume parsed values rather than unchecked bodies.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Zod parsing and inference](https://zod.dev/basics)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
