---
description: Per-pass skill invocation, inputs, outputs, and gates for the resume tailoring workflow. Reference — consult when unclear which pass owns which step.
globs: []
---

# Pass Sequence

Literal invocation prompts are in `@PROMPTS.md`. This file is the structural
reference — what each pass reads, writes, and requires.

## Prompt 0 — Log a new JD

Runs before Pass 1, every application.

- **Skill:** none (mechanical logging)
- **Writes:** `job-descriptions/{slug}-{slug}.md` (new), `job-descriptions/{slug}-
  company_notes.md` (new or appended — see `.agents/rules/write-permissions.md`)
- **Rules loaded:** naming-convention, write-permissions
- **Model/effort:** see `.agents/rules/model-effort.md`

## Pass 1 — JD Intake & Match Analysis

- **Skill:** `job-description-analyzer`
- **Reads:** `job-descriptions/{slug}.md`, `@raw-resume-docs/master_experience_
  bank.md`, `@knowledge/target_lane_rubrics_and_bullets.md`
- **Writes:** `resume-outputs/p01/{slug}_analysis.md`, ending in a numbered
  `## Open Questions for Josh` section plus a blank numbered answer list at the
  bottom (see `.agents/rules/checkpoint.md`)
- **Read-only elsewhere.** Does not draft resume content.
- **Gate for next:** Josh fills the numbered answer list. Blocks Pass 2.

## Pass 2 — Tailored Rewrite

- **Skills:** `resume-tailor`, `resume-bullet-writer`, `resume-quantifier`,
  `resume-section-builder`. Conditionally: `career-changer-translator` (only if
  Josh authorized pivot framing), `tech-resume-optimizer` (only STREAMING-lane JDs
  with heavy technical requirements). The cover letter draft (if needed) is
  assembled directly from the bank's Section 5.7 component bank plus
  `@knowledge/cover_letter_standards.md` — not from the generic
  `cover-letter-generator` skill, which pulls its own templates and would bypass
  Josh's component bank. `cover-letter-generator` stays in scope as a fallback
  only, never the default.
- **Reads:** bank, `knowledge/*` **excluding** `writing_style_profile-josh_v2.md`,
  the numbered answers in the `p01` file
- **Writes:** `resume-outputs/p02/{slug}_draft.md`, `_cover_letter_draft.md`
  (if applicable), `_changelog.md`
- **Changelog required subsections:** Rule-compliance checks (walk through the
  bank-facts and resume-format rules explicitly), Open items carried to Pass 3
- **Cover letter draft required footer:** Verification notes tracing every claim
  to its bank source or checkpoint-sourced willingness statement
- **Rules loaded:** checkpoint, bank-facts, resume-format
- **Gate to run:** complete numbered answer list in the `p01` file

## Pass 3 — ATS/Format Audit + Cover Letter Voice Pass

Two parallel tracks.

**Resume track:**
- **Skills:** `resume-ats-optimizer`, `resume-formatter`
- **Reads:** Pass 2 draft, `@knowledge/ats_ai_screening.md`,
  `@knowledge/resume_standards.md` as overridden by
  `.agents/rules/resume-format.md`
- **Do not apply** `writing_style_profile-josh_v2.md` to the resume at any point.
- **Output:** `resume-outputs/p03/{slug}_final.md` with Build spec table and
  suggested PDF filename

**Cover letter track** (only if the Pass 2 cover letter draft exists):
- **No dedicated skill** — apply `@knowledge/writing_style_profile-josh_v2.md` on
  top of `@knowledge/cover_letter_standards.md`
- **Rules loaded:** cover-letter-voice
- **Output:** `resume-outputs/p03/{slug}_cover_letter_final.md` with
  Writing-profile self-check and Fact / rule check subsections

**Also writes:** `resume-outputs/p03/{slug}_tailoring_notes.md`

## Escalation

If a JD requirement can't be honestly matched to bank content and Josh didn't
supply a fact at the checkpoint, the final output states the gap plainly. Do not
hedge with vague language. Do not let Pass 3 polish over an unresolved gap.

For model/effort escalation (a different kind of escalation, decided between
runs), see `.agents/rules/model-effort.md`.
