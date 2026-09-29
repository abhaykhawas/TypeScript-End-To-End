# Day 09 — Deriving types and update contracts

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Derive related types instead of copying every field. keyof produces a union of keys. typeof in a type expression obtains the type of a value. Indexed access retrieves a property's type. Pick and Omit select fields; Partial makes selected fields optional; Record models a key-to-value mapping.

Do not blindly use Partial<User> as a public update body. That can expose id, role, or passwordHash. First select fields the operation is allowed to change, then make them optional. These types still need runtime validation at HTTP boundaries.

## Worked example
```ts
type User = {
  id: string;
  name: string;
  email: string;
  role: "admin" | "student";
  passwordHash: string;
};
type PublicUser = Omit<User, "passwordHash">;
type ProfilePatch = Partial<Pick<User, "name" | "email">>;
type Role = User["role"];
const roleLabels: Record<Role, string> = {
  admin: "Administrator", student: "Student"
};
function getProperty<T, K extends keyof T>(object: T, key: K): T[K] {
  return object[key];
}
console.log(getProperty(roleLabels, "admin"));
export {};
```

## Why this works
K must be a real key of T, and the return type reflects that key. Record requires both supported roles here. Omit changes a type only; it does not physically delete passwordHash from a runtime object. Construct a public response explicitly.

## Do this today
1. Create CreateTaskInput and UpdateTaskInput from allowed editable fields.
2. Write toPublicUser(user) that physically excludes passwordHash.
3. Use ReturnType and Awaited to derive an async helper's result type.
4. Create a status-to-label Record and deliberately omit a status.
5. Stretch: compare Pick<User, 'email'> and User['email'].

## Check your work
- [ ] ProfilePatch cannot include role or passwordHash.
- [ ] Public response objects actually exclude secrets at runtime.
- [ ] You understand type derivation versus data transformation.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
