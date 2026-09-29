# Day 03 — Objects, type aliases, and interfaces

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Model domain data before typing components. An optional property may be absent, so reading it can produce undefined. `readonly` prevents assignments through that typed reference; it does not freeze an object at runtime.

Both interfaces and type aliases can describe objects. Interfaces support declaration merging and extension. Aliases can also represent unions, tuples, and other type expressions. Choose a consistent convention; do not treat either as universally superior.

TypeScript is structural: compatible shapes are accepted. Fresh object literals receive extra-property checks, but object types are not universally exact schemas.

## Worked example
```ts
type Student = {
  readonly id: string;
  name: string;
  email: string;
  mentorId?: string;
};
interface Named { name: string }

function greet(person: Named): string {
  return `Hello, ${person.name}`;
}
const student: Student = {
  id: "s1", name: "Maya", email: "maya@example.com"
};
console.log(greet(student));
console.log(student.mentorId ?? "Unassigned");
export {};
```

## Why this works
`greet` needs only name, so Student is compatible. The nullish fallback handles an absent mentorId. id is readonly through Student. Reassigning id is a compile-time error, while editing name is allowed.

## Do this today
1. Define Product and CartItem; CartItem contains a Product and quantity.
2. Define Institute with contact details and an optional website.
3. Write getStudentLabel(student) and safely handle an absent mentor.
4. Pass an existing object with extra properties to greet, then compare a fresh literal with extra properties.
5. Stretch: show how interface extension and a type intersection can express a shared base shape.

## Check your work
- [ ] The example prints Hello, Maya and Unassigned.
- [ ] Missing required email is rejected.
- [ ] You can explain optional versus nullable properties.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
