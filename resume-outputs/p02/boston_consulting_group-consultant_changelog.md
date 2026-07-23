# Pass 2 Changelog — Boston Consulting Group, Consultant (Experienced Hire)

## Checkpoint gate

`resume-outputs/p01/boston_consulting_group-consultant_analysis.md` numbered answer
list checked before starting: 6 OQs, 6 answers, no blank positions. Gate passed.

**Slug note:** the invocation used `role_slug=consulting`, but Pass 1 already
established `boston_consulting_group-consultant` as the slug (matching the JD's
"Consultant" title). Kept `consultant` for this pass so the p01→p02→p03 file chain
stays intact under one slug, per `naming-convention.md` ("never reuse a slug" — the
concern there is collision, not that the slug can drift mid-application). Flagging in
case `consulting` was intentional and a rename is wanted before Pass 3.

## Inputs used

- Lane: STRATEGY (OQ-1, confirmed no objection)
- Base: 5.6.C (Consulting/Strategy), used as-is per OQ-1
- No new bank-content facts (OQ-2 = N/A, OQ-6 = N/A) — nothing added to
  `checkpoint_log.md`, nothing needed
- Logistics/willingness (OQ-3): open to relocating to Chicago, San Diego, or
  Washington, D.C.; MBA confirmed complete May 2026 (current date July 2026, clears
  before any realistic start); yes to international travel, extended hours, and
  driver's-license/passport status
- Pivot framing (OQ-4): "Minimum pivoting used in the resume but acknowledge it" —
  applied as a single bullet swap, not a Section 6 narrative (see Rule-compliance below)
- Cover letter (OQ-5): yes, drafted

## Rule-compliance checks

### `bank-facts.md`

- **Bullet pool constraint:** every resume bullet is either an unmodified bank Section
  4 bullet or a direct reword of one (keyword fit only — no number, scope, or
  ownership change). Specifically: the Etsy portfolio bullet was reworded to surface
  "wide range of clients and projects" without touching the 12-client count or 30%
  figure; the Carlson bullets had semicolons replaced with commas only (format
  scrub, not a fact edit); the ASL Professional bullet is bank 4.4b bullet-1 verbatim
  in substance, reframed in wording only.
- **Annotation vocabulary:** no `[status: estimated]` bullets used (the only estimated
  bullet in the bank, M16/the resource-cataloging 40%, was not selected). No `RETIRED`
  content used (the "most-requested course" superlative never appears).
- **Section 1.3 hard rules:** checked individually —
  - No SQL attributed to any Revry bullet. ✅ (neither Revry bullet mentions SQL)
  - No invented VPN discrepancy-reduction percentage. ✅ (VPN bullet stays qualitative)
  - M11/M27 not merged. ✅ (neither appears on this resume — Etsy bullets used here
    are the portfolio and 6-month-engagement variants, not the SEO/engagement pair)
  - CMO/VP is the headline audience on both Carlson presentation mentions (resume
    bullet and cover letter); C-suite/director-level language never used. ✅
  - M5 adoption stated as a fact only, no % or $, for both the Little Tikes and DNP
    halves. ✅
- **No two variants of the same fact:** confirmed the Etsy "portfolio of 12 concurrent
  clients" bullet is used alone; the "Advised 12 Etsy sellers" variant (5.6.B) does
  not appear anywhere on this resume. Confirmed the ASL Professional entry uses only
  the merged bullet-1 variant (teaching + interpreting) — bullet-2 (Salesforce/
  engagement-80%) was dropped entirely rather than run alongside it, so no duplicate
  ASL/Fusion fact pair exists on this resume.
- **Cover letter fact traceability:** see the Verification notes table in the cover
  letter draft — every claim traces to an M-number, a bank section, the JD text
  (for BCG-descriptive language, not a claim about Josh), or a checkpoint answer
  (labeled as such).

### `resume-format.md`

