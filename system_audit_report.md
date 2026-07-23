# RESUME-BUILDR-OPTIMIZER: COMPREHENSIVE SYSTEM AUDIT

**Auditor:** Antigravity (Claude Opus 4.6 Thinking)
**Date:** July 23, 2026
**Scope:** Full read-only audit of architecture, rules, knowledge assets, prompt design, and two completed application outputs (BCG Consultant, KPMG Data Analyst contract)

---

## 1. EXECUTIVE SUMMARY

This is a genuinely well-engineered resume tailoring system. The architecture — a multi-pass, file-based workflow with strict separation between source-of-truth (bank), knowledge (standards), and outputs — is thoughtful and mostly coherent. The master experience bank is the standout asset: meticulously sourced, internally consistent, and honest about the limits of its claims. The two completed outputs demonstrate that the system produces real, compliance-checked, bank-traceable documents, not generic AI output.

That said, the system has three urgent issues. **First**, the KPMG cover letter contains a structural duplicate — two back-to-back Revry descriptions (opening paragraph and body paragraph 1) that repeat substantially the same facts. This was introduced in Pass 2, carried through Pass 3 uncaught, and is not flagged in any changelog or tailoring notes. **Second**, the checkpoint log is effectively dead — it was never written to during either application run, meaning OQ-2 facts from the KPMG run (chi-squared, Cronbach's alpha, Tableau experience, Data Visualization class) are preserved only in the p01 analysis file, not in the persistent staging area they were designed for. **Third**, the `target_lane_rubrics_and_bullets.md` knowledge file is severely truncated — it contains only Section 5.4 and Section 6 of what appears to be a much larger document, meaning the lane rubrics that Pass 1 is supposed to match against (Sections 2.3, 3.3, 4.3 referenced within the file itself) do not exist in the project.

---

## 2. DIMENSION-BY-DIMENSION FINDINGS

---

### DIMENSION 1 — ARCHITECTURE & RULE SYSTEM COHERENCE

---

#### 1.1 — Are all rule files referenced in AGENTS.md and PROMPTS.md, or are any orphaned?

**VERDICT:** WORKING

**FINDING:** All eight rule files in [.agents/rules/](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules) are listed in the rule-file table in [AGENTS.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/AGENTS.md) with their glob triggers. PROMPTS.md explicitly references `write-permissions.md`, `naming-convention.md`, `bank-facts.md`, `resume-format.md`, `cover-letter-voice.md`, and `model-effort.md` by name in the prompt text. `checkpoint.md` is referenced implicitly via the checkpoint mechanic and explicitly in `pass-sequence.md`. `pass-sequence.md` is a reference file (empty globs, loaded on demand), correctly documented as such. No orphaned rule files.

**EVIDENCE:** [AGENTS.md L48-56](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/AGENTS.md) rule-file table; each prompt block in [PROMPTS.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/PROMPTS.md) references the relevant rules inline.

**RECOMMENDATION:** None required. The wiring is clean.

---

#### 1.2 — Do any two rule files contradict each other?

**VERDICT:** WORKING

**FINDING:** The resume-format.md / resume_standards.md override hierarchy is explicitly documented: "Overrides `@knowledge/resume_standards.md` for this project" at [resume-format.md L13](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/resume-format.md#L13). The key divergence — section order (Skills before Experience, vs. the standards doc's Education-first order) — is called out at [resume-format.md L78-79](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/resume-format.md#L78-L79). The scope guard at [resume-format.md L15-21](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/resume-format.md#L15-L21) prevents the resume-only rules from bleeding into cover letter files.

**EVIDENCE:** The scope guard comment at line 6-8 acknowledges the glob can't exclude `*_cover_letter_draft.md` and addresses it via a prose guard — imperfect but workable given current glob limitations.

**RECOMMENDATION:** The scope guard is a prose workaround for a glob limitation. If a future agent ignores prose and follows only the glob, it will apply resume-format rules to the cover letter. Consider splitting the glob or adding a mechanical filename check instruction (e.g., "If the filename contains `cover_letter`, stop and load `cover-letter-voice.md` instead").

---

#### 1.3 — Are the glob-based auto-load triggers correct and unambiguous?

**VERDICT:** FRAGILE

**FINDING:** Most globs are correct. The known fragility is in `resume-format.md`'s glob `["resume-outputs/p02/*_draft.md", "resume-outputs/p03/*_final.md"]`, which matches cover letter filenames (`*_cover_letter_draft.md`, `*_cover_letter_final.md`). The prose scope guard mitigates this, but it's a soft mitigation — a less careful agent could miss it. Additionally, `bank-facts.md` triggers on `["resume-outputs/p02/**", "resume-outputs/p03/**"]`, which is correct but broad — it loads for changelogs and tailoring notes too, which is fine (they reference bank rules) but could theoretically cause confusion if a non-content file in p02/p03 were created.

**EVIDENCE:** [resume-format.md L3](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/resume-format.md#L3) and [L6-8](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/resume-format.md#L6-L8).

**RECOMMENDATION:** Add `!*_cover_letter_*` exclusion to the glob if the platform supports it, or rename the glob comment from an HTML comment to a bolded instruction at the top of the frontmatter so it's not easily skipped.

---

#### 1.4 — Is pass-sequence.md clear enough for a cold start?

**VERDICT:** WORKING

**FINDING:** [pass-sequence.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/pass-sequence.md) is well-structured: each pass lists skills, reads, writes, gates, and rules loaded. The gate between Pass 1 and Pass 2 is explicitly documented. The "Literal invocation prompts are in `@PROMPTS.md`" note at line 8 correctly directs an agent to the copy-paste prompts. A new agent could pick this up cold.

**EVIDENCE:** [pass-sequence.md L6-83](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/pass-sequence.md).

**RECOMMENDATION:** Consider adding a one-line "run `/clear` between passes" note to pass-sequence.md for agents that don't read PROMPTS.md's preamble.

---

#### 1.5 — Is write-permissions.md sufficiently narrow?

**VERDICT:** WORKING

**FINDING:** [write-permissions.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/write-permissions.md) is tight. `knowledge/` is absolute no-write. `master_experience_bank.md` is explicitly read-only with a note that it's OS-level `chmod`-protected. `checkpoint_log.md` is the sole writable file in `raw-resume-docs/`. `job-descriptions/` has exactly two permitted write actions (new JD file, new or append-only company notes). This is well-designed.

**EVIDENCE:** [write-permissions.md L15-48](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/write-permissions.md#L15-L48).

**RECOMMENDATION:** None required. This is one of the build's genuine strengths.

---

### DIMENSION 2 — KNOWLEDGE ASSET QUALITY & COMPLETENESS

---

#### 2.1 — Is the Canonical Facts Sheet complete and consistent?

**VERDICT:** WORKING

**FINDING:** Section 1 of [master_experience_bank.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md) is thorough. Employer names, titles, dates, and tense are canonicalized with explicit "do not use these incorrect variants" warnings. The identity/education section covers every field a resume or application form would need. All Section 4 bullets use facts anchored in Section 1 — no bullet uses a title, date, or employer name that contradicts Section 1.

**EVIDENCE:** [master_experience_bank.md L16-88](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L16-L88).

**RECOMMENDATION:** Add a "Last verified" date to Section 1.1 (Identity & Education) so Josh knows when to re-check contact info.

---

#### 2.2 — Is the Metrics Ledger fully traceable?

**VERDICT:** WORKING

**FINDING:** M1 through M30 are all defined with employer, status, and usage notes. I spot-checked every metric reference in both BCG and KPMG final outputs against the ledger — all trace correctly. The verification notes tables in both cover letters map every numerical claim to an M-number. The one gap: the Section 4 bullets don't always carry inline M-number tags — the tracing is done by the agent at draft/audit time rather than being mechanically embedded in the bullet text. This works but depends on agent diligence.

**EVIDENCE:** [master_experience_bank.md L47-81](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L47-L81); [BCG CL final fact table](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_cover_letter_final.md#L102-L114); [KPMG CL final fact table](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_cover_letter_final.md#L71-L88).

**RECOMMENDATION:** Consider adding inline M-number references to the Section 4 bullet text itself (e.g., as trailing comments) so agents don't need to cross-reference the ledger manually each time. This would make the tracing mechanical rather than interpretive.

---

#### 2.3 — Could two variants of the same fact end up on one resume?

**VERDICT:** FRAGILE

**FINDING:** The bank flags some variant pairs explicitly — e.g., "Advised 12 Etsy sellers" vs. "Managed a portfolio of 12 concurrent clients" at [line 218](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L218), and the fulfillment-app pair at [line 219](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L219). But the flagging is inconsistent — some variant pairs are marked with explicit "do not place on the same resume" notes, while others that share the same underlying fact (e.g., the multiple Revry dashboard bullets that describe the same QuickSight/Looker Studio work from different angles) rely on the agent to recognize the overlap. In the KPMG draft, Revry bullet 1 and the combined Python-plus-dashboard bullet from 5.6.A were not both used, but the risk is real for a less careful agent.

**EVIDENCE:** [bank-facts.md L46-50](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/bank-facts.md#L46-L50); [master_experience_bank.md L218-219](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L218-L219).

**RECOMMENDATION:** Add a variant-pair index to the bank — a flat table listing every pair of bullets that cannot coexist on one resume, with the bullet text's first 10 words and the reason. This makes the check mechanical rather than interpretive.

---

#### 2.4 — Is RETIRED content clearly isolated?

**VERDICT:** WORKING

**FINDING:** There is exactly one RETIRED item (the "most-requested language course" superlative at [line 81](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L81)), and it is marked with `RETIRED — do not use` in bold, with a confirmed replacement (M28) named inline. The hard-rules section at line 87 repeats the prohibition. Neither BCG nor KPMG outputs used it.

**EVIDENCE:** [master_experience_bank.md L81](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L81), [L87](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L87).

**RECOMMENDATION:** None. Isolation is adequate for one item. If more items retire over time, consider a separate RETIRED section at the end of the bank to group them.

---

#### 2.5 — Are the base resumes genuinely one-page at 10.5pt?

**VERDICT:** FRAGILE

**FINDING:** The base resumes are markdown code blocks in the bank, not rendered PDFs. Page fit cannot be confirmed from within the workflow. Both the BCG and KPMG changelogs flag this as an open item, and both final files carry "Length/page-fit note" sections with cut levers (Carlson DNP bullet, ASL continuity bullet, Education Format B). The system acknowledges the problem and provides fallback levers, but there is no mechanism to test page fit before Josh opens Google Docs. The BCG final has 9 bullets + Format A education; the KPMG final has 10 bullets. Both are described as "tight but plausible."

**EVIDENCE:** [BCG final L163-173](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L163-L173); [KPMG final L133-135](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_final.md#L133-L135).

**RECOMMENDATION:** Add a character/word-count budget to `resume-format.md` derived from empirical testing (e.g., "at 10.5pt TNR with 0.5" margins, the one-page ceiling is approximately X characters including headers"). This gives the agent a mechanical check rather than relying on "likely tight."

---

#### 2.6 — Is writing_style_profile-josh_v2.md specific enough?

**VERDICT:** WORKING

**FINDING:** This is one of the strongest assets in the build. [writing_style_profile-josh_v2.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/knowledge/writing_style_profile-josh_v2.md) is 367 lines, evidence-mapped to 18 writing samples, with hard rules, context-switching matrices, before/after examples, a never-do list, and a self-check checklist. It specifies punctuation (no em-dashes, no semicolons, no ellipses, no colons introducing lists), verb preferences (plain English over Latinate), sentence structure, and register-specific behavior. The BCG final cover letter's writing-profile self-check at [L79-92](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_cover_letter_final.md#L79-L92) demonstrates that the profile produces a genuinely differentiated voice — the "mostly adopted" word choice, the sentence-splitting, and the contraction-off register are all evidence-based adjustments.

**EVIDENCE:** [writing_style_profile-josh_v2.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/knowledge/writing_style_profile-josh_v2.md) entire file; [BCG CL final self-check](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_cover_letter_final.md#L73-L92).

**RECOMMENDATION:** None. This is a genuine strength.

---

#### 2.7 — Knowledge gaps?

**VERDICT:** FRAGILE

**FINDING:** The truncated state of [target_lane_rubrics_and_bullets.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/knowledge/target_lane_rubrics_and_bullets.md) is a significant gap. The file is only 24 lines and contains Sections 5.4 and 6 of what is clearly a larger document — the file itself references "sections 2.3, 3.3, 4.3" for lane-specific language, but those sections do not exist in this file. This means Pass 1's lane-matching step, which is instructed to "Read... `knowledge/target_lane_rubrics_and_bullets.md`", is working from an incomplete reference. Both BCG and KPMG Pass 1 analyses appear to have defaulted to using the bank's own lane tags and base-resume descriptions to make lane decisions, which worked, but the designed knowledge asset was not available in full.

**EVIDENCE:** [target_lane_rubrics_and_bullets.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/knowledge/target_lane_rubrics_and_bullets.md) — 24 lines, 1678 bytes. References to "sections 2.3, 3.3, 4.3" at [line 22](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/knowledge/target_lane_rubrics_and_bullets.md#L22) that don't exist in the file.

**RECOMMENDATION:** Restore the full `target_lane_rubrics_and_bullets.md` with Sections 1–6 intact, or explicitly acknowledge in the file that it's a partial extract and redirect agents to the bank's own lane tags for lane matching.

---

### DIMENSION 3 — PROMPT DESIGN & AGENT INSTRUCTION QUALITY

---

#### 3.1 — Is each prompt genuinely self-contained?

**VERDICT:** WORKING

**FINDING:** Each prompt block in [PROMPTS.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/PROMPTS.md) reads its inputs from disk (JD file, bank, prior pass's output file) rather than relying on conversational memory. The preamble at lines 19-25 explicitly instructs "Run `/clear` between prompts — same terminal window is fine, but don't carry one pass's context into the next." Pass 2 reads the p01 file to get Josh's answers. Pass 3 reads the p02 draft.

**EVIDENCE:** [PROMPTS.md L19-25](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/PROMPTS.md#L19-L25); each prompt block names its input files explicitly.

**RECOMMENDATION:** None. This is well-designed.

---

#### 3.2 — Are placeholders the only variables?

**VERDICT:** WORKING

**FINDING:** `[company_slug]` and `[role_slug]` are the only bracketed placeholders. There are no hidden assumptions — the prompt text includes all file paths, skill names, and rule file references inline.

**EVIDENCE:** [PROMPTS.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/PROMPTS.md) — every prompt block.

**RECOMMENDATION:** None.

---

#### 3.3 — Is the Human Checkpoint failure-resistant?

**VERDICT:** FRAGILE

**FINDING:** The mechanic works: Josh edits the numbered list, Pass 2 checks completeness. But the system has no handling for *quality* of answers, only *presence*. The BCG answers were clean and concise. The KPMG OQ-2 answer was a dense multi-sentence response containing new facts, a URL, and a typo ("Vizualization"). Pass 2 correctly refused to use the new facts (checkpoint staging rule), but there's no instruction for what Pass 2 should do if an answer is ambiguous, contradictory, or vague beyond "stop and tell me which OQ numbers are still unanswered." A vague answer (e.g., "maybe" for a yes/no OQ) would pass the completeness check but leave the agent guessing.

**EVIDENCE:** [KPMG p01 analysis L146-152](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p01/kpmg-data_analyst_contract_analysis.md#L146-L152) — OQ-2 answer is 4 lines long with multiple embedded facts; [checkpoint.md L40-48](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/checkpoint.md#L40-L48) — checks for existence and length only.

**RECOMMENDATION:** Add a "clarity check" instruction to Pass 2: if an answer is ambiguous or could be interpreted multiple ways, stop and ask Josh to clarify that specific OQ rather than proceeding with a guess.

---

#### 3.4 — Does Prompt 2 handle both resume and cover letter adequately?

**VERDICT:** FRAGILE

**FINDING:** Prompt 2 gives the resume draft 17 lines of detailed instruction (lines 152-169 of PROMPTS.md) covering bullet pool, keyword integration, summary cap, character scrub, and format rules. The cover letter draft gets 8 lines (lines 175-183), and those 8 lines are primarily structural ("draft it using `cover_letter_standards.md` and the bank's Section 5.7 component bank"). The cover letter draft instruction does not explicitly require the agent to avoid duplicating content between the opening paragraph and the body proof paragraph. This is the root cause of the KPMG duplicate-Revry issue: the opening paragraph summarized the Revry work, and then body paragraph 1 detailed the same Revry work, creating near-verbatim repetition.

**EVIDENCE:** [PROMPTS.md L175-183](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/PROMPTS.md#L175-L183); [KPMG CL final L16-31](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_cover_letter_final.md#L16-L31) — opening paragraph (lines 16-21) and body paragraph 1 (lines 23-31) both describe the same Revry dashboarding, VPN investigation, and data cleaning work.

**RECOMMENDATION:** Add to Prompt 2's cover letter instruction: "The opening paragraph should hook with a summary-level claim; the body paragraph(s) should add *different* evidence that the opening did not detail. Do not repeat the same Revry/Carlson/Etsy proof in both the opening and the body."

---

#### 3.5 — Does Prompt 3 have scope ambiguity on ATS audit?

**VERDICT:** WORKING

**FINDING:** Prompt 3's resume track is explicitly scoped to formatting and ATS audit: "This pass is formatting/ATS work only — no voice rewrite" and "carried forward with no content changes." Both BCG and KPMG Pass 3 final files confirm "no substantive content changes" from Pass 2. The agent did not use the ATS audit as a license to rewrite content.

**EVIDENCE:** [PROMPTS.md L209-231](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/PROMPTS.md#L209-L231); [BCG final L4](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L4); [KPMG final L4](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_final.md#L4).

**RECOMMENDATION:** None.

---

#### 3.6 — Are model/effort recommendations calibrated?

**VERDICT:** WORKING

**FINDING:** [model-effort.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/model-effort.md) provides a clear table with per-pass recommendations for three agent platforms. The escalation ladder is sensible (raise effort on rule-compliance misses, switch models on persistent reasoning failures). The "why high" justification at lines 21-25 is specific and grounded.

**EVIDENCE:** [model-effort.md L13-19](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/model-effort.md#L13-L19).

**RECOMMENDATION:** The Antigravity CLI column recommends "Flash (Medium)" for Pass 2 and Pass 3, but the BCG and KPMG runs appear to have been done with Claude models. If Antigravity/Gemini is used for future runs, the Flash recommendation may be too light for Pass 2's multi-skill chain — consider adding a note that Gemini Pro or equivalent may be needed for Pass 2/3.

---

#### 3.7 — Does Prompt 0 produce enough for Pass 1?

**VERDICT:** FRAGILE

**FINDING:** Prompt 0 creates the JD file and company-notes file, but the company-notes file defaults to "None" in the body. Both BCG and KPMG company-notes files have "None" as the body. The KPMG JD file *does* contain company notes embedded in the saved JD text (lines 51-98), but this was pasted in as raw JD text, not structured per the company-notes schema — the agent had to fish for it during Pass 2. Prompt 0 does not instruct the agent to populate the company-notes file with any research or JD-embedded company information.

**EVIDENCE:** [boston_consulting_group-company_notes.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/job-descriptions/boston_consulting_group-company_notes.md) — body is "None"; [kpmg-company_notes.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/job-descriptions/kpmg-company_notes.md) — body is "None"; [kpmg-data_analyst_contract.md L51-98](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/job-descriptions/kpmg-data_analyst_contract.md#L51-L98) — company info embedded in raw JD text.

**RECOMMENDATION:** Add a step to Prompt 0: "If the JD text or URL page contains an 'About the company' section, extract that content into the free-form body of the company-notes file rather than leaving it as 'None'. This gives Pass 2 and the cover letter a structured company-detail source."

---

### DIMENSION 4 — COMPLIANCE ENFORCEMENT & HARD-FAIL CHECKING

---

#### 4.1 — Em-dash/semicolon sweep

**VERDICT:** WORKING

**FINDING:** Both BCG and KPMG final resumes contain zero em-dashes and zero semicolons in the resume text blocks. I confirmed this with `grep` searches against both final files — em-dashes appear only in the commentary/audit sections (outside the code fence), never in the resume text itself. The KPMG final additionally changed date range formatting from en-dash (`–`) to `to` (e.g., "Sep 2025 to Apr 2026") to avoid smart-substitution risk, which is a thoughtful extra precaution. The BCG final kept en-dashes in date ranges, which is technically correct per the rule (em-dashes are banned, en-dashes in ranges are fine) but inconsistent with the KPMG approach.

**EVIDENCE:** Grep results: zero em-dashes in KPMG final (entire file); BCG final em-dashes only on lines 1, 59, 66, 72, 80, 93, 102, etc. — all in commentary, none inside the code fence on lines 8-53.

**RECOMMENDATION:** Standardize date-range format across applications — either always use `to` or always use en-dashes, but pick one and document it in `resume-format.md`.

---

#### 4.2 — 50-word summary cap

**VERDICT:** WORKING

**FINDING:** BCG final summary: I count 47 words. "Carlson(1) MBA(2) '26(3) brand(4) and(5) strategy(6) consultant(7). Led(8) survey(9) analysis(10) and(11) executive(12) presentation(13) for(14) a(15) Little(16) Tikes(17) brand-architecture(18) refresh(19), delivering(20) 3(21) recommendations(22) to(23) CMO(24) and(25) VP-level(26) stakeholders(27) on(28) a(29) 6-person(30) team(31). Skilled(32) at(33) translating(34) complex(35) business(36) problems(37) into(38) clear(39) recommendations(40) across(41) teams(42) with(43) diverse(44) backgrounds(45) and(46) disciplines(47)." — **47 words, PASS.**

KPMG final summary: "Data(1) analyst(2) and(3) Carlson(4) MBA(5) graduate(6) focused(7) on(8) data(9) cleaning(10), dashboard(11) QA(12), Python(13) modeling(14), and(15) stakeholder(16) reporting(17). Built(18) Amazon(19) QuickSight(20) and(21) Looker(22) Studio(23) dashboards(24), structured(25) metadata(26) with(27) Python(28) and(29) IMDb(30) API(31) pulls(32), and(33) investigated(34) anomalies(35) to(36) turn(37) messy(38) content(39) and(40) advertising(41) datasets(42) into(43) decision-ready(44) insights(45)." — **45 words, PASS.**

Both agents performed word counts and documented them. The counting method is consistent (hand-count, reported in the draft and final files).

**EVIDENCE:** [BCG final L59-64](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L59-L64); [KPMG final L78](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_final.md#L78).

**RECOMMENDATION:** None — the counts are correct and documented.

---

#### 4.3 — Number traceability to Metrics Ledger

**VERDICT:** WORKING

**FINDING:** Line-by-line audit of both final resumes and cover letters against the Metrics Ledger:

**BCG final resume numbers:** 700/500 (M1), 3 recommendations ×2 (M2), 6-person team ×2 (M3), CMO/VP (M4), 12 concurrent clients (M26), 30% revenue growth (M7), ~$15K (M6), 75+ students (M13), 15+ campus (M13), 100+ events (M14), 10+ high-profile (M14). All traceable. ✅

**BCG final cover letter numbers:** 700/500 (M1), three recommendations (M2), six-person team (M3), CMO/VP (M4). All traceable. ✅

**KPMG final resume numbers:** 700/500 (M1), 3 recommendations (M2), CMO/VP (M4), 6-person team (M3), 18% conversion (M9), 12% ad-cost (M10), 70% time cut (M8), 75+ students (M13), 15+ campus (M13), 100+ events (M14), 10+ high-profile (M14). All traceable. ✅

**KPMG final cover letter numbers:** 700/500 (M1), three recommendations (M2), 18% conversion (M9), 12% ad-cost (M10). All traceable. ✅

No invented numbers found in either output.

**EVIDENCE:** Cross-referenced against [master_experience_bank.md L47-81](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L47-L81).

**RECOMMENDATION:** None. The bank-facts discipline held across both runs.

---

#### 4.4 — Variant exclusion reasoning

**VERDICT:** WORKING

**FINDING:** Both changelogs document variant exclusion explicitly. BCG changelog at [L52-57](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/boston_consulting_group-consultant_changelog.md#L52-L57): "Confirmed the Etsy 'portfolio of 12 concurrent clients' bullet is used alone; the 'Advised 12 Etsy sellers' variant (5.6.B) does not appear anywhere." KPMG changelog at [L85-89](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_changelog.md#L85-L89): "the resume does not use both Etsy 'Advised 12 sellers' and 'portfolio of 12 concurrent clients' variants."

**EVIDENCE:** Both changelogs, "No two variants of the same fact" subsections.

**RECOMMENDATION:** None — the reasoning is documented and correct.

---

#### 4.5 — Cover letter voice pass content-lock compliance

**VERDICT:** FRAGILE

**FINDING:** Both cover letter finals include Fact/rule check tables that account for every claim. The BCG final explicitly documents three word-choice swaps ("substantially" → "mostly"; second "adopted" → "acted on"; "act on immediately" → "put into practice immediately") as phrasing changes, not fact changes, and confirms "nothing new was introduced." The KPMG final states "No new factual claims were introduced in the voice pass." However, neither fact table catches the structural problem of the duplicate Revry paragraph (see 4.7 below) — the fact table checks that each claim traces to a source, but not that the *same source* is used redundantly across paragraphs.

**EVIDENCE:** [BCG CL final L96-123](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_cover_letter_final.md#L96-L123); [KPMG CL final L69-90](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_cover_letter_final.md#L69-L90).

**RECOMMENDATION:** Add a "redundancy check" to the fact/rule check instruction: "Confirm that no proof paragraph restates claims already made in the opening paragraph."

---

#### 4.6 — Orphaned rules (documented but never invoked)

**VERDICT:** FRAGILE

**FINDING:** The `cover_letter_standards.md` checklist at Section 8 includes "Adds content not on the resume; does not just paraphrase it" ([cover_letter_standards.md L185](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/knowledge/cover_letter_standards.md#L185)). This is a key quality standard, but PROMPTS.md never explicitly instructs the agent to check against it. The KPMG cover letter violates this standard — its body paragraph 1 substantially paraphrases the resume's Revry bullets rather than adding distinct content.

Additionally, `ats_ai_screening.md` Section 7.4 includes "LinkedIn alignment — Resume titles, employers, and dates must match LinkedIn exactly" ([ats_ai_screening.md L149-153](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/knowledge/ats_ai_screening.md#L149-L153)). This is flagged but explicitly unresolvable from within the workflow. It's documented as a gap in both tailoring notes, which is the correct handling.

**EVIDENCE:** [cover_letter_standards.md L185](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/knowledge/cover_letter_standards.md#L185); [KPMG CL final L16-31](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_cover_letter_final.md#L16-L31).

**RECOMMENDATION:** Add the "adds content not on the resume" check to Prompt 2's cover letter instruction as an explicit requirement.

---

#### 4.7 — Missing rules or checks that should exist

**VERDICT:** BROKEN

**FINDING:** There is no rule or check that prevents a cover letter's opening paragraph from substantially repeating the same facts as its body proof paragraph. This is the root cause of the KPMG cover letter's duplicate Revry issue.

**Duplicate Revry paragraph documentation:**

- **Opening paragraph** (lines 16-21 of the final): "At Revry, I built executive dashboards from messy content and advertising data, checked source issues before stakeholder review, and traced reporting anomalies tied to international VPN usage."
- **Body paragraph 1** (lines 23-27): "At Revry, I built revenue, advertising, and content-performance dashboards in Amazon QuickSight and Looker Studio, used Python and IMDb API pulls to collect TV and film metadata, and investigated streaming-data discrepancies tied to international VPN usage."

Both describe the same underlying Revry work (dashboards, VPN investigation, data cleaning). Body paragraph 1 adds tool names (QuickSight, Looker Studio, Python, IMDb API) but the core claims are redundant.

- **Introduced in:** Pass 2 draft ([KPMG CL draft L18-31](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_cover_letter_draft.md#L18-L31)) — the structure was baked in at draft time.
- **Caught by Pass 3:** No. The Pass 3 voice pass changed minor wording but did not flag or fix the structural duplication. The Fact/rule check table accounts for each claim's source but does not check for redundancy.
- **Flagged in changelog or tailoring notes:** No. Neither the [KPMG changelog](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_changelog.md) nor the [KPMG tailoring notes](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_tailoring_notes.md) mention the duplication.

**RECOMMENDATION:** Add a "structural redundancy check" rule, either to `cover-letter-voice.md` or as a Pass 2 cover letter instruction: "The opening paragraph may summarize the candidate's positioning but must not preview specific facts (tool names, project names, metrics) that will appear in the body proof paragraphs. If the opening and body describe the same work, restructure so the opening hooks broadly and the body adds specific detail not already stated."

---

### DIMENSION 5 — OUTPUT QUALITY: BCG APPLICATION

---

#### 5.1 — Does it read as a STRATEGY-lane BCG application?

**VERDICT:** WORKING

**FINDING:** The resume leads with "Carlson MBA '26 brand and strategy consultant" in the summary. The first experience entry is the Carlson Brands Enterprise consulting engagement. A BCG recruiter scanning for 10 seconds would see: MBA from Carlson, led survey analysis, delivered recommendations to CMO/VP stakeholders, collaborated on a 6-person consulting team, delivered a market assessment. This reads as strategy consulting, not data analytics or marketing. The Revry and Etsy entries are correctly framed toward stakeholder synthesis and client-portfolio management rather than their technical or marketing angles.

**EVIDENCE:** [BCG final L12-17](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L12-L17) (summary); [L27-32](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L27-L32) (Carlson block leads Experience).

**RECOMMENDATION:** None.

---

#### 5.2 — Action verbs

**VERDICT:** WORKING

**FINDING:** Every bullet leads with a strong verb: Led, Collaborated, Presented, Delivered, Synthesized, Built, Managed, Ran, Taught. No instances of "helped," "assisted," "supported," "responsible for," or "worked on."

**EVIDENCE:** [BCG final L29-44](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L29-L44).

**RECOMMENDATION:** None.

---

#### 5.3 — Quantification percentage

**VERDICT:** WORKING

**FINDING:** 6 of 9 bullets carry an explicit number (67%). The remaining 3 (Carlson DNP, both Revry bullets) carry named projects/specific facts. Per the standard's own wording ("numbers, project names, or facts in at least 70% of bullets"), 9/9 bullets carry some form of specificity. Strictly on numbers-only, 67% is slightly below the 70% target. The BCG final acknowledges this at [L119-126](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L119-L126).

**EVIDENCE:** [BCG final L119-126](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L119-L126).

**RECOMMENDATION:** If Josh wants to push the numeric ratio above 70%, the Revry "Synthesized audience and advertiser data" bullet could be swapped for a more quantified Revry variant from the bank.

---

#### 5.4 — Summary BCG-specific language

**VERDICT:** FRAGILE

**FINDING:** The summary mirrors BCG language ("complex business problems," "diverse backgrounds and disciplines") but does not name BCG, consulting, or any BCG-specific framing. It's a competent strategy-lane summary, not a BCG-tailored one. Given that the JD is a generic experienced-hire posting with no practice-area or industry specifics, this is arguably appropriate — there isn't much BCG-specific language to mirror. But it does read as a general consulting summary.

**EVIDENCE:** [BCG final L12-17](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L12-L17).

**RECOMMENDATION:** Josh should decide whether to add "targeting management consulting" or similar lane framing to the summary. The current version is defensible for a generic posting but could be sharpened.

---

#### 5.5 — Cover letter voice quality

**VERDICT:** WORKING

**FINDING:** The BCG cover letter reads as professional and specific, not templated. Sentences that work well: "This is the kind of cross-functional work, with colleagues from many backgrounds and disciplines, that BCG asks its consultants to lead" — specific echo of JD language, not generic praise. "Both engagements meant translating ambiguous research findings into recommendations a non-specialist executive audience could put into practice immediately" — honest, plain language, bridges the proof to BCG's needs.

Sentences that feel slightly flat: "That is what draws me to this role over a narrower, single-industry seat" — the contrast is vague; "narrower" isn't specific about what Josh is choosing *against*. "I would welcome a conversation about how my survey-driven strategy work could support BCG's consulting teams" — the closing is functional but generic; "survey-driven strategy work" undersells the breadth demonstrated above.

**EVIDENCE:** [BCG CL final L23-49](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_cover_letter_final.md#L23-L49).

**RECOMMENDATION:** Sharpen the closing to reference a specific BCG practice area or initiative if Josh has one in mind.

---

#### 5.6 — Company-specific detail quality

**VERDICT:** FRAGILE

**FINDING:** The BCG company-notes file body is "None." The cover letter's company-specific detail comes entirely from the JD text: "BCG describes its work as cases that reshape business, government, and society, backed by mentors and structured learning toward an eventual practice-area specialization." This is JD parroting, not company research. A recruiter who reads many cover letters for this posting will see this exact phrasing frequently. The letter lacks a specific BCG initiative, case study, article, or practice area that would signal genuine research.

**EVIDENCE:** [boston_consulting_group-company_notes.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/job-descriptions/boston_consulting_group-company_notes.md) — body is "None"; [BCG CL final L41-46](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_cover_letter_final.md#L41-L46).

**RECOMMENDATION:** Before submitting, Josh should add at least one company-specific detail to the company-notes file — a BCG report he's read, a practice area he's targeting (e.g., Consumer sector, Digital BCG, BCG X), an info session he attended, or a specific BCG consultant he spoke with.

---

#### 5.7 — Untraceable claims

**VERDICT:** WORKING

**FINDING:** No untraceable claims found. Every factual statement about Josh traces to the bank or a checkpoint answer. The BCG-descriptive language (about BCG's casework, mentors, practice areas) is correctly attributed to the JD in the fact table, not presented as a claim about Josh.

**EVIDENCE:** [BCG CL final fact table L102-114](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_cover_letter_final.md#L102-L114).

**RECOMMENDATION:** None.

---

### DIMENSION 6 — OUTPUT QUALITY: KPMG APPLICATION

---

#### 6.1 — De-streaming effectiveness

**VERDICT:** FRAGILE

**FINDING:** The de-streaming partially works. The summary successfully avoids saying "streaming" or "media" — it reads as "Data analyst and Carlson MBA graduate focused on data cleaning, dashboard QA, Python modeling, and stakeholder reporting." However, streaming-specific language leaks through in the Revry bullets: "streaming trends," "ad-monetization insights," "content-performance dashboards," "TV and film metadata," and "content planning." These are all bank-accurate facts, but they signal streaming/media domain expertise rather than the general data-analyst positioning the JD calls for. A KPMG recruiter might wonder why a data analyst is talking about TV metadata and streaming trends.

**EVIDENCE:** [KPMG final L29-39](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_final.md#L29-L39) — "streaming trends," "ad-monetization," "TV and film metadata," "content-performance."

**RECOMMENDATION:** The bank's Revry bullets are inherently streaming-domain. For non-media JDs, consider adding de-streamed Revry variant bullets to the bank (e.g., "revenue and advertising performance dashboards" instead of "content-performance dashboards surfacing streaming trends") that describe the same work in general data-analyst language.

---

#### 6.2 — Credential gap handling

**VERDICT:** WORKING

**FINDING:** The JD asks for "Bachelor's degree from an accredited college or university in mathematics, statistics, computer science, or a related field." The resume truthfully lists: MBA (Carlson) and BA in ASL/English Interpreting (*magna cum laude*). It does not reframe the degree field, add a fake minor, or imply the BA is in a quantitative field. The changelog explicitly notes: "Do not reframe the degree field" ([KPMG changelog L130-132](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_changelog.md#L130-L132)). This is honest and non-evasive.

**EVIDENCE:** [KPMG final L65-71](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_final.md#L65-L71); [KPMG changelog L130-132](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_changelog.md#L130-L132).

**RECOMMENDATION:** None. The handling is appropriate.

---

#### 6.3 — Impact of unpromoted OQ-2 facts

**VERDICT:** FRAGILE

**FINDING:** Josh supplied significant new facts in OQ-2: chi-squared analysis, Cronbach's alpha, standard error, cross-tabulation, a dummy-data dashboard URL, Tableau experience, Power BI experience beyond "familiarity," and a Data Visualization class. These directly address four of the five bank-content gaps identified in Pass 1 (statistical techniques, Tableau/Power BI, data visualization). The system correctly refused to use them (checkpoint staging rule). But their absence meaningfully weakens the resume for this specific JD — "statistical analysis techniques and methodologies" is a listed qualification, and without chi-squared/Cronbach's alpha, the resume can only point to "viewership-projection modeling" as indirect statistical evidence.

The escalation path is documented in the KPMG changelog at [L117-120](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_changelog.md#L117-L120) and tailoring notes at [L33](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_tailoring_notes.md#L33): "remain unused until Josh promotes them into `master_experience_bank.md`." But the *how* of that promotion is not documented anywhere (see 7.5).

**Additionally:** the Pass 2 agent did not write these facts to `checkpoint_log.md` as required by [checkpoint.md L54-56](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/checkpoint.md#L54-L56). The KPMG changelog explicitly says: "This pass did not append to `raw-resume-docs/checkpoint_log.md` because the invocation explicitly limited writes to `resume-outputs/p02/`" ([KPMG changelog L27-28](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_changelog.md#L27-L28)). This is a compliance failure — the checkpoint rule requires writing to `checkpoint_log.md`, and the agent chose not to because of a write-scope interpretation.

**EVIDENCE:** [KPMG p01 L146-152](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p01/kpmg-data_analyst_contract_analysis.md#L146-L152); [checkpoint_log.md](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/checkpoint_log.md) — still empty (header only); [checkpoint.md L54-56](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/checkpoint.md#L54-L56); [KPMG changelog L27-29](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_changelog.md#L27-L29).

**RECOMMENDATION:** Two fixes: (1) Prompt 2 should explicitly instruct the agent to write candidate facts to `checkpoint_log.md` per `checkpoint.md`, not just to `resume-outputs/p02/`. (2) Josh should promote the OQ-2 facts into the bank and re-run the KPMG application if it's still active.

---

#### 6.4 — Cover letter resume duplication

**VERDICT:** BROKEN

**FINDING:** The KPMG cover letter heavily duplicates the resume. The opening paragraph summarizes the Revry work (dashboards, VPN investigation, data cleaning). Body paragraph 1 restates the same Revry work in more detail (QuickSight, Looker Studio, Python, IMDb API, VPN). Then it adds Carlson survey analysis and Etsy paid-media — but these are also on the resume. The closing paragraph adds the KPMG Digital Gateway/Claude detail, which is the only content not on the resume. The `cover_letter_standards.md` standard at Section 8 explicitly says: "Adds content not on the resume; does not just paraphrase it." This standard is violated.

**EVIDENCE:** [KPMG CL final L16-42](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_cover_letter_final.md#L16-L42); [cover_letter_standards.md L185](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/knowledge/cover_letter_standards.md#L185).

**RECOMMENDATION:** The KPMG cover letter needs a rewrite. Body paragraph 1 should tell a story the resume cannot — e.g., describe a specific Revry investigation narrative (the VPN discrepancy discovery process), or discuss Josh's approach to data-quality work in a way that adds depth beyond the bullet-level resume facts.

---

#### 6.5 — Duplicate Revry paragraph confirmation

**VERDICT:** BROKEN

**FINDING:** **Confirmed.** The KPMG cover letter contains two back-to-back descriptions of Revry work:

- **Lines 16-21** (opening paragraph): "At Revry, I built executive dashboards from messy content and advertising data, checked source issues before stakeholder review, and traced reporting anomalies tied to international VPN usage."
- **Lines 23-31** (body paragraph 1, starting "At Revry, I built revenue, advertising, and content-performance dashboards in Amazon QuickSight and Looker Studio, used Python and IMDb API pulls to collect TV and film metadata, and investigated streaming-data discrepancies tied to international VPN usage.")

Both describe dashboarding, data cleaning, and VPN investigation. Body paragraph 1 adds tool names but is not a genuinely distinct proof argument.

**Introduced in:** Pass 2 draft. The [KPMG CL draft L18-31](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_cover_letter_draft.md#L18-L31) shows the same structure — opening paragraph with summary Revry, body paragraph 1 with detailed Revry.

**Pass 3 action:** The voice pass changed minor wording (e.g., "which maps closely to KPMG's need" → "That work maps closely to KPMG needs") but did not flag or fix the structural duplication. Neither the [writing-profile self-check](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_cover_letter_final.md#L50-L66) nor the [fact/rule check](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_cover_letter_final.md#L69-L90) mention the duplication.

**Flagged in changelog:** No. [KPMG P2 changelog](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p02/kpmg-data_analyst_contract_changelog.md) does not mention it. [KPMG tailoring notes](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_tailoring_notes.md) does not mention it.

**RECOMMENDATION:** Fix the KPMG cover letter: restructure so the opening hooks broadly (e.g., "My background is in data cleaning, dashboard QA, Python-supported reporting, and stakeholder-ready analytics") without detailing specific Revry work, then let body paragraph 1 be the first and only detailed Revry description. Add a structural redundancy check to the cover letter rules.

---

#### 6.6 — "Dear Hiring Manager" and contact name research

**VERDICT:** FRAGILE

**FINDING:** Both applications use "Dear Hiring Manager." Neither Prompt 0 nor Prompt 1 includes a step to research the hiring manager's name. `cover-letter-voice.md` at [L48-50](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/cover-letter-voice.md#L48-L50) instructs: "Use the hiring manager's name if findable via LinkedIn, the company careers page, the team page, or a targeted Google search." But this instruction appears only in the Pass 3 cover letter voice rule, by which point the content is locked and name insertion would be a minor edit. The BCG tailoring notes report that a web search was run but no name was found ([BCG tailoring notes L81-86](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_tailoring_notes.md#L81-L86)). For the KPMG contract posting, a named hiring manager is unlikely but worth attempting.

**EVIDENCE:** [cover-letter-voice.md L48-50](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/cover-letter-voice.md#L48-L50); both cover letter finals use "Dear Hiring Manager."

**RECOMMENDATION:** Add a "hiring-manager name research" step to Prompt 0 or Prompt 1 — earlier in the workflow, before content is locked. Even a note like "If you find a name, log it in the company-notes file" would help.

---

#### 6.7 — Untraceable claims in KPMG outputs

**VERDICT:** WORKING

**FINDING:** No untraceable claims found. Every factual statement about Josh traces to the bank or a checkpoint answer. The KPMG/Digital Gateway/Claude detail is correctly attributed to the JD's embedded company-notes text, not presented as a claim about Josh.

**EVIDENCE:** [KPMG CL final fact table L71-88](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_cover_letter_final.md#L71-L88).

**RECOMMENDATION:** None.

---

### DIMENSION 7 — SYSTEM FRAGILITY, SCALABILITY & MISSING INFRASTRUCTURE

---

#### 7.1 — PDF rendering gap

**VERDICT:** FRAGILE

**FINDING:** The workflow produces markdown, not PDF. The Google Docs typesetting step is manual. Both final files include a "Manual export hygiene" section instructing Josh to disable smart substitutions and straighten quotes. This is the right mitigation, but the risk is real: Google Docs can auto-substitute straight quotes with curly quotes, straight hyphens with em dashes, and double hyphens with en dashes — all of which would violate the zero-em-dash rule without Josh noticing. The plain-text paste test is flagged but has never been run (both final files mark it as `[ ]` incomplete).

**EVIDENCE:** [BCG final L193-207](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L193-L207); [KPMG final L155-163](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_final.md#L155-L163).

**RECOMMENDATION:** Create a per-application pre-submission checklist template (see Section 5 below) that Josh runs after typesetting, including the paste test and a final em-dash/semicolon search in the Google Doc.

---

#### 7.2 — LinkedIn verification gap

**VERDICT:** FRAGILE

**FINDING:** Both tailoring notes flag LinkedIn alignment as unverifiable from within the workflow. This is a meaningful gap — `ats_ai_screening.md` Section 7.4 warns that "inconsistencies are red flags" and "recruiters often check LinkedIn before passing the resume to the hiring manager." The workflow can't access LinkedIn, so this must be Josh's responsibility.

**EVIDENCE:** [BCG tailoring notes L47-50](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_tailoring_notes.md#L47-L50); [KPMG final L108](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_final.md#L108).

**RECOMMENDATION:** Add a "LinkedIn alignment" checklist item to the per-application pre-submission checklist. Consider adding a one-time skill or prompt that compares a LinkedIn export against the bank's Section 1.2 employers/titles/dates.

---

#### 7.3 — Plain-text paste test gap

**VERDICT:** FRAGILE

**FINDING:** The test is flagged in both final files but never executed. Since the workflow doesn't produce a PDF, the test can't run until Josh typesets. This is a known-and-accepted gap, not a design flaw. The risk is that Josh forgets or skips it.

**EVIDENCE:** [BCG final L101-103](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/boston_consulting_group-consultant_final.md#L101-L103); [KPMG final L94](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p03/kpmg-data_analyst_contract_final.md#L94).

**RECOMMENDATION:** Include it in the pre-submission checklist.

---

#### 7.4 — Application Tracking Log maintenance

**VERDICT:** FRAGILE

**FINDING:** The Application Tracking Log in [Section 5.8](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L490-L508) has 11 target companies listed but most rows have "Josh to fill in" for dates and outcomes. BCG and KPMG are not in the log. There is no mechanism to keep the log updated — Prompt 0 does not instruct the agent to add the new application to the log, and `write-permissions.md` does not allow writes to `master_experience_bank.md` where the log lives. The log will drift and become useless.

**EVIDENCE:** [master_experience_bank.md L490-508](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L490-L508).

**RECOMMENDATION:** Either move the tracking log out of the read-only bank into a separate writable file (e.g., `raw-resume-docs/application_log.md`), or add a Prompt 0 instruction to remind Josh to update the log manually after each application.

---

#### 7.5 — Bank promotion documentation

**VERDICT:** MISSING

**FINDING:** There is no documentation anywhere in the project for *how* Josh should promote a fact from `checkpoint_log.md` into `master_experience_bank.md`. The rules say Josh does this "himself, separately" ([write-permissions.md L24](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/write-permissions.md#L24)), but there is no guidance on: which section of the bank to add the fact to, how to assign an M-number, what status tag to use, whether to create a new bullet variant or modify an existing one, or how to handle facts that span multiple sections (like the chi-squared / Tableau / Data Visualization class facts from KPMG OQ-2). The risk is a malformed bank entry that breaks the system's tracing integrity.

**EVIDENCE:** No file in the project contains promotion instructions. Search for "promot" in all rule and knowledge files returns only references *to* the promotion action, not instructions *for* it.

**RECOMMENDATION:** Add a "Bank Promotion Guide" section to the bank itself (or as a separate file in `raw-resume-docs/`), specifying: (1) assign the next sequential M-number, (2) fill in the ledger row with employer, status, and usage notes, (3) add the bullet variant to the appropriate Section 4 employer pool, (4) tag with lane and status, (5) run a quick cross-check that the new fact doesn't duplicate or conflict with an existing entry.

---

#### 7.6 — Cross-lane/ambiguous JD handling

**VERDICT:** FRAGILE

**FINDING:** The three lanes (STREAMING, MKTG, STRATEGY) don't cleanly cover every possible JD. The KPMG Data Analyst role is a case in point — it's a general data analyst posting that doesn't map to any lane well. Pass 1 selected STREAMING as the "least wrong" option and then de-streamed the language, which is a workable approach but relies on agent judgment rather than a documented procedure. The tracking log shows roles at 3M, General Mills, Cummins, ETS, Education Pioneers, Coloplast, and Global Organic — some of these (e.g., ETS, Education Pioneers) might not map cleanly to any lane. There is no documented fallback for an agent that encounters a truly cross-lane JD or a JD outside all three lanes.

**EVIDENCE:** [KPMG p01 analysis L12-27](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/resume-outputs/p01/kpmg-data_analyst_contract_analysis.md#L12-L27) — "The role is not streaming-specific, but among the three available lanes... STREAMING is the closest match because it is the bank's data/BI-heavy lane." [master_experience_bank.md L490-508](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/raw-resume-docs/master_experience_bank.md#L490-L508) — target company list.

**RECOMMENDATION:** Add a "GENERAL DATA / CROSS-LANE" fallback procedure to `pass-sequence.md` or the bank, specifying: when no lane is a clean fit, start from the base resume closest to the JD's technical requirements, de-domain the language, and note the cross-lane status in the analysis file.

---

#### 7.7 — Orphaned or missing skills

**VERDICT:** FRAGILE

**FINDING:** 22 skill directories exist in `.agents/skills/`. PROMPTS.md explicitly calls the following skills:

- Pass 1: `job-description-analyzer`
- Pass 2: `resume-tailor`, `resume-bullet-writer`, `resume-quantifier`, `resume-section-builder`, conditionally `career-changer-translator`, conditionally `tech-resume-optimizer`
- Pass 3: `resume-ats-optimizer`, `resume-formatter`

That's 8 skills called (6 always, 2 conditionally). The remaining 14 skills are never called in PROMPTS.md:

- `academic-cv-builder`, `application-form-filler`, `cold-email-writer`, `cover-letter-generator`, `creative-portfolio-resume`, `executive-resume-writer`, `interview-prep-generator`, `linkedin-profile-optimizer`, `offer-comparison-analyzer`, `portfolio-case-study-writer`, `reference-list-builder`, `resume-version-manager`, `salary-negotiation-prep`

`cover-letter-generator` is explicitly mentioned in `pass-sequence.md` as "stays in scope as a fallback only, never the default" ([pass-sequence.md L40-42](file:///Users/dweebz/ai_projects/code-playground/resume-buildr-optimizer/.agents/rules/pass-sequence.md#L40-L42)). The rest are likely utility skills available for Josh's broader career toolkit but not part of the core resume-tailoring workflow.

No skills are called in PROMPTS.md that don't have a corresponding skill directory.

**EVIDENCE:** Skills directory listing; PROMPTS.md skill references.

**RECOMMENDATION:** Add a note to AGENTS.md clarifying which skills are part of the core workflow and which are supplementary tools for other career tasks. This prevents future confusion about whether an unused skill is orphaned or intentionally available but uncalled.

---

#### 7.8 — Top 3 failure points for the next application

**VERDICT:** N/A (predictive)

**FINDING:**

1. **Cover letter structural duplication.** The KPMG cover letter's duplicate-Revry issue will recur in any application where Revry is both the strongest proof and the most JD-relevant experience. Without a structural redundancy check, the agent will naturally preview the best proof in the opening and then detail it again in the body.

2. **Checkpoint log not written.** The KPMG run demonstrated that the agent may interpret Prompt 2's write scope narrowly and skip the `checkpoint_log.md` write. If the next application surfaces new facts in OQ answers, they will be stranded in the p01 file and not persisted to the staging area.

3. **Company-notes gap.** Both applications had "None" in the company-notes body. Without a Prompt 0 instruction to extract company info from the JD, the next application will also have an empty company-notes file, producing a cover letter with generic company detail.

**RECOMMENDATION:** Fix all three before the next run — see specific recommendations above.

---

## 3. PRIORITY REPAIR LIST

| # | Issue | Severity | Fix |
|---|---|---|---|
| 1 | **KPMG cover letter duplicate Revry paragraph** | BROKEN | Rewrite the KPMG cover letter with a restructured opening that hooks broadly, then a single detailed Revry proof paragraph. Add a structural redundancy check to the cover letter rules. |
| 2 | **Checkpoint log never written to** | BROKEN | Fix Prompt 2 to explicitly instruct writes to `checkpoint_log.md` for new facts from OQ answers. Verify `write-permissions.md` is not being misread as restricting this write. |
| 3 | **Missing bank promotion guide** | MISSING | Create a promotion guide (format, M-number assignment, which section, status tagging) and place it in `raw-resume-docs/` or the bank itself. |
| 4 | **target_lane_rubrics_and_bullets.md truncated** | BROKEN | Restore the full file or acknowledge its partial state and redirect agents to the bank's lane tags. |
| 5 | **Prompt 0 company-notes gap** | FRAGILE | Add a step to Prompt 0 to extract company info from JD text into the company-notes body. |
| 6 | **Cover letter instruction lacks anti-duplication rule** | FRAGILE | Add explicit instruction to Prompt 2: opening and body must not describe the same work. |
| 7 | **Prompt 2 cover letter rigor gap** | FRAGILE | Add "adds content not on the resume" from `cover_letter_standards.md` §8 as an explicit Prompt 2 check. |
| 8 | **OQ-2 facts not promoted for KPMG** | FRAGILE | Josh should promote chi-squared, Cronbach's alpha, Tableau, Power BI, Data Visualization class facts into the bank and re-run if KPMG application is still active. |
| 9 | **Application tracking log in read-only file** | FRAGILE | Move the log to a writable file or add a manual-update reminder to Prompt 0. |
| 10 | **Page-fit unverifiable in workflow** | FRAGILE | Add a character-count budget to `resume-format.md` based on empirical Google Docs testing. |

---

## 4. WHAT IS WORKING WELL

- **Master experience bank** — The anchor of the entire system. Meticulously sourced, status-tagged, variant-flagged, with hard rules that prevent the most dangerous fabrication patterns. This is best-in-class for LLM-assisted resume work.
- **Writing style profile** — 367 lines of evidence-mapped voice specification. Produces genuinely differentiated output, not generic AI prose. The self-check checklist is a real enforcement mechanism.
- **Write-permissions discipline** — Default-deny with narrow exceptions. OS-level chmod as a backstop. The bank has never been accidentally modified.
- **Bank-facts traceability** — Every number in both final outputs traces to a verified M-number. No invented metrics. No rounded figures. The verification tables in cover letters are genuine accountability artifacts.
- **Em-dash/semicolon enforcement** — Both final resumes are clean. The agents performed character sweeps and documented them. The KPMG agent went further by switching date ranges to "to" format to preempt smart-substitution risk.
- **Checkpoint gate** — The file-based mechanic works. Pass 2 checks completeness before proceeding. Both BCG and KPMG demonstrated correct gating behavior.
- **Pass sequence clarity** — A new agent could pick up `pass-sequence.md` cold and understand what each pass reads, writes, and requires.
- **Variant exclusion** — Both changelogs explicitly document which variant was chosen and why the alternative was excluded.
- **Prompt self-containment** — Each prompt reads from disk, not memory. Model switching between passes is safe.

---

## 5. PRE-SUBMISSION CHECKLIST GAPS

The following items should be on a per-application checklist run *after* Google Docs typesetting and *before* submission, but are not currently documented as a consolidated checklist anywhere in the workflow:

- [ ] Open the exported PDF, select all text, copy, paste into a plain text editor. Confirm reading order is correct and no garbled characters appear.
- [ ] Search the Google Doc for `—` (em dash). Count should be zero.
- [ ] Search the Google Doc for `;` (semicolon). Count should be zero.
- [ ] Search the Google Doc for curly/smart quotes. Replace with straight quotes.
- [ ] Confirm the resume fits on exactly one page. If not, apply the documented cut levers in priority order.
- [ ] Confirm no bullet exceeds 2 rendered lines at the chosen font size.
- [ ] Open LinkedIn. Confirm employer names, job titles, and employment dates match the resume exactly.
- [ ] Confirm LinkedIn Skills section overlaps meaningfully with the resume Skills section.
- [ ] If the cover letter uses "Dear Hiring Manager," make one final attempt to find a named contact via LinkedIn, the company's team page, or a Google search.
- [ ] Confirm the company-notes file has been populated with at least one specific company detail (not "None").
- [ ] If any checkpoint answers supplied new facts not yet promoted to the bank, decide whether to promote them before submitting (if the facts would strengthen the application).
- [ ] Update the application tracking log with the company, role, lane, base resume, and date applied.

---

## 6. OPEN QUESTIONS FOR JOSH

1. **target_lane_rubrics_and_bullets.md** — This file is 24 lines and references sections (2.3, 3.3, 4.3) that don't exist in it. Is there a fuller version of this file that was not imported into the project? If so, it should be restored to `knowledge/`.

2. **KPMG OQ-2 fact promotion** — You supplied chi-squared, Cronbach's alpha, standard error, cross-tabulation analysis, Tableau experience, Power BI experience, and a Data Visualization class in OQ-2. Are you ready to promote these into `master_experience_bank.md`? If so, which employer/section should they go under — Carlson (for the survey-analysis stats), Skills (for Tableau/Power BI upgrade from "familiarity" to "experience"), or both?

3. **KPMG cover letter** — The duplicate Revry paragraph issue is real and should be fixed before submission if this application is still active. Do you want the cover letter rewritten, or is this application already submitted/closed?

4. **Page fit** — Have you typeset either the BCG or KPMG resume in Google Docs yet? Did they fit on one page at 10.5pt, or did you need to use the documented cut levers?

5. **LinkedIn alignment** — Have you confirmed that your LinkedIn profile matches the resume's employer names, titles, and dates? The workflow flags this as unverifiable from inside the system.

6. **BCG company-specific detail** — The BCG cover letter's only company-specific detail is paraphrased JD text. Do you have any BCG-specific notes (a contact, an info session, a practice area of interest) that should be added to the company-notes file before submission?

7. **Application tracking log** — Are you maintaining this log manually? Neither BCG nor KPMG appear in it currently. Should it be moved out of the read-only bank into a writable file?

8. **Date range format** — The BCG final uses en-dashes in date ranges (`Jan 2025 – Dec 2025`) while the KPMG final uses "to" (`Jan 2025 to Dec 2025`). Which format do you prefer as the standard?
