# Day 10 — Literal preservation, satisfies, tuples, and dictionaries

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
as const preserves literal types and marks object properties and array entries readonly in the inferred type; it is not a runtime deep freeze. satisfies checks compatibility while retaining useful inferred information. An ordinary annotation can widen some information.

Tuples model fixed-position values; arrays model variable-length lists. A Record<string, T> is often a dictionary, but runtime keys may be missing. Enable noUncheckedIndexedAccess to make many indexed reads acknowledge undefined. The check is stricter, so add it after your basics are comfortable.

## Worked example
```ts
const statuses = ["todo", "doing", "done"] as const;
type TaskStatus = (typeof statuses)[number];
const labels = {
  todo: "To do",
  doing: "In progress",
  done: "Completed"
} satisfies Record<TaskStatus, string>;

type Coordinates = readonly [latitude: number, longitude: number];
const office: Coordinates = [22.57, 88.36];
console.log(labels.doing, office[0]);
export {};
```

## Why this works
The readonly tuple of statuses becomes a source for the union. Indexed access with number obtains its element union. satisfies catches omitted or invalid keys in the fresh literal. Coordinate position labels document meaning but do not validate latitude ranges.

## Do this today
1. Create roles as const and derive Role from that array.
2. Create a complete role-to-permission map checked using satisfies.
3. Add noUncheckedIndexedAccess: true to practice tsconfig and fix indexed reads.
4. Model a pair with a tuple; compare it with number[].
5. Stretch: add exactOptionalPropertyTypes and study omission versus explicit undefined.

## Check your work
- [ ] Changing statuses updates the derived type.
- [ ] An invalid map key or missing required key is caught.
- [ ] An out-of-range array lookup is handled safely.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
