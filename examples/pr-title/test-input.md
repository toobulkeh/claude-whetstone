# Test input: a sample diff

```diff
diff --git a/auth/middleware.ts b/auth/middleware.ts
@@ -12,7 +12,7 @@ export async function requireAuth(req, res, next) {
   const token = req.headers.authorization?.replace('Bearer ', '');
   if (!token) return res.status(401).end();

-  const claims = verify(token);
+  const claims = verify(token, { clockTolerance: 30 });
   if (claims.exp < Date.now() / 1000) {
     return res.status(401).json({ error: 'expired' });
   }
diff --git a/auth/middleware.test.ts b/auth/middleware.test.ts
@@ -45,6 +45,12 @@ describe('requireAuth', () => {
     expect(res.status).toBe(401);
   });
+
+  it('accepts tokens within 30s clock skew', async () => {
+    // ...
+  });
 });
```

Diff intent: add a 30-second clock-skew tolerance to JWT verification to fix
intermittent 401s on clients with mildly out-of-sync clocks. Single concern.
