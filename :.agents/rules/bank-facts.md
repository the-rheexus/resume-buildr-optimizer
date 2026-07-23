---
description: Bank as the sole source of factual claims. Bullet pool constraint, annotation vocabulary, hard rules, no duplicate variants. Load when composing any Pass 2 or Pass 3 resume or cover letter output.
globs: ["resume-outputs/p02/**", "resume-outputs/p03/**"]
---

# Bank & Facts

## The bank is the only source

`@raw-resume-docs/master_experience_bank.md` is the only source of factual claims
for any resume or cover-letter output. Never invent, round, estimate, or embellish
a number, scope, ownership, or claim.

## Bullet pool constraint

Every bullet on a resume must already exist in the bank's Section 4 bullet pools,
or be a direct, substance-unmodified combination of bank facts. Rewording for
keyword fit is fine. Changing numbers, scope, or ownership language (`led` vs.
`partnered`) is not.

## Annotation vocabulary

The bank uses `[status: verified]`, `[status: estimated]`, and `RETIRED`, plus the
hard rules in Section 1.3.

- `[status: estimated]` bullets — if used at all, must be labeled as estimates in
  the changelog and tailoring notes. Prefer verified alternatives when available.
- `RETIRED` bullets — never used.
- `[needs-check]` is not a live bank tag. If this workflow raises a new open item
  or conflict, flag it as an OQ in the Pass 1 analysis, not as a bank annotation
  to search for.

## Section 1.3 hard rules (non-overridable, ever)

Absolute — JD keyword pressure never justifies breaking these:

- No SQL attributed to any Revry bullet.
- No invented discrepancy-reduction percentage.
- M11 (35% organic traffic, SEO) and M27 (55% engagement, interactions/clicks/
  likes) are separate metrics — never merged into one bullet.
- CMO/VP is the headline audience for Carlson presentation bullets. C-suite is a
  secondary detail. `director-level` is never used.
- M5 adoption claims are usable as an adoption fact only — no invented % or $.

## No two variants of the same fact

Section 4 of the bank flags variant pairs of the same underlying fact (e.g. the
Etsy `advised 12 sellers` line vs. the `portfolio of 12 concurrent clients` line).
Never place both on the same resume — pick one per lane. Check the bank flags
before finalizing.

## Cover letter fact traceability

Every claim in a cover letter must trace to a bank fact (with M-number or section
reference) or a checkpoint-sourced willingness statement (e.g. `relocating to
Chicago`), labeled as such in the Verification notes / Fact-rule check subsection.

## Gap handling

If a JD requirement can only be matched by stretching a bank fact, don't stretch —
flag it as a gap in Pass 1. Distinguish two kinds of gaps:

- **Bank-content gap** — nothing in the bank supports the requirement.
- **Logistics/willingness gap** — relocation, travel, timing, credentials. Not
  resume content and not bank-sourceable either way.

Flag both as OQs. Don't conflate them, and don't treat a logistics answer as
license to add an unsupported resume claim.
