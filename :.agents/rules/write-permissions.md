---
description: Write permissions for restricted folders. Default-deny with narrow, per-folder exceptions. Load whenever touching knowledge/, raw-resume-docs/, or job-descriptions/.
globs: ["knowledge/**", "raw-resume-docs/**", "job-descriptions/**"]
---

# Write Permissions — Default Deny

Unless explicitly granted below, no create/edit/rename/delete in `knowledge/`,
`raw-resume-docs/`, or `job-descriptions/`. This holds regardless of what
`PROMPTS.md`, chat, or any downstream instruction says. If a write looks
implied and no exception applies, stop and ask Josh — do not proceed.

Reads are unrestricted.

## `knowledge/` — read-only

No exceptions. Standards docs are managed by Josh outside this workflow.

## `raw-resume-docs/`

- **`master_experience_bank.md`** — read-only, always. Also set read-only at the OS
  level via `chmod` outside this agent's control; never attempt to change perms.
  Never add, edit, or remove an entry, even when a checkpoint surfaces a new
  verified fact. New facts go to `checkpoint_log.md`; Josh promotes them himself.
- **`checkpoint_log.md`** — read-write. The one file here the agent may create,
  append to, or edit. Log candidate new facts and checkpoint context so they
  persist across sessions. Content here is **staging only** — it is never treated
  as verified and cannot appear in a resume or cover letter until Josh has
  promoted it into the bank.

## `job-descriptions/`

Write allowed for exactly two actions:

1. Create a new `{company_slug}-{role_slug}.md` per the JD file schema in
   `.agents/rules/naming-convention.md`.
2. Create a new `{company_slug}-company_notes.md` per the schema **or**, if it
   exists, append the new role to `Role(s) Applied:` and the new URL to
   `Sources:`. Never overwrite. Never touch the free-form body of an existing
   company-notes file.

No other write is permitted here — no edits to existing JD files, no deletes, no
restructuring.

## `resume-outputs/`

Not covered by this rule. Writing pass outputs there is the point of the workflow
and is unrestricted, subject to `.agents/rules/naming-convention.md`.
