# TypeScript End to End — 30-Day MERN Course

Created for Abhay: already comfortable with JavaScript, React, Express, Node, and MongoDB.

## Start here
Open [Day 01](Day-01-Setup-and-Mental-Model.md). Study in numerical order.
Plan 90–120 minutes per lesson; checkpoint projects may need a second session.
“Day” means a learning session, not a deadline. Thirty sessions can span 4–6 weeks or longer.
These are teaching notes and exercises, not an already-installed application.
Run setup commands yourself when you reach those lessons. No packages were installed by creating this course.

## How to use a lesson
1. Read Understand and reproduce the worked example.
2. Hover over inferred types and deliberately introduce an invalid value.
3. Complete Do this today without copying a finished solution.
4. Run the correct type check and test the runtime behavior.
5. Tick the checks and write what you learned.
6. Consult [Exercise hints](EXERCISE-HINTS.md) only after an attempt.

Create practice/, client/, and server/ beside these Markdown files.
Keep standalone practice exercises as modules with export {}.
The examples are focused scaffolds: exercises ask you to implement the remaining behavior.
Code fragments labelled “fragment” or “merge” need the surrounding project from earlier days.
Use ESM with NodeNext on the practice/server projects, and the generated Vite settings on the client.
Do not change module systems in an existing app just to imitate this course.
Use current compatible package versions and commit your package lockfile.

## Course index
| Day | Lesson |
| --- | --- |
| 01 | [Setup and the TypeScript mental model](Day-01-Setup-and-Mental-Model.md) |
| 02 | [Inference, primitive types, and arrays](Day-02-Inference-and-Primitives.md) |
| 03 | [Objects, type aliases, and interfaces](Day-03-Objects-Aliases-and-Interfaces.md) |
| 04 | [Functions, callbacks, and asynchronous return types](Day-04-Functions-and-Callbacks.md) |
| 05 | [Unions, literal types, and narrowing](Day-05-Unions-Literals-and-Narrowing.md) |
| 06 | [Unknown, null safety, and assertions](Day-06-Unknown-Null-and-Assertions.md) |
| 07 | [Discriminated unions and exhaustive handling](Day-07-Discriminated-Unions-and-Never.md) |
| 08 | [Generics that preserve relationships](Day-08-Generics.md) |
| 09 | [Deriving types and update contracts](Day-09-Keyof-Typeof-and-Utility-Types.md) |
| 10 | [Literal preservation, satisfies, tuples, and dictionaries](Day-10-Const-Satisfies-and-Collections.md) |
| 11 | [Modules, imports, and compiler configuration](Day-11-Modules-and-Compiler-Configuration.md) |
| 12 | [Fundamentals project: an in-memory task service](Day-12-Fundamentals-Checkpoint.md) |
| 13 | [React setup and component props](Day-13-React-Setup-and-Props.md) |
| 14 | [React state and form events](Day-14-React-State-and-Forms.md) |
| 15 | [Reducers and Context without unsafe defaults](Day-15-Reducers-and-Context.md) |
| 16 | [Typed API calls, validation, and request state](Day-16-API-Calls-and-Request-State.md) |
| 17 | [Effects, refs, and routing boundaries](Day-17-Effects-Refs-and-Routing.md) |
| 18 | [React checkpoint: a complete local TaskBoard](Day-18-React-Checkpoint.md) |
| 19 | [Express + TypeScript server setup](Day-19-Express-Server-Setup.md) |
| 20 | [Validate request bodies and environment variables](Day-20-Runtime-Validation-and-Environment.md) |
| 21 | [Typed routes and in-memory CRUD](Day-21-Typed-Routes-and-CRUD.md) |
| 22 | [Mongoose models, inferred types, and DTO mapping](Day-22-Mongoose-Models-and-DTOs.md) |
| 23 | [Authentication, JWT verification, and request typing](Day-23-Authentication-and-Request-Types.md) |
| 24 | [Authorization and ownership checks](Day-24-Authorization-and-Ownership.md) |
| 25 | [Error handling and meaningful backend tests](Day-25-Errors-and-Backend-Tests.md) |
| 26 | [Connect the React client to the typed API](Day-26-Connect-the-MERN-App.md) |
| 27 | [Useful advanced types, overloads, and classes](Day-27-Advanced-Types-and-Classes.md) |
| 28 | [Migrate one existing JavaScript feature](Day-28-Migrate-an-Existing-JS-Feature.md) |
| 29 | [Full-stack verification and production build checks](Day-29-Full-Stack-Verification.md) |
| 30 | [Capstone completion and final assessment](Day-30-Capstone-and-Final-Assessment.md) |

