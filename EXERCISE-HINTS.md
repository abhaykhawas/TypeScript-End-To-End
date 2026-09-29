# Exercise Hints — Use After an Attempt

These are directions and expected behaviors, not complete project solutions.
Keep your attempt before reading a hint. Explain your fix in your own words.

| Day | Hint / expected result |
| --- | --- |
| 01 | Number assigned to a string should fail checking; emitted JavaScript has no type annotation. |
| 02 | Average: handle length === 0 before dividing; Number.isFinite checks non-finite values. |
| 03 | Optional means the property may be absent; nullable means null is an allowed value. |
| 04 | Async missing lookup: Promise<Student \| null>; annotate callback inputs through the parent function contract. |
| 05 | Check score === null rather than if(score); zero is a valid score. |
| 06 | Check object, non-null, property presence, and property type before returning unknown data. |
| 07 | New union variants should break an exhaustive switch until their branch is added. |
| 08 | last returns T \| undefined. findById has a constraint requiring a string id. |
| 09 | ProfilePatch = Partial<Pick<User, "name" \| "email">>; construct public objects explicitly. |
| 10 | Derive union from (typeof values)[number]; missing labels should fail satisfies. |
| 11 | Node ESM relative imports refer to emitted .js paths even in TS source. |
| 12 | Patch only allowed fields; validate trimmed title; copy flat records before exposing them. |
| 13 | Callback prop is (id: string) => void; an optional callback needs a conditional check. |
| 14 | Use a functional state update; input strings need conversion before numeric use. |
| 15 | A reducer action union ties each payload to a specific type literal. |
| 16 | Assign response JSON to unknown, then parse. A response generic is not validation. |
| 17 | AbortController cleanup plus an aborted guard prevents obsolete requests from changing state. |
| 18 | Catch storage/JSON failures and validate the stored shape. Keep filtered lists derived. |
| 19 | Import app without calling listen in tests; run the compiled server as a separate check. |
| 20 | Pass safeParse.data downstream; strict schemas reject forbidden keys; empty patches fail. |
| 21 | After res.status(...).json(...), return so execution does not send twice. |
| 22 | Validate ObjectId format before a query; check null; map ObjectId and Date to strings. |
| 23 | jwt.verify is required; match issuer/audience/algorithm; validate payload and use a signed expiry. |
| 24 | Include ownerId in the database filter, not merely a frontend condition. |
| 25 | Four-argument error handler goes last; use a dedicated test database and intentional malformed-JSON handling. |
| 26 | 204 has no JSON body; the Vite dev proxy does not configure production routing. |
| 27 | Use Awaited and ReturnType before inventing a complicated conditional type. |
| 28 | Migrate leaf helpers first and preserve behavior; allowJs supports a mixed project. |
| 29 | Type fixtures need their own checking config when tests are excluded from the production build. |
| 30 | Adding a type field cannot backfill existing database records; choose a migration/default policy. |

## Common compiler messages

### “Possibly undefined” or “Possibly null”
Inspect whether the value is a lookup, optional property, route parameter, or ref.
Branch on absence. Do not add ! unless you have a documented invariant the compiler cannot express.

### “Type X is not assignable to type Y”
Compare the actual and expected shape, including required fields and union alternatives.
Check whether raw form data or a database record is being passed where a validated input or DTO belongs.

### “Property does not exist on union”
Narrow using a discriminant, typeof, instanceof, or a checked property.
Do not assert the desired variant just because it is convenient.

### “Cannot find module”
Check package installation, file spelling, relative path, module mode, and Node .js suffixes.
If runtime import works but declarations are missing, inspect whether the package ships types.

### “Object literal may only specify known properties”
It may be a typo or a forbidden operation input.
Do not widen a public update contract to permit an id/owner/role field unintentionally.

## Testing input examples
- Task create valid: { "title": "Learn TS", "description": "Day 20" }.
- Blank title: { "title": "   " } → 400.
- Extra owner: { "title": "Task", "ownerId": "another-user" } → 400.
- Empty patch: {} → 400.
- Invalid status: { "status": "finished" } → 400.
- Missing credentials on task route → 401.
- Validly formatted missing task id → 404.
- Another user's valid task id → 404.
- Successful delete → 204, response body empty.

Use these to understand the contract. Implement and verify each behavior yourself.
