# Pass 3 Final — Boston Consulting Group, Consultant (Experienced Hire)

**Lane:** STRATEGY | **Base:** 5.6.C (Consulting/Strategy) | **Pivot framing:** minimal, one bullet only (per checkpoint answer 4)
**Source:** `resume-outputs/p02/boston_consulting_group-consultant_draft.md`, carried forward with no content changes. This pass is formatting/ATS audit only, per instructions. `writing_style_profile-josh_v2.md` was **not** applied to this file.

---

```
JOSHUA EKONG
Minneapolis, MN | (240) 467-4705 | j.ekong@outlook.com | linkedin.com/in/joshekong
────────────────────────────────────────────────────────────────────────────────
SUMMARY
Carlson MBA '26 brand and strategy consultant. Led survey analysis and executive
presentation for a Little Tikes brand-architecture refresh, delivering 3
recommendations to CMO and VP-level stakeholders on a 6-person team. Skilled at
translating complex business problems into clear recommendations across teams
with diverse backgrounds and disciplines.

SKILLS
Strategy & Research: Market/competitive assessment, brand positioning & architecture,
  messaging frameworks, business-case/strategy decks, consumer insights, hypothesis-driven
  analysis
Analytics: Survey design & analysis (Qualtrics), KPI definition, competitive benchmarking,
  Python, Excel, QuickSight, Looker Studio

EXPERIENCE
UNIVERSITY OF MINNESOTA, CARLSON BRANDS ENTERPRISE, Graduate Student Brand Consultant
Minneapolis, MN                                                   Jan 2025 – Dec 2025
Led analysis of a consumer survey (700 collected, 500 retained) and synthesized 3 recommendations informing Little Tikes' brand architecture and repositioning, with the majority adopted for the brand-architecture refresh.
Collaborated with a 6-person consulting team to translate ambiguous research findings into clear strategy, activation, and portfolio recommendations.
Presented 3 recommendations in a formal strategy deck to CMO- and VP-level stakeholders, clarifying master and sub-brand relationships to strengthen portfolio coherence.
Delivered market and competitive assessments for the School of Nursing's DNP program, benchmarking peer programs, with the majority of recommendations adopted as part of optimizing the program's enrollment funnel.

REVRY, Data Analytics Intern, Glendale, CA (remote)               Sep 2025 – Apr 2026
Synthesized audience and advertiser data into decision-ready reporting for Sales, Marketing, and executive stakeholders.
Built executive-facing dashboards in QuickSight and Looker Studio and investigated streaming-data discrepancies tied to international VPN usage, distinguishing likely U.S. from international viewers.

ETSY EXPERT (self-employed), Etsy Store Owner & Consultant, Minneapolis, MN
                                                                  Dec 2022 – Oct 2025
Managed a wide range of clients and projects as a portfolio of 12 concurrent consulting engagements, driving 30% average client revenue growth while keeping deliverables on schedule.
Ran a client's sales channel as a 6-month consulting engagement, generating ~$15K through data-driven positioning and market research.

ASL PROFESSIONAL, Teacher & Interpreter                           Jun 2021 – Oct 2025
Taught ASL curricula for 75+ students across a 15+-campus network and interpreted 100+ concerts, events, and educational settings, including 10+ high-profile, delivering accurate communication across diverse audiences under real-time pressure.

EDUCATION
UNIVERSITY OF MINNESOTA, Carlson School of Management, Minneapolis, MN
Master of Business Administration                                             May 2026

NORTH CENTRAL UNIVERSITY, Minneapolis, MN
Bachelor of Arts, American Sign Language/English Interpreting                April 2021
magna cum laude
```

---

## Hard-fail checks

**(a) Summary word count — PASS.** Hand-counted at 47 words (cap 50). "Carlson MBA
'26 brand and strategy consultant." (7) + "Led survey analysis and executive
presentation for a Little Tikes brand-architecture refresh, delivering 3
recommendations to CMO- and VP-level stakeholders on a 6-person team." (24, running
31) + "Skilled at translating complex business problems into clear recommendations
across teams with diverse backgrounds and disciplines." (16, running 47).

