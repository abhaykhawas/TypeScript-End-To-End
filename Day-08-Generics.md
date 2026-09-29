# Day 08 — Generics that preserve relationships

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Generics let a function work with many types while preserving the relationship between its inputs and outputs. Returning any loses that relationship. A type parameter is a placeholder selected at a call site or inferred from arguments.

Use a constraint when your algorithm depends on a property. Avoid adding generic parameters that do not connect anything; a generic is useful when it expresses a real relationship.

## Worked example
```ts
function first<T>(items: readonly T[]): T | undefined {
  return items[0];
}
function indexById<T extends { id: string }>(items: T[]): Map<string, T> {
  return new Map(items.map(item => [item.id, item]));
}
type Student = { id: string; name: string };
const students: Student[] = [{ id: "s1", name: "Maya" }];
const student = first(students);
console.log(student?.name);
const lookup = indexById(students);
console.log(lookup.get("s1")?.name);
export {};
```

## Why this works
first preserves the array element type and acknowledges emptiness. The constraint guarantees an id while preserving other Student properties. Map.get can return undefined even when the map's value type is Student.

## Do this today
1. Write last<T>(items) and handle an empty array.
2. Write a generic Paginated<T> type with items, page, pageSize, and total.
3. Write findById<T extends {id: string}>(items, id).
4. Compare a generic identity function against one accepting and returning any.
5. Stretch: write mapItems<T, U>(items, transform) returning U[].

## Check your work
- [ ] Student properties remain available after using a helper.
- [ ] Empty and missing results require handling.
- [ ] The constraint rejects objects without a string id.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
