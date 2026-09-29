# Day 11 — Modules, imports, and compiler configuration

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Know which tool checks, transforms, and executes your code. tsc can check and emit; a frontend bundler transforms for the browser; some development runners execute TS without checking types. Passing a dev run is not proof of a clean type check.

This course uses NodeNext ESM on the server. Write .js suffixes for relative imports in TS source because the emitted files run as JavaScript under Node. Use import type for symbols that exist only in the type system. Frontend Vite templates use bundler-oriented settings; keep their configuration rather than copying backend settings.

Learn target, lib, module, moduleResolution, strict, include, outDir, noEmit, and declaration. A paths alias does not by itself rewrite imports for Node.

## Worked example
```ts
// src/types/student.ts
export type Student = { id: string; name: string };
```
```ts
// src/services/student-service.ts
import type { Student } from "../types/student.js";
export function describeStudent(student: Student): string {
  return `${student.id}: ${student.name}`;
}
```
```ts
// src/day11.ts
import { describeStudent } from "./services/student-service.js";
console.log(describeStudent({ id: "s1", name: "Maya" }));
```
```bash
npx tsc --noEmit
npx tsc
node dist/day11.js
```

## Why this works
The type import is erased. The runtime service import remains and points to a real emitted .js file. include controls which source files enter this project. @types packages supply declarations for libraries that do not ship their own; check a package before installing duplicate declarations.

## Do this today
1. Split one exercise into types, helpers, and a main module.
2. Inspect output for runtime versus type-only imports.
3. Run compiled code from dist, not just through a development runner.
4. Write down how NodeNext and a frontend bundler differ.
5. Stretch: read an installed package's declaration file to learn its API.

## Check your work
- [ ] The built module executes without module-resolution errors.
- [ ] You can identify a missing package versus a missing type declaration.
- [ ] Source/output folders remain separate.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Choosing compiler options](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
