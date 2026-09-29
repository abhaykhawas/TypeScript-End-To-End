# Day 06 — Unknown, null safety, and assertions

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Use unknown when you do not yet know a value's shape. any permits operations without proof and can spread through a codebase. unknown makes you check first. Under strict null checks, find results, optional properties, and nullable state require handling.

An assertion such as `as Student` changes the compiler's belief. It does not transform or inspect the value. The non-null assertion `!` similarly removes uncertainty without a runtime check. Prefer a guard that establishes a fact.

## Worked example
```ts
function readName(value: unknown): string {
  if (
    typeof value === "object" && value !== null &&
    "name" in value && typeof value.name === "string"
  ) {
    return value.name;
  }
  throw new Error("Expected an object with a string name");
}
try {
  console.log(readName({ name: "Abhay" }));
} catch (error: unknown) {
  console.error(error instanceof Error ? error.message : "Unknown failure");
}
export {};
```

## Why this works
Checking object alone is insufficient because typeof null is object. The property existence check permits inspection, and the string check establishes the return type. A caught value can be anything thrown by JS, so Error is checked before accessing message.

## Do this today
1. Accept unknown and safely read a numeric age property.
2. Use find() on a student array and handle the missing case without !.
3. Parse JSON into an unknown variable and inspect the fields you use.
4. Take one as assertion from an existing app and replace it with a check.
5. Stretch: distinguish an optional email from email: string | null.

## Check your work
- [ ] Valid input returns Abhay; malformed input throws the intended error.
- [ ] No unchecked any or non-null assertions are used.
- [ ] You understand that JSON.parse does not validate a domain model.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Everyday Types: assertions and null](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
