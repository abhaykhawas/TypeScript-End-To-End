# Day 01 — Setup and the TypeScript mental model

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
TypeScript adds static checking to JavaScript. A type annotation describes what values are allowed; it does not convert a value or validate a network response. Most type syntax disappears from emitted JavaScript. Your existing knowledge of closures, promises, arrays, and the event loop still applies.

Create a separate `practice` directory inside this learning folder; put your code there, not in the lesson files. Check `node -v` and `npm -v`. Use a currently supported Node LTS satisfying your tools' requirements. Do not install anything globally. Run the commands below yourself.

Use `strict` from the start. `noEmitOnError` stops this project's build on type errors; `tsc --noEmit` checks without building. This exercises project uses ESM consistently.

## Worked example
```bash
mkdir practice
cd practice
npm init -y
npm install -D typescript @types/node
npm pkg set type=module
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
    "noEmitOnError": true,
    "rootDir": "src",
    "outDir": "dist",
    "types": ["node"]
  },
  "include": ["src"]
}
```

Create `src/day01.ts`:
```ts
const learner: string = "Abhay";
console.log(`Hello, ${learner}`);
export {};
```
```bash
npx tsc --noEmit
npx tsc
node dist/day01.js
```

## Why this works
`NodeNext` follows Node's module rules; `type=module` makes this project ESM. `export {}` makes an exercise a module so its names do not collide with another day's exercise. Keep that line in standalone exercise files. Look at the emitted JS: the `: string` annotation is absent.

## Do this today
1. Change learner to a number and inspect the diagnostic. Fix the value, then compile again.
2. Compare the .ts source and .js output. Write down what was removed.
3. Create day01-exercises.ts with a name, age, and isMentor variable; print them.
4. Stretch: explain why a successful type check cannot prove that a server is reachable.

## Check your work
- [ ] The example prints Hello, Abhay.
- [ ] An incompatible assignment fails type checking.
- [ ] You can distinguish checking, building, and running.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[The Basics](https://www.typescriptlang.org/docs/handbook/2/basic-types.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
