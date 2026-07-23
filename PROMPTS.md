# PROMPTS.md — Resume Tailoring Prompts (Joshua Ekong)

Run in `resume-buildr/`, in order, against the rules in `AGENTS.md`. Copy-paste each
block as-is into the CLI, filling in the bracketed placeholders. Wait for each pass's
output before running the next — do not batch them.

All writes in these prompts stay inside the permissions granted in
`.agents/rules/write-permissions.md`: narrow create/append rights in `job-descriptions/`, full
write rights in `resume-outputs/`, read-write rights on
`raw-resume-docs/checkpoint_log.md` only, and no write rights anywhere else in
`knowledge/` or `raw-resume-docs/` — `master_experience_bank.md` is never written
by the agent, under any circumstance.

The Human Checkpoint is file-based, not a chat exchange — Josh edits the numbered
answer list at the bottom of the `p01` file directly.

Recommended model/effort per pass is in `.agents/rules/model-effort.md` — set it
before running each prompt block below.

Each prompt is self-contained: it reads everything it needs from disk (the JD,
the bank, the prior pass's output file) rather than relying on what Claude
remembers from earlier in the conversation. Run `/clear` between prompts — same
terminal window is fine, but don't carry one pass's context into the next. This
also means switching `/model` between prompts is safe; nothing depends on
conversational continuity.

---

## Prompt 0 — Log a new JD (run first, every application)

**Recommended Models**
- Claude Code: Haiku 4.5
- Codex: gpt-5.5, low/medium

```
Log a new job description per `.agents/rules/naming-convention.md`.

Company: [Company Name]
Role: [Exact Role Title]
JD URL: [URL]
JD text:
[paste full JD text]

1. Slug the company and role per `.agents/rules/naming-convention.md`.
2. Create job-descriptions/{company_slug}-{role_slug}.md using the JD file schema
   in `.agents/rules/naming-convention.md` (Source, Date pulled, ---, then the
   full JD text).
3. Check whether job-descriptions/{company_slug}-company_notes.md already exists.
   - If not, create it using the company-notes schema in
     `.agents/rules/naming-convention.md`, with this role
     and URL as the first entries and "None" as the body.
   - If it exists, append this role to Role(s) Applied and this URL to Sources.
     Do not overwrite the file or touch the existing free-text body.
4. Tell me the exact company_slug and role_slug you used — every later prompt in
   this sequence needs them.
```

---

## Prompt 1 — JD Intake & Match Analysis

**Recommended Models**
- Claude Code: Sonnet 5, medium
- Codex: gpt-5.5, medium/high

```
Run Pass 1 of the resume tailoring workflow defined in AGENTS.md for
company_slug=[company_slug], role_slug=[role_slug].

Read job-descriptions/[company_slug]-[role_slug].md,
raw-resume-docs/master_experience_bank.md, and
knowledge/target_lane_rubrics_and_bullets.md. Use the job-description-analyzer skill.

Produce resume-outputs/p01/[company_slug]-[role_slug]_analysis.md containing:
1. Lane recommendation (STREAMING / MKTG / STRATEGY) with reasoning
2. Closest base resume to start from (bank Section 5.6.A / 5.6.B / 5.6.C)
3. Coverage map — each JD requirement mapped to the bank bullet(s) that support it,
   or marked GAP if nothing in the bank supports it. Distinguish bank-content GAPs
   (nothing in master_experience_bank.md supports the requirement) from
   logistics/willingness items (relocation, travel, timing, credentials) that
   aren't resume content and can't be sourced from the bank either way — flag
   both, don't conflate them.
4. Any relevant conflicts or open items using the bank's real annotation
   vocabulary ([status: verified] / [status: estimated] / RETIRED / Section 1.3
   hard rules) — the bank doesn't use a literal "[needs-check]" tag today, so
   don't search for one; flag anything that needs my judgment as a new open
   question instead.
5. A numbered "## Open Questions for Josh" list (OQ-1, OQ-2, ...) covering at
   least: lane/base resume confirmation, any content GAPs that need a real fact,
   any logistics/willingness items relevant to this JD, whether to use pivot
   framing (bank Section 6) if relevant, whether a cover letter is needed, and a
   catch-all for any new fact I want considered.

Then end the file with:

---
*Next gate: Human Checkpoint (blocks Pass 2). Awaiting Josh's answers below.*

1. 
2. 
[one blank numbered line per OQ, in the same order]

This is a read-only pass against knowledge/, raw-resume-docs/, and
job-descriptions/ — don't edit or draft any resume content, and don't write
anywhere except the p01 output file. Stop after the file is written. Do not ask me
the checkpoint questions in chat — I'll answer them by editing the file.
```

---

## Human Checkpoint — file-based, not a chat reply

Open `resume-outputs/p01/[company_slug]-[role_slug]_analysis.md`, find "Open
Questions for Josh," and answer each OQ with a short response next to its matching
number in the blank list at the bottom — answer 1 → OQ-1, answer 2 → OQ-2, and so
on, in order. Save the file. Do not reply in chat — Prompt 2 reads the answers
from this section.

If an answer supplies a real new fact not currently in the bank, it's fine to note
that inline. It'll be logged to `raw-resume-docs/checkpoint_log.md` when Prompt 2
runs, and still isn't usable in the resume until I've promoted it into
`master_experience_bank.md` myself, separately.

---

## Prompt 2 — Tailored Rewrite

Run only after the numbered answer list in the `p01` file is filled in.

**Recommended Models**
- Claude Code: Sonnet 5, high
- Codex: gpt-5.5, high
*(multi-skill chain — see `.agents/rules/model-effort.md`)*

```
Run Pass 2 of the resume tailoring workflow defined in AGENTS.md for
company_slug=[company_slug], role_slug=[role_slug].

First, open resume-outputs/p01/[company_slug]-[role_slug]_analysis.md, find "Open
Questions for Josh" and the numbered answers beneath it, and match each answer to
its OQ by position (answer 1 -> OQ-1, answer 2 -> OQ-2, etc.). If the answer list
is missing, shorter than the OQ count, or any position is blank, stop and tell me
exactly which OQ numbers are still unanswered — don't guess or proceed on a
partial set.

Use the resume-tailor, resume-bullet-writer, resume-quantifier, and
resume-section-builder skills. Use career-changer-translator only if the answers
explicitly authorize pivot framing. Use tech-resume-optimizer only if this is a
STREAMING-lane JD with heavy technical/data-stack requirements.

Rules:
- Only use bullets that already exist in master_experience_bank.md Section 4
- Never invent, round, or estimate a number
- Never place two variants of the same underlying fact on one resume
- Reorder bullets within each role by JD relevance, most relevant first
- Integrate 3-5 exact JD keywords in context, not as a list
- Draft the summary from the matched lane's Section 5.5 variant, adjusted to
  mirror JD language, then CUT IT TO 50 WORDS MAX. The 5.5 variants run 50-60
  words as written, so this almost always requires trimming — do not pad, a
  30-word summary is fine. Count the words before writing.
- Scrub banned characters. The Section 5.5 / 5.6 source material contains
  em-dashes and semicolons (role headers like "REVRY — Data Analytics Intern —
  Glendale, CA", summary and bullet prose, and the education block). The resume
  must contain ZERO em-dashes (—) and ZERO semicolons (;). Replace em-dashes in
  header/education lines with commas, and split or comma-join any summary/bullet
  prose that uses them. Do a final character sweep before writing the draft.
- Company names and school names in ALL CAPS; two static Education formats only
  (Format A default, Format B compact) — full spec in
  `.agents/rules/resume-format.md`.
- Do not apply knowledge/writing_style_profile-josh_v2.md here or anywhere in the
  resume — that file is scoped to the Pass 3 cover letter voice pass only
- This is a read-only pass against knowledge/, raw-resume-docs/, and
  job-descriptions/ — write only to resume-outputs/p02/

If the answers confirmed a cover letter is needed, also draft it using
knowledge/cover_letter_standards.md and the bank's Section 5.7 component bank,
using job-descriptions/[company_slug]-company_notes.md for at least one
company-specific detail (read-only reference — do not edit that file here). End
the cover letter draft with a "Verification notes" section tracing every claim to
its bank source (M-number or section), and separately noting any claim that comes
from a checkpoint answer rather than the bank (e.g., a stated willingness like
relocation). This draft does not need to be in my voice yet — that happens in
Pass 3.

Produce:
- resume-outputs/p02/[company_slug]-[role_slug]_draft.md
- resume-outputs/p02/[company_slug]-[role_slug]_cover_letter_draft.md (if applicable)
- resume-outputs/p02/[company_slug]-[role_slug]_changelog.md, including a
  "Rule-compliance checks" subsection walking through the relevant rules in
  `.agents/rules/bank-facts.md` and `.agents/rules/resume-format.md`, and an
  "Open items carried to Pass 3" subsection flagging anything Pass 3 needs to
  verify or resolve (length risk, header/location
  decisions, etc.)
```

---

## Prompt 3 — ATS & Format Audit + Cover Letter Voice Pass

**Recommended Models**
- Claude Code: Sonnet 5, high
- Codex: gpt-5.5, high
*(two parallel tracks, exacting self-check on the cover letter — see `.agents/rules/model-effort.md`)*

```
Run Pass 3 of the resume tailoring workflow defined in AGENTS.md for
company_slug=[company_slug], role_slug=[role_slug].

RESUME TRACK — use the resume-ats-optimizer and resume-formatter skills on
resume-outputs/p02/[company_slug]-[role_slug]_draft.md:
- Run the full knowledge/ats_ai_screening.md Section 8 checklist
- Verify formatting against knowledge/resume_standards.md Section 3 as overridden
  by `.agents/rules/resume-format.md`: Times New Roman, bold all-caps headers,
  horizontal rule under the contact line, dates right-aligned via table cells
  (not tab stops), 10pt minimum, single column, Skills before Experience
- Run these hard-fail checks and fix any violation before writing the final:
  (a) SUMMARY is 50 words or fewer — count them;
  (b) ZERO em-dashes (—) and ZERO semicolons (;) anywhere in the document —
      search the full text, including role header lines and the education block;
  (c) company and school names are ALL CAPS;
  (d) Education uses Format A (or Format B only if the resume would otherwise
      exceed one page) — no ad-hoc variant.
- Confirm no tables/graphics used for layout and only standard section headers
- Do NOT apply knowledge/writing_style_profile-josh_v2.md to the resume, at any
  point in this pass. The resume is formatting/ATS work only — no voice rewrite.
- Include a "Build spec" table (font, sizes, margins, bullet style, dates-via-
  table-cells, export format) for me to apply when typesetting in Google Docs,
  plus the suggested final PDF filename per `.agents/rules/naming-convention.md`:
  Ekong_Joshua_Resume_{CompanyShortName}_{Year}.pdf
- Flag the manual Google Docs export hygiene step for me to do myself (disable
  smart substitutions, straighten quotes/hyphens) rather than attempting it

COVER LETTER TRACK — only if
resume-outputs/p02/[company_slug]-[role_slug]_cover_letter_draft.md exists:
- Rewrite it applying knowledge/writing_style_profile-josh_v2.md's rules and
  guidelines on top of knowledge/cover_letter_standards.md, so the final letter is
  in my voice, professional in tone, and still meets every structural/length
  requirement in cover_letter_standards.md — the style profile adjusts voice, not
  substance or structure.
- Do not introduce any new factual claims in this rewrite — content is locked from
  Pass 2; this pass only changes phrasing/voice/tone.
- Include two subsections directly in the final cover letter file: a
  "Writing-profile self-check" running through writing_style_profile-josh_v2.md's
  own self-check criteria explicitly, and a "Fact / rule check" reconfirming every
  claim still traces to a bank fact (or a checkpoint-sourced willingness
  statement, labeled as such), with nothing new introduced.

Write a tailoring-notes file summarizing keyword coverage, bullets changed, any
open items carried from Pass 2, and whether the cover letter voice pass ran.

This pass reads knowledge/ for standards only — write only to resume-outputs/p03/

Produce:
- resume-outputs/p03/[company_slug]-[role_slug]_final.md
- resume-outputs/p03/[company_slug]-[role_slug]_cover_letter_final.md (if applicable)
- resume-outputs/p03/[company_slug]-[role_slug]_tailoring_notes.md
```

---

## Escalation reminder

If Pass 1 or Pass 2 hits a JD requirement with no bank support and I didn't supply
a fact in the numbered answers, the final output should state that gap plainly
rather than hide it — don't let Pass 3 polish over an unresolved gap. For
model/effort escalation (a different kind of escalation), see
`.agents/rules/model-effort.md`.