- **Summary cap:** drafted from the Section 5.5 STRATEGY variant (50 words as
  written, contains a semicolon), trimmed and rewritten to 47 words, semicolon
  removed. Counted by hand before finalizing (see word-by-word count in the draft
  file). ✅ under 50.
- **ALL CAPS institution names:** JOSHUA EKONG (name, per header convention),
  UNIVERSITY OF MINNESOTA, CARLSON BRANDS ENTERPRISE, REVRY, ETSY EXPERT, ASL
  PROFESSIONAL, NORTH CENTRAL UNIVERSITY all caps; divisions ("Carlson School of
  Management"), locations, and degree names stay title case. ✅
- **Em-dash / semicolon scrub:** final character sweep on the resume draft — zero
  `—`, zero `;`, zero ` - `. Specifically converted: role-header em-dashes to commas
  (`REVRY, Data Analytics Intern, Glendale, CA (remote)`; same pattern for Carlson,
  Etsy, ASL Professional); the two semicolons inside Carlson bullets 1 and 4 (from
  the bank's 5.6.C copy) replaced with `, with the majority...` constructions; the
  Section 5.5 summary's semicolon removed by restructuring into two sentences. En
  dashes in date ranges (`Jan 2025 – Dec 2025`) are a distinct character from
  em-dash and were left as-is, matching the bank's own date-range convention. Note:
  this scrub was **not** applied to the cover letter, per the resume-format scope
  guard (cover-letter files are explicitly out of scope for this rule).
- **Education format:** Format A (default) used, both schools, blank line between
  them, honor line lowercase italics/no bold.
- **Section order:** SUMMARY → SKILLS → EXPERIENCE → EDUCATION, as required.
- **Company/school names, dates:** consistent format across all entries; no bullet
  exceeds 2 lines at 10.5pt in the source text (Pass 3 to confirm against actual
  typeset line wraps).

## Open items carried to Pass 3

- **Length/page-fit risk:** this draft carries 4 Carlson bullets + 2 Revry + 2 Etsy +
  1 ASL bullet (9 total), one more bullet than some other lane bases run at this
  length. Pass 3 should confirm one-page fit at 10.5pt before finalizing; if it runs
  long, the DNP bullet (Carlson #4) is the natural cut since OQ-2 flagged it as the
  "second case" breadth bullet rather than a differentiating metric, and Format B
  (compact education) is the next lever if bullet trims aren't enough.
- **Hiring-manager name:** this pass had no ability to search for a named contact
  (read-only against knowledge/raw-resume-docs/job-descriptions); the cover letter
  uses "Dear Hiring Manager" per the standards' fallback rule. Pass 3 or Josh should
  do a genuine name search (LinkedIn, BCG team page) before this goes out, and swap
  the salutation if a name turns up.
- **Company-notes gap:** `boston_consulting_group-company_notes.md` has no
  free-form content ("None"). The cover letter's required company-specific detail
  was pulled from the JD text instead. If Josh has BCG-specific notes (a contact, an
  info session, a specific practice area of interest) that were never logged, adding
  them to that file would strengthen the letter's second body paragraph.
- **Logistics completeness in the letter:** driver's-license/passport status
  (confirmed "yes" in OQ-3) was left out of the letter body as a minor operational
  detail. Flagging in case Josh wants every JD logistics item explicitly addressed
  rather than the higher-signal subset (travel, hours, relocation, degree timing).
- **Pivot-framing minimality:** confirmed only one bullet (ASL Professional) carries
  any pivot-adjacent framing; nothing in the Summary, cover letter, or other bullets
  references the ASL/teaching-to-analytics narrative. Pass 3 should sanity-check that
  this reads as "acknowledged, not leaned on" per OQ-4, and dial it back further (or
  revert to the stock 5.6.C bullet) if it reads as more pivot-forward than intended.
- **Slug mismatch:** flagged above — used `consultant` (matching Pass 1) instead of
  the invocation's `consulting`. Confirm before Pass 3 if a rename across all three
  passes is actually wanted.
