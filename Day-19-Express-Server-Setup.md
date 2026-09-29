# Day 19 — Express + TypeScript server setup

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Create server beside client. This course targets Express 5, ESM, and compiled Node execution. tsx is a convenient development runner; always run a separate type check because executing TS does not prove its types are correct.

Keep routes, controllers, services, models, schemas, and types in src. Begin small: split only when responsibilities become clear. Never reuse Vite's browser compiler configuration for this Node application. Use .js relative import suffixes in backend TS source.

## Worked example
```bash
mkdir server
cd server
npm init -y
npm pkg set type=module
npm install express@5 mongoose zod dotenv bcryptjs jsonwebtoken
npm install -D typescript tsx @types/node @types/express @types/jsonwebtoken
npm pkg set scripts.dev="tsx watch src/index.ts"
npm pkg set scripts.typecheck="tsc --noEmit"
npm pkg set scripts.build="tsc"
npm pkg set scripts.start="node dist/index.js"
mkdir src
```

Create `tsconfig.json`:
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noEmitOnError": true,
    "rootDir": "src",
    "outDir": "dist",
    "types": ["node"]
  },
  "include": ["src"]
}
```

Create `src/index.ts`:
```ts
import express from "express";
const app = express();
app.use(express.json());
app.get("/api/health", (_req, res) => { res.json({ ok: true }); });
app.listen(3000, () => console.log("API listening on 3000"));
```
```bash
npm run typecheck
npm run dev
# Separately: npm run build, then npm start
```

## Why this works
Middleware must run before routes that need parsed JSON. Express's declarations are supplied by @types/express; Mongoose and Zod ship declarations. This server starts with no database requirement, so the health route is an independent setup check.

## Do this today
1. Start the server and request GET http://localhost:3000/api/health.
2. Split app creation into app.ts and listening into index.ts.
3. Inspect compiled imports and start the compiled server.
4. Create .gitignore excluding node_modules, dist, and .env; keep a placeholder .env.example.
5. Stretch: add a graceful shutdown handler that closes the HTTP server.

## Check your work
- [ ] Health returns {ok:true}.
- [ ] Both the type check and compiled execution succeed.
- [ ] No application code relies on a global TypeScript installation.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Express guide](https://expressjs.com/en/starter/hello-world.html) · [TS module options](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
