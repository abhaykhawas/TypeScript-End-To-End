# Day 02 — Inference, primitive types, and arrays

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Inference means TypeScript derives a type from a value. Prefer `const name = "Abhay"` over redundant annotations. Annotate where intent is not obvious or an empty collection needs an element type. Use lowercase `string`, `number`, and `boolean`, not wrapper types such as `String`.

A mutable variable usually widens: `let status = "pending"` becomes string. A constant primitive can keep its literal type. Arrays describe their elements, not their length. A `number[]` can still be empty. TypeScript's number type does not mean positive, finite, or integer.

## Worked example
```ts
const institute = "Learn2Code";
let sessions = 4;
const prices: number[] = [];
prices.push(40, 50);
const total = prices.reduce((sum, price) => sum + price, 0);

function formatPrice(amount: number): string {
  return `$${amount.toFixed(2)}`;
}
console.log(formatPrice(total));
export {};
```

## Why this works
`prices` needs a declared element type because it starts empty. The reduce callback parameters and total are inferred. `formatPrice` states a contract for callers. A string argument is rejected; NaN is still a number and requires a runtime check if it matters.

## Do this today
1. Create string[] students and number[] scores. Calculate the average, handling an empty list.
2. Write formatPercentage(value: number) returning a string. Decide how to handle values outside 0–100.
3. Inspect inferred types by hovering over const and let variables in your editor.
4. Deliberately push a string into scores and explain the error.
5. Stretch: write a function that rejects non-finite prices at runtime.

## Check your work
- [ ] The example prints $90.00.
- [ ] Empty scores do not produce an accidental NaN average.
- [ ] You understand why annotations do not enforce numeric ranges.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
