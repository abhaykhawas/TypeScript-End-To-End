# Day 26 — Connect the React client to the typed API

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Replace local storage as the source of truth with authenticated server data. Keep the token in memory for this learning demo, clear it on logout, and document that reloading signs you out. Never put the token in a URL or log it.

Use Vite's development proxy to forward /api to the backend. This keeps relative client URLs and avoids unnecessary dev CORS setup. Production hosting must provide its own /api routing or an intentional API origin and CORS policy.

Expand Day 16's TaskSchema to the complete Day 18 Task shape (description, ownerId, createdAt, updatedAt). Export that schema from a single client contracts module and reuse it in storage and API parsing. Validate successful responses and handle non-success bodies defensively. A delete returning 204 has no JSON body; do not call response.json() on it.

## Worked example
```ts
// Merge into client/vite.config.ts; keep generated plugins
server: {
  proxy: { "/api": "http://localhost:3000" }
}
```
```ts
// client/src/services/api.ts; reuse your complete TaskSchema
export async function listTasks(token: string, signal?: AbortSignal) {
  const response = await fetch("/api/tasks", {
    signal, headers: { Authorization: `Bearer ${token}` }
  });
  if (response.status === 401) throw new Error("Session expired; sign in again");
  if (!response.ok) throw new Error(`Unable to load tasks (${response.status})`);
  const raw: unknown = await response.json();
  return TaskSchema.array().parse(raw);
}
export async function removeTask(id: string, token: string): Promise<void> {
  const response = await fetch(`/api/tasks/${encodeURIComponent(id)}`, {
    method: "DELETE", headers: { Authorization: `Bearer ${token}` }
  });
  if (!response.ok) throw new Error(`Unable to delete (${response.status})`);
}
```

## Why this works
The Vite fragment belongs inside defineConfig's configuration object; it is not a standalone TS file. List data is checked at runtime. Deletion trusts the successful status and reads no body. Model login response as token plus public user and validate that response too.

## Do this today
1. Implement register/login/logout screens and typed auth state.
2. Wire task create, patch, list, and delete through services/api.ts.
3. Remove old local-only persistence or use it only as an explicitly named draft cache.
4. On 401 clear auth state and present the sign-in flow.
5. Stretch: show loading per operation and prevent duplicate submissions.

## Check your work
- [ ] A logged-in user sees only their own tasks.
- [ ] Task changes survive frontend and backend restarts.
- [ ] Delete handles 204 without a JSON parsing error.
- [ ] Network errors, 401, invalid responses, and empty lists have distinct UI behavior.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[Vite server proxy](https://vite.dev/config/server-options.html#server-proxy) · [Zod basics](https://zod.dev/basics)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