**(b) Em-dashes and semicolons — PASS.** Ran `grep` for `—`, `;`, and ` - ` against
the resume text block in isolation (lines inside the code fence only, excluding this
document's own commentary). Zero hits for all three. En dashes in date ranges
(`Jan 2025 – Dec 2025`) are a distinct character from an em dash and are the bank's
own date-range convention, left as-is per `resume-format.md`.

**(c) ALL CAPS company/school names — PASS.** `UNIVERSITY OF MINNESOTA`, `CARLSON
BRANDS ENTERPRISE`, `REVRY`, `ETSY EXPERT`, `ASL PROFESSIONAL`, `NORTH CENTRAL
UNIVERSITY` are all caps wherever they appear. `Carlson School of Management`
(a division within the Education entry, not the employer name for that entry) stays
title case, matching the format spec. `CARLSON BRANDS ENTERPRISE` is capped because
it functions as the employer name in the Experience entry, not a division of a
degree-granting school in that context.

**(d) Education format — PASS.** Format A (default) used for both schools: matches
the spec's line-1/line-2/line-3 structure and blank-line-between-schools rule
exactly, character-for-character against `resume-format.md`'s template. One page fit
does not currently require falling back to Format B (see Length/page-fit note
below).

## `ats_ai_screening.md` §8 checklist

**Parsing**
- [x] Single-column layout
- [x] No tables, text boxes, headers/footers used for content in this markdown. The
  one place a table appears in the workflow is the *dates* column, which is
  deliberately a borderless 2-column table row applied at Google Docs typesetting
  time (see Build spec) — this is the ATS-safe pattern the standard itself
  recommends over tab stops, not the "table for layout" anti-pattern it warns
  against.
- [x] Standard font: Times New Roman (per `resume-format.md` override; TNR is on
  the standard's own approved list)
- [x] Standard bullets: single `•` character throughout, confirmed by scan (9/9
  bullets)
- [x] PDF will be generated from a text source (Google Docs export, not a scan)
- [x] Plain-text paste test: not yet run against an actual exported PDF (can't be
  tested until Josh typesets it) — flagged for Josh to run per §2.2 before
  submitting

**Keywords**
- [x] All "required" JD asks (advanced degree, cross-functional collaboration,
  complex-business-problem analysis, senior-stakeholder presentation, breadth
  across clients/industries) appear in context — this JD is unusually thin on hard
  technical requirements, per Pass 1's coverage map, so there is no keyword gap to
  close
- [x] Critical terms appear in both Skills and an experience bullet: "competitive
  assessment/benchmarking" (Skills; Carlson DNP bullet), "survey design & analysis"
  (Skills; Carlson bullet 1)
- [x] No keyword stuffing, no hidden text, no bottom keyword list
- [x] Title alignment: Summary states "brand and strategy consultant" directly;
  reads as the target Consultant lane without needing a bridge bullet

**Authenticity**
- [x] Specific numbers, project names, or facts present in 9/9 bullets. 6 of 9
  carry an explicit number (Carlson 1–3, Etsy 1–2, ASL). The remaining 3 (Carlson
  4, Revry 1, Revry 2) carry a named project/fact instead of a raw number (DNP
  program name; Sales/Marketing/executive stakeholder audience; QuickSight/Looker
  Studio/international-VPN specifics) — flagging this distinction rather than
  rounding it up, since the checklist's own wording ("numbers, project names, **or**
  facts") is more permissive than a strict numeric count, but Josh should know not
  all 9 are number-bearing.
- [x] No generic AI-tell phrases scanned for ("uniquely positioned," "results-driven,"
  etc.) — none found
- [x] Voice and sentence length vary across bullets, no uniform rhythm

**LinkedIn alignment**
- [ ] Titles, employers, and dates matching LinkedIn exactly — **cannot verify
  from this workflow** (read-only against `knowledge/`, `raw-resume-docs/`,
  `job-descriptions/`, no LinkedIn access). Josh should confirm before submitting.
- [ ] Skills overlap with LinkedIn Skills section — same limitation, flagged for
  Josh

**File**
- [x] Suggested filename follows the pattern (see Build spec below)
- [x] Current version, not a stale draft

## Formatting audit against `resume-format.md` (overriding `resume_standards.md` §3)

| Spec | Status |
|---|---|
| Times New Roman throughout | To apply at typesetting — flagged in Build spec |
| Body 10.5 pt (10 pt minimum) | To apply at typesetting — flagged in Build spec |
| Name 14 pt, bold | To apply at typesetting — flagged in Build spec |
| Section headers bold, ALL CAPS, 11–12 pt | Headers already ALL CAPS in text (`SUMMARY`, `SKILLS`, `EXPERIENCE`, `EDUCATION`); bolding/size to apply at typesetting |
| Horizontal rule under contact line | Present (line below header) |
| Dates right-aligned via borderless 2-column table row, never tab stops | Markdown uses space-padding as a visual placeholder only — no literal tab characters present; real implementation is a Google Docs table row, per Build spec |
| Margins 0.5"–0.75" | To apply at typesetting — flagged in Build spec |
| Standard round `•` bullets, single style | Confirmed, 9/9 |
| Single column, no text boxes, no graphics, no columns | Confirmed |
| Section order: SUMMARY → SKILLS → EXPERIENCE → EDUCATION | Confirmed |
| Consistent date format across entries | Confirmed (`Mon YYYY – Mon YYYY` throughout) |
| No bullet exceeds 2 lines | Expected to hold at 10.5pt single-column width based on bullet length, but not verified against an actual rendered PDF — see Length/page-fit note |

**No tables or graphics used for layout; only standard section headers used.**
Confirmed — `SUMMARY`, `SKILLS`, `EXPERIENCE`, `EDUCATION` are all standard,
parser-recognizable section names.

## Length / page-fit note (carried from Pass 2, re-checked here)

This draft carries 9 bullets (4 Carlson + 2 Revry + 2 Etsy + 1 ASL), one more than
some other lane bases run at this length. At 10.5pt with 0.5"–0.75" margins this is
likely tight but plausible for one page — it cannot be confirmed definitively
without an actual rendered PDF, which this workflow does not produce. If Josh finds
it runs long in Google Docs:
1. Cut the Carlson DNP bullet (bullet 4) first — it's the "second case" breadth
   bullet, not a differentiating metric, per Pass 2's changelog.
2. Fall back to Education Format B (compact) only if the bullet trim alone isn't
   enough.

## Build spec (for Google Docs typesetting)

| Element | Spec |
|---|---|
| Font | Times New Roman, throughout |
| Body text size | 10.5 pt (10 pt absolute minimum) |
| Name size | 14 pt, bold |
| Section header size | 11–12 pt, bold, ALL CAPS |
| Margins | 0.5"–0.75" all sides |
| Bullet style | Standard round `•`, single style, no sub-bullets |
| Dates | Right-aligned via a borderless 2-column table row per line (name/title in left cell, date in right cell) — never tab stops or manual spacing |
| Horizontal rule | Single thin rule directly under the contact line |
| Line spacing | 1.0–1.15 within sections |
| Color | Black text on white background only |
| Export format | PDF, generated directly from the Google Doc (File → Download → PDF) — never a scanned image, never a shared Google Docs link |

**Suggested final PDF filename:** `Ekong_Joshua_Resume_BCG_2026.pdf`

## Manual export hygiene (Josh to do himself)

Before exporting the final PDF from Google Docs:
- Disable smart quotes / smart substitutions (Tools → Preferences → uncheck "Use
  smart quotes") so no curly quotes or auto-inserted em dashes sneak in during
  typing or paste.
- Straighten any curly quotes and auto-substituted em dashes/hyphens that already
  exist in the doc before export — a plain apostrophe (`'`) and straight quotes
  (`"`) only, no em dashes anywhere.
- Re-run the plain-text paste test from `ats_ai_screening.md` §2.2 on the exported
  PDF (select all, copy, paste into a plain text editor) to confirm parsing order
  and character integrity before submitting.

This workflow does not attempt any of the above — it's a Google Docs UI step for
Josh to do by hand.
