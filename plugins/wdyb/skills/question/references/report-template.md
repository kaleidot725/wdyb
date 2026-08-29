# wdyb #123 — Add retry to API client

[owner/repo #123](https://github.com/owner/repo/pull/123) · author · main ← feat/retry · 2026-08-29

8 correct / 3 close / 2 wrong / 1 unanswered (14 hunks)

## Summary

You consistently catch added error handling, but tend to miss changes in the
ordering of async operations.

## Hunks

### 2. `src/api/client.ts` `@@ -40,3 +48,9 @@` — ⚠️ Close

```diff
@@ -40,3 +48,9 @@ export class ApiClient {
   async get(path) {
-    return this.fetch(path)
+    for (let i = 0; i < this.retries; i++) {
+      await sleep(2 ** i * 100)
+    }
```

**Your interpretation**

> Adding a retry.

**What it actually does**

Adds a retry *and* an exponential backoff delay between attempts.

**Notes**

The delay comes from `sleep(2 ** i * 100)` at `src/api/client.ts:52`, which the
interpretation does not mention.
