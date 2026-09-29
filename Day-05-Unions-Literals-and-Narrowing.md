# Day 05 — Unions, literal types, and narrowing

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Unions express alternatives. A role union restricts a string to domain values; a string | number value requires narrowing before using methods specific to either member. Narrow with typeof, equality, instanceof, the in operator, and control flow.

Truthiness checks can accidentally discard valid 0 or empty-string values. Use an explicit null check when those values should survive. Types describe valid program states more usefully than a collection of unrelated booleans.

## Worked example
```ts
type Role = "admin" | "mentor" | "student";
function normalizeId(id: string | number): string {
  if (typeof id === "number") return String(id);
  return id.trim();
}
function canManage(role: Role): boolean {
  return role === "admin";
}
function displayScore(score: number | null): string {
  return score === null ? "Not graded" : `${score}%`;
}
console.log(normalizeId(" 42 "), displayScore(0));
export {};
```

## Why this works
After the number branch returns, id must be string. displayScore preserves zero. Role excludes misspellings such as "admn". This is static enforcement; unknown request strings still need validation.

## Do this today
1. Create TaskStatus = 'todo' | 'doing' | 'done' and a status label function.
2. Write formatInput(value: string | number | null) with explicit branches.
3. Model a contact identifier as email or phone variants with a kind property.
4. Demonstrate why if(score) mishandles zero.
5. Stretch: use instanceof Date to narrow Date | string.

## Check your work
- [ ] Zero displays as 0%, not Not graded.
- [ ] Every union member is handled.
- [ ] You can explain why a union needs narrowing.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
