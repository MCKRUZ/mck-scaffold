### L2-openspec-brownfield — methodology notes

This project uses **OpenSpec** with **Shotgun CLI** indexing (L2 Spec-Anchored, brownfield-first).

**Core insight:** in a brownfield codebase, the spec for any change is a *delta* — what was ADDED, what was MODIFIED, what was REMOVED — not a full re-specification of behavior that already exists.

**Workflow for any change:**

1. **Index first.** If you haven't run Shotgun recently, run `uvx shotgun plan "<what you're adding>"` to refresh the code-graph index. Skipping this step wastes tokens.
2. **Write the delta spec** in `openspec/<feature>/spec.md`. Use the ADDED / MODIFIED / REMOVED format:

   ```
   # Add OAuth2 login flow

   ## ADDED
   - REQ-AUTH-OAUTH-001: WHEN user clicks "Sign in with Google", the system SHALL redirect to OAuth2 authorization endpoint.
   - Files: `src/auth/oauth.ts` (new), `tests/auth/oauth.test.ts` (new)

   ## MODIFIED
   - `src/auth/service.ts:authenticate()`: extend to accept oauth tokens alongside passwords.
   - Existing REQ-AUTH-001 unchanged.

   ## REMOVED
   - (none)
   ```

3. **Implement against the delta.** The spec is constrained to the change; existing behavior is presumed correct unless touched by MODIFIED.

4. **Update the delta spec** if implementation reveals an addendum is needed. Spec and code stay in sync.

**Brownfield discipline rules:**

- Existing code is presumed correct. Don't refactor adjacent code unless it's in MODIFIED.
- New REQ identifiers follow existing domain patterns (look at `openspec/` for prior naming conventions).
- If a change touches more than 5 existing files, that's a signal to split into multiple delta specs.

**When NOT to use the full delta:**

- Pure typo fixes / formatting / comments — direct commit, no delta.
- Adding a unit test for existing behavior — addendum to the existing REQ's test list.
- Anything in a never-to-be-merged spike branch — lean preset for that branch.
