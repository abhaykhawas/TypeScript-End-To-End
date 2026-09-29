# Day 28 — Migrate one existing JavaScript feature

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Choose a small feature from an existing MERN app: profile editing, task filtering, or a student list. Work on a branch and keep existing behavior. Convert leaf utilities first, then API contracts, components, services, and database integration. Preserve the original module system unless you intentionally plan a separate ESM migration.

For incremental checking, allowJs permits JS files in a TS project; checkJs checks JS too. JSDoc can add types before renaming. Avoid emitting over source files: use noEmit for a bundler frontend or a separate outDir for a compiled backend.

Every temporary escape hatch should have a reason and removal task. A renamed .tsx file filled with any is not a successful migration.

## Worked example
```js
// A JS file you can check before renaming
// @ts-check
/**
 * @param {{id: string, name: string}[]} users
 * @param {string} query
 * @returns {{id: string, name: string}[]}
 */
export function searchUsers(users, query) {
  return users.filter(user =>
    user.name.toLowerCase().includes(query.toLowerCase())
  );
}
```
```ts
// After migration
export type UserSummary = { id: string; name: string };
export function searchUsers(users: UserSummary[], query: string): UserSummary[] {
  return users.filter(user =>
    user.name.toLowerCase().includes(query.toLowerCase())
  );
}
```

## Why this works
JSDoc expresses the same input/output contract while the source remains JavaScript. Rename only after configuration and imports are compatible. checkJs can reveal old bugs and library declaration mismatches; fix root causes rather than disabling strict checks globally.

## Do this today
1. Pick one real feature and record its current behavior and edge cases.
2. Add a small boundary type and convert one utility.
3. Convert its API function and one component without changing UX.
4. Keep a migration log listing compiler errors and their fixes.
5. Stretch: use @ts-expect-error only for a deliberate temporary case, documenting why and when it will be removed.

## Check your work
- [ ] The migrated feature behaves as before.
- [ ] Imports and production build work.
- [ ] No global strictness settings were weakened to finish the feature.
- [ ] Temporary escapes are tracked and limited.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Migrating from JavaScript](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
