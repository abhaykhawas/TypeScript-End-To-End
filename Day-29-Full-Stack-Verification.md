# Day 29 — Full-stack verification and production build checks

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Today verifies the capstone, not just its type annotations. Separate compile-time assertions from runtime tests. A wrong status literal should fail type checking; malformed JSON, ownership, and expired sessions require runtime behavior tests.

Check client build and server typecheck/build separately. Launch the server's compiled output and use the frontend build through the intended hosting arrangement. Vite preview previews static output but is not a production deployment system. The dev proxy is not automatically reproduced by production hosting.

Do not repeat low-value tests that merely restate type aliases. Focus on boundaries where bugs can escape the compiler.

## Worked example
```ts
// Put in a dedicated type-check fixture, not a runtime route
import type { TaskDto } from "../src/types/task.js";
const allowed: TaskDto["status"] = "done";
// @ts-expect-error: this literal must remain invalid
const invalid: TaskDto["status"] = "finished";
void allowed;
void invalid;
```
```bash
# In client
npm run build
# In server
npm run typecheck
npm run build
npm start
# Run your backend test script separately
```


## Why this works
@ts-expect-error should fail when the expected diagnostic disappears, making it useful in a dedicated type test. Tests outside src are not included by the production config from Day 19; give the type fixture its own checking config or include it in a separate noEmit test configuration. Do not mistake transpile-only execution for a type assertion check.

## Do this today
1. Run register→login→create→edit→filter→delete→logout as a real browser flow.
2. Test user B against user A's task ids.
3. Test invalid body, malformed id, empty result, network failure, invalid response shape, and token expiry.
4. Inspect auth/task responses for password hashes and internal fields.
5. Stretch: add one browser E2E test for the main happy path and one ownership API test.

## Check your work
- [ ] Both production builds and dedicated tests pass.
- [ ] Compiled backend starts without a dev runner.
- [ ] Client API routing is documented for production.
- [ ] Known limitations and follow-up work are written down.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[TSConfig reference](https://www.typescriptlang.org/tsconfig/) · [Vite build](https://vite.dev/guide/build.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