## Milestones
- Days 1–6: type everyday functions, model objects, and handle uncertain values.
- Days 7–12: use unions, generics, derived types, modules, and a typed in-memory service.
- Days 13–18: build a typed React TaskBoard.
- Days 19–25: build a validated, authenticated Express/Mongoose API.
- Days 26–30: connect, migrate, test, and extend the full MERN application.

## Capstone: TaskBoard
Implement register, login, logout, task create/read/update/delete, search, and status filtering.
Task DTO: id, title, description, status, ownerId, createdAt, updatedAt.
Status: todo | doing | done. IDs and dates in JSON are strings.
Create input: title, optional description. Patch input: title, description, status; at least one.
Never accept id, ownerId, createdAt, or updatedAt from task write bodies.
Login response: { token, user: { id, name, email } }; never return passwordHash.
List response: TaskDto[]. Detail/create/patch response: TaskDto.
Errors: { message: string }. Delete success: HTTP 204 with no body.
JWT demo: Bearer token held in memory, one-hour expiry, reload requires sign-in.
Ownership: queries scope by caller. Other users' task IDs return 404.
Validate before using input and validate responses before adopting client state.
Keep API functions in services/api.ts and use regular components/reducers.

## Endpoint checklist
| Method | Endpoint | Success | Main failure cases |
| --- | --- | --- | --- |
| POST | /api/auth/register | 201, public user | 400 input; 409 duplicate email |
| POST | /api/auth/login | 200, token + public user | 400 input; 401 credentials |
| GET | /api/tasks | 200, task array | 401 auth; 400 invalid query |
| POST | /api/tasks | 201, task | 400 input; 401 auth |
| GET | /api/tasks/:id | 200, task | 400 malformed id; 401 auth; 404 missing/not owned |
| PATCH | /api/tasks/:id | 200, task | 400 invalid/empty patch; 401 auth; 404 missing/not owned |
| DELETE | /api/tasks/:id | 204, no body | 400 malformed id; 401 auth; 404 missing/not owned |

## Final acceptance
- [ ] Strict checking stays enabled.
- [ ] No unexplained any, as, !, or suppression comments in application code.
- [ ] Every untrusted boundary has runtime checks.
- [ ] Database records map intentionally to public JSON DTOs.
- [ ] Every task operation enforces owner identity.
- [ ] Empty, loading, error, expired-session, and not-found states are handled.
- [ ] Client build, server type check, server build, and relevant tests pass.
- [ ] Compiled backend executes and production API routing is documented.
- [ ] The main browser flow and two-user isolation have been exercised.
- [ ] A real JavaScript feature has been migrated without changing behavior.
- [ ] PROJECT-REVIEW.md records checks actually run and known limitations.

## Debugging questions
What is the actual type? What type is required? Is the data nullable or unvalidated?
Does a check establish the fact, or am I merely asserting it?
Is this a browser module problem, a Node module problem, or a missing declaration?
Did the development runner execute code without checking types?
Are types preventing a bad state, or are independent flags still permitting it?

## Official references
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro)
- [TSConfig reference](https://www.typescriptlang.org/tsconfig/)
- [React with TypeScript](https://react.dev/learn/typescript)
- [Vite](https://vite.dev/guide/)
- [Express](https://expressjs.com/en/guide/routing.html)
- [Mongoose with TypeScript](https://mongoosejs.com/docs/typescript.html)
- [Zod](https://zod.dev/basics)

The learning goal: implement and evolve a MERN feature with useful static contracts,
runtime validation, clear ownership rules, and confidence in the production build.
