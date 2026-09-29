# Day 23 — Authentication, JWT verification, and request typing

**Time:** 90–120 minutes. **Audience:** JavaScript-fluent MERN developer.
**Routine:** 20 min understand → 25 min reproduce the example → 45 min exercises → 10 min review.
**Workspace:** Use the practice project for Days 1–12, client for Days 13–18, server for Days 19–25, and both for Days 26–30.

## Understand
Implement registration and login with a User model containing name, normalized email, and passwordHash. Hash passwords with bcryptjs; compare on login. Return a public user DTO constructed explicitly, never the full record. Handle duplicate email races using the database's unique index error as well as any pre-check.

Verifying a JWT checks its signature and configured claims. Decoding alone does not authenticate it. A verified payload still needs shape validation. Use fixed issuer, audience, algorithm, expiry, and a secret from validated configuration.

Augment Express Request with an optional auth property; optionality reflects that some routes run before auth middleware.

## Worked example
```ts
// src/types/express.d.ts
export {};
declare global {
  namespace Express {
    interface Request { auth?: { userId: string } }
  }
}
```
```ts
// src/middleware/auth.ts
import jwt from "jsonwebtoken";
import { z } from "zod";
import type { RequestHandler } from "express";
import { config } from "../config.js";
const Claims = z.object({ sub: z.string().regex(/^[a-f0-9]{24}$/i) });
export const requireAuth: RequestHandler = (req, res, next) => {
  const header = req.headers.authorization;
  if (!header?.startsWith("Bearer ")) {
    res.status(401).json({ message: "Authentication required" }); return;
  }
  try {
    const verified: unknown = jwt.verify(header.slice(7), config.JWT_SECRET, {
      algorithms: ["HS256"], issuer: "taskboard-api", audience: "taskboard-client"
    });
    req.auth = { userId: Claims.parse(verified).sub };
    next();
  } catch {
    res.status(401).json({ message: "Invalid or expired token" });
  }
};
```

## Why this works
Sign login tokens with the same issuer/audience and subject set to the database user id, plus expiresIn: "1h" and algorithm: "HS256". Use jwt.sign({}, secret, options). Declaration merging adds a compile-time property; middleware actually assigns it at runtime. This course uses an in-memory browser token for the demo; it clears on reload. A production auth design needs an explicit session/refresh strategy.

## Do this today
1. Implement validated POST /api/auth/register and /api/auth/login.
2. Create a unique email index and handle a duplicate-key error as 409.
3. Sign a token with subject, issuer, audience, expiry, and algorithm matching verification.
4. Test absent, malformed, tampered, and expired tokens.
5. Stretch: validate that the authenticated user still exists or is active where your application requires it.

## Check your work
- [ ] Plaintext passwords are never stored or returned.
- [ ] Correct credentials return a token and public user.
- [ ] Wrong credentials and invalid tokens return controlled 401 responses.
- [ ] The declaration file is included in server type checking.
- [ ] Explain the example aloud without reading it.
- [ ] Run the relevant type-check command; do not silence errors with `any`.
- [ ] Save your exercise code and a short note about one error you fixed.

## Read after practising
[jsonwebtoken documentation](https://github.com/auth0/node-jsonwebtoken) · [Declaration merging](https://www.typescriptlang.org/docs/handbook/declaration-merging.html)

## Progress rule
Continue when you can complete the core exercises unaided. Stretch tasks are optional. Compiler errors are learning feedback; inspect the value, expected type, and boundary before changing an annotation.
