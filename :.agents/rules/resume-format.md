---
description: Resume-only format spec. Overrides knowledge/resume_standards.md. Summary word cap, ALL CAPS names, no em-dashes or semicolons, two static education formats, TNR spec. Never applied to cover letters — see scope guard.
globs: ["resume-outputs/p02/*_draft.md", "resume-outputs/p03/*_final.md"]
---

<!-- The globs above also match *_cover_letter_draft.md / *_cover_letter_final.md
     because simple wildcards can't exclude them. The Scope guard below makes this
     safe: on any cover-letter file, this rule self-disables. -->


# Resume Format

Overrides `@knowledge/resume_standards.md` for this project.

**Scope guard — resume files only.** This rule applies ONLY to the resume draft
(`*_draft.md`) and final resume (`*_final.md`). It does **not** apply to any file
whose name contains `cover_letter`, even if this rule loads alongside one because
of a loose glob match. If the file being written is a cover letter, ignore every
rule below and defer to `.agents/rules/cover-letter-voice.md`. The 50-word summary
cap, ALL CAPS institution rule, and Education Format A/B are resume-only and must
never be imposed on a cover letter.

## Summary — hard cap at 50 words

Cap the resume summary at **50 words MAX**. Do not pad to hit the cap — a sharper
30-word summary beats a bloated 50-word one. If you can't fit the useful thing in
50 words, cut the least-differentiating clause until you can. No exceptions for
JD keyword pressure or pivot setup — those go into bullets, not the summary.

The bank's Section 5.5 summary variants are **not** pre-trimmed to this cap
(STREAMING runs ~60 words, MKTG ~51, STRATEGY ~50). Use the 5.5 variant as raw
material, then cut to 50 or fewer. The shorter summaries baked into the Section
5.6 base resumes (17–40 words) are already compliant and are the better starting
point when they fit the lane. Count the words before writing the file.

## Company and school names — ALL CAPS

Employer names and school names are ALL CAPS wherever they appear (header,
Experience, Education). Divisions (`Carlson School of Management`), locations,
and degree names stay in title case. Only the top-level institution name is caps.

## Punctuation — no em-dashes, no semicolons

**Zero em-dashes (`—`) and zero semicolons (`;`)** anywhere on the resume. Break
into two sentences, use a comma, use parentheses, or restructure. Hyphens (`-`) in
compound words (`cross-functional`, `data-driven`) are fine. Hyphens as sentence
connectors (` - `) are not — restructure instead.

**The bank's source content already contains these characters — you must scrub
them on the way in.** The Section 5.6 base resumes each carry 8–9 em-dashes and
3–4 semicolons (in the summary, the role header lines like `REVRY — Data Analytics
Intern — Glendale, CA`, and inside bullets). The Section 5.5 summary variants and
the Section 2 canonical header/education block do too. Copying any of these
forward verbatim will fail the resume. When you pull base-resume content into the
draft:

- Replace em-dashes in role/header lines and education lines with commas
  (`REVRY, Data Analytics Intern, Glendale, CA (remote)`; `UNIVERSITY OF
  MINNESOTA, Carlson School of Management, Minneapolis, MN`).
- Replace em-dashes and semicolons inside summary and bullet prose by splitting
  into two sentences or joining with a comma or `and`.

This is a mechanical character substitution, not a rewording of a bank fact — it
does not violate the bank rules in `.agents/rules/bank-facts.md`. Do a final
character sweep before writing the file: zero `—`, zero `;`, zero ` - `.

## Formatting spec (Pass 3 audit checklist)

- Times New Roman throughout
- Body 10.5 pt (10 pt minimum)
- Name 14 pt, bold
- Section headers bold, ALL CAPS, 11–12 pt
- Horizontal rule under the contact line
- Dates right-aligned via a borderless 2-column table row — never tab stops
- Margins 0.5"–0.75"
- Standard round `•` bullets, single style
- Single column, no text boxes, no graphics, no columns
- Section order: **SUMMARY → SKILLS → EXPERIENCE → EDUCATION** (Skills before
  Experience overrides the generic order in resume_standards.md §2)
- Consistent date format across all entries
- No bullet exceeds 2 lines

## Education — two static formats, no ad-hoc variants

**Format A (default — visually preferred):**

```
UNIVERSITY OF MINNESOTA, Carlson School of Management, Minneapolis, MN
Master of Business Administration                                             May 2026

NORTH CENTRAL UNIVERSITY, Minneapolis, MN
Bachelor of Arts, American Sign Language/English Interpreting                April 2021
magna cum laude
```

- Line 1: `INSTITUTION NAME` (bold, ALL CAPS), division (title case, optional),
  `City, State`
- Line 2: degree (bold), date right-aligned via a borderless 2-column table row
- Line 3 (optional): honor (`magna cum laude`), lowercase italics, no bold
- Blank line between schools

**Format B (compact — use only when Format A pushes the resume over one page):**

```
UNIVERSITY OF MINNESOTA, Carlson School of Management, Minneapolis, MN, MBA                                 May 2026
NORTH CENTRAL UNIVERSITY, Minneapolis, MN, BA American Sign Language/English Interpreting, magna cum laude  April 2021
```

- One line per school. Institution, division, location, degree (and honor if any),
  comma-separated. Date right-aligned in the same table row.
- No blank line between schools.

Use Format A by default. Switch to Format B only after every other length lever
(bullet trims, moving content to the cover letter, dropping a role bullet) is
exhausted.

## Pass 3 output requirements

- Include a Build spec table (font, sizes, margins, bullet style, dates via table
  cells, export format) for Josh to apply when typesetting in Google Docs.
- Suggest the final PDF filename per `.agents/rules/naming-convention.md`:
  `Ekong_Joshua_Resume_{CompanyShortName}_{Year}.pdf`.
- Flag the manual Google Docs export hygiene step for Josh (disable smart
  substitutions, straighten quotes/hyphens) — do not attempt it.
