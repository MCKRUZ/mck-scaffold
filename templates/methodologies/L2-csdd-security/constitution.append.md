# Security Clauses (CSDD overlay)

> Appended to `docs/constitution.md` by the CSDD security overlay. These are blocking constitutional rules — the `@constitution-enforcer` skill rejects implementations that violate any clause marked **MUST**.
>
> Source: Constitutional SDD (Marri 2026, arXiv:2602.02584). Format adapted from the canonical CSDD reference.
>
> **Tune these for your domain.** The defaults below are a starting set — CWE-89, CWE-79, CWE-798, CWE-502, CWE-310, CWE-22, CWE-352. Add domain-specific clauses (PCI-DSS for payments, HIPAA for health, SOC 2 controls, etc.) before going to production.

---

## SEC-001 (CWE-89) — SQL Injection Prevention

**Level:** MUST
**Rule:** All database queries MUST use parameterized queries or an ORM with auto-parameterization. Direct string concatenation in SQL is PROHIBITED.
**Enforcement:** Static analysis (semgrep rule `sql-injection` or equivalent) runs on every commit.
**Violation:** Reject implementation, require rewrite.

---

## SEC-002 (CWE-79) — Cross-Site Scripting Prevention

**Level:** MUST
**Rule:** All user-controlled data rendered to HTML MUST be output-encoded. `dangerouslySetInnerHTML` (React), `[innerHTML]` (Angular), or equivalent direct-HTML APIs are PROHIBITED unless paired with a vetted sanitizer (DOMPurify or equivalent) and an inline justification comment.
**Enforcement:** Static analysis + code review.
**Violation:** Reject implementation, require sanitization or removal.

---

## SEC-003 (CWE-798) — Hardcoded Credentials

**Level:** MUST
**Rule:** Secrets, API keys, tokens, and connection strings MUST come from a vault or environment variables at runtime. Source-controlled secrets are PROHIBITED. This includes `.env` files (use `.env.example` templates only).
**Enforcement:** Pre-commit secrets-scan hook (gitleaks, trufflehog, or equivalent) configured in `.claude/settings.json`.
**Violation:** Reject commit. If a secret is detected, rotate it immediately and document the rotation in the commit message.

---

## SEC-004 (CWE-502) — Insecure Deserialization

**Level:** MUST
**Rule:** Untrusted input MUST NOT be deserialized into objects without a safe-list or schema validation. `pickle.loads`, `yaml.load` (without `SafeLoader`), `eval`, and equivalent constructs on user input are PROHIBITED.
**Enforcement:** Static analysis (semgrep rule `insecure-deserialization` or equivalent).
**Violation:** Reject implementation, require safe-list deserializer.

---

## SEC-005 (CWE-310) — Cryptographic Failures

**Level:** MUST
**Rule:** Custom cryptographic implementations are PROHIBITED. Use platform-vetted libraries: bcrypt/argon2 for password hashing (cost factor ≥ 12 for bcrypt); AES-GCM for symmetric encryption; ECDSA/RSA-2048+ for signatures. Random number generation for security purposes MUST use cryptographically secure sources (`crypto.getRandomValues`, `secrets.token_bytes`, equivalent).
**Enforcement:** Code review + dependency-list audit.
**Violation:** Reject implementation, require platform-library replacement.

---

## SEC-006 (CWE-22) — Path Traversal Prevention

**Level:** MUST
**Rule:** Any file path constructed from user input MUST be canonicalized and validated against an allowlist of permitted directories before being passed to a file-system API.
**Enforcement:** Static analysis + targeted unit tests covering `..`, absolute paths, and URL-encoded traversal sequences.
**Violation:** Reject implementation, require path validation.

---

## SEC-007 (CWE-352) — Cross-Site Request Forgery Prevention

**Level:** MUST
**Rule:** All state-changing HTTP endpoints (POST / PUT / PATCH / DELETE) MUST validate a CSRF token, use the `SameSite=Strict` cookie attribute, or require an authentication header that browsers do not auto-attach.
**Enforcement:** Integration tests covering each state-changing endpoint.
**Violation:** Reject endpoint, require CSRF protection.

---

## Variance procedure

If a clause genuinely cannot be satisfied (legacy integration, third-party dependency, etc.), produce a Variance ADR:

```
# ADR-NNN: Variance from SEC-XXX

**Status:** Accepted | Superseded
**Date:** YYYY-MM-DD
**Clause:** SEC-XXX (CWE-NNN)

## Context
{why the clause cannot be satisfied}

## Decision
{what compensating control will be used}

## Consequences
{security risks accepted, monitoring added, sunset date for variance}
```

ADR-tracked variances are auditable. Silent variances are violations.
