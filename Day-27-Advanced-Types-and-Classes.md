# Day 27 — Useful advanced types, overloads, and classes

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Now learn advanced language features in small examples. Mapped types transform a set of keys. Conditional types choose based on assignability; infer can capture a type inside another type. Template literal types construct string patterns. Prefer built-in utility types when they already express your intent.

Overloads describe multiple call signatures with one compatible implementation. Often a union parameter is simpler. Classes can use typed fields, interfaces, access modifiers, and generics; you do not need to rewrite a functional MERN app around them.

TypeScript private is primarily compile-time access control; JavaScript #private fields also enforce privacy at runtime.

## Worked example
```ts
type Flags<T> = { [K in keyof T]: boolean };
type ArrayItem<T> = T extends readonly (infer Item)[] ? Item : never;
type TaskEvent = `task:${"created" | "updated" | "deleted"}`;
interface Repository<T extends { id: string }> {
  get(id: string): T | undefined;
  save(item: T): void;
}
class MemoryRepository<T extends { id: string }> implements Repository<T> {
  #items = new Map<string, T>();
  get(id: string): T | undefined { return this.#items.get(id); }
  save(item: T): void { this.#items.set(item.id, item); }
}
const repository = new MemoryRepository<{ id: string; title: string }>();
repository.save({ id: "t1", title: "Review advanced types" });
console.log(repository.get("t1")?.title);
export {};
```

## Why this works
Flags preserves the keys of T and changes values to boolean. ArrayItem extracts an array element type, but not an arbitrary object's property. Template strings restrict event names. This repository returns the stored reference; copy or use an immutable policy if callers must not mutate it.

## Do this today
1. Create Flags<{title:string; description:string}> for form touched state.
2. Use Awaited<ReturnType<typeof listTasks>> rather than inventing a complex extractor.
3. Build a generic repository exercise and explain the reference-mutation limitation.
4. Write a format overload for Date versus number, then compare with a union implementation.
5. Stretch: inspect conditional-type distribution over unions and learn how tuple wrapping changes it.

## Check your work
- [ ] You can explain each advanced type in plain language.
- [ ] You choose built-in helpers when they suffice.
- [ ] You understand compile-time private versus runtime #private.
- [ ] Advanced types simplify a real contract rather than just making it impressive.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Mapped Types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html) · [Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html) · [Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
