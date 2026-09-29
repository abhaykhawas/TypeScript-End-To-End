# Day 25 — Error handling and meaningful backend tests

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Express 5 forwards rejected promises returned by async handlers to error middleware. Express 4 requires a wrapper or explicit next(error); do not mix assumptions. Error middleware has four arguments and belongs after routes.

Unknown errors should not expose stack traces, credentials, or database details in responses. Expected validation and not-found outcomes can be handled directly. Translate known duplicate-key or malformed-body failures deliberately.

Write tests for behavior at boundaries: runtime input, authorization, missing records, and DTO serialization. A clean type check cannot substitute for these tests.

## Worked example
```ts
// src/middleware/errors.ts
import type { ErrorRequestHandler } from "express";
export const errorHandler: ErrorRequestHandler = (
  error: unknown, _req, res, next
) => {
  if (res.headersSent) { next(error); return; }
  // Log internally using your logger; do not send raw errors to clients.
  res.status(500).json({ message: "Internal server error" });
};
```
```ts
// In app.ts, after routes
app.use((_req, res) => { res.status(404).json({ message: "Route not found" }); });
app.use(errorHandler);
```
```bash
npm install -D supertest @types/supertest
# Keep tests in tests/, outside the src-only production build.
# A test script can use: tsx --test tests/*.test.ts
```

## Why this works
Separate app creation from listening so tests can import app without opening a fixed port. Use a dedicated test database and explicit setup/cleanup; never clear a production database. Add a classifier for malformed JSON so it returns 400 rather than falling through to this minimal 500 fallback.

## Do this today
1. Test blank title→400, valid creation→201, missing id→404, missing auth→401.
2. Test two-user ownership isolation and forbidden patch fields.
3. Test that task dates/ids are strings and passwordHash is absent from auth responses.
4. Simulate a service rejection and verify the generic 500 response.
5. Stretch: add unit tests for validation transformations and DTO mapping.

## Check your work
- [ ] Tests verify behavior, not just copied implementation details.
- [ ] A failed async handler does not hang the request.
- [ ] Malformed JSON gets an intentional 400 response after your classifier is added.
- [ ] npm run typecheck and npm run build both pass.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Express error handling](https://expressjs.com/en/guide/error-handling.html) · [Supertest](https://github.com/ladjs/supertest)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
