# AGENTS.md — Resume Tailoring Workflow (Joshua Ekong)

Josh's resume and cover-letter tailoring project. This file is the index. Detailed
rules live under `.agents/rules/` (also symlinked to `.claude/rules/`) and auto-load by path glob when they apply.
Per-pass invocation prompts are in `PROMPTS.md`. `CLAUDE.md` and `GEMINI.md` are symlinks to this
file.

@raw-resume-docs/master_experience_bank.md
@PROMPTS.md

## Folder map

```
resume-buildr/
├── AGENTS.md · CLAUDE.md → AGENTS.md · GEMINI.md → AGENTS.md · PROMPTS.md · skills-lock.json
├── .agents/
│   ├── AGENTS.md       → symlink to ../AGENTS.md
│   ├── rules/          (source of truth for auto-loaded rules — see index below)
│   └── skills/         (source of truth for skills, each in <name>/SKILL.md)
├── .claude/
│   ├── rules/          → symlink to ../.agents/rules
│   ├── skills/         → symlink to ../.agents/skills
│   └── settings.local.json
├── knowledge/          (standards docs — read-only; see inventory below)
├── raw-resume-docs/    (bank read-only + checkpoint_log.md read-write)
├── job-descriptions/   (JD + company-notes files, flat, persistent)
└── resume-outputs/     (p01 analysis, p02 draft, p03 final — writable)
```

### `knowledge/` inventory (read-only standards docs)

| File | Used by |
|---|---|
| `resume_standards.md` | Pass 2 draft, Pass 3 resume audit (overridden by `resume-format.md`) |
| `ats_ai_screening.md` | Pass 3 resume audit |
| `target_lane_rubrics_and_bullets.md` | Pass 1 analysis |
| `cover_letter_standards.md` | Pass 2 cover letter draft, Pass 3 cover letter voice pass |
| `writing_style_profile-josh_v2.md` | Pass 3 cover letter voice pass **only** — never the resume |
| `interview_and_networking.md` | Not used by this workflow. Reference doc for downstream interview/networking prep; kept here intentionally, not orphaned. |

## Hard defaults (always in force, do not bypass)

- **Default-deny writes** to `knowledge/`, `raw-resume-docs/`, and
  `job-descriptions/`. Exceptions listed in `.agents/rules/write-permissions.md`.
  If a task looks like it needs a write here and no exception matches, stop and
  ask Josh.
- **Only source of factual claims** is `@raw-resume-docs/master_experience_bank.md`.
  Never invent, round, estimate, or embellish a number, scope, or claim. Rewording
  for keyword fit is fine; changing numbers, scope, or ownership language is not.
- **`checkpoint_log.md` is staging only** — a fact logged there is not usable in
  a resume or cover letter until Josh promotes it into the bank himself.
- **Human Checkpoint blocks Pass 2.** Josh answers by editing the numbered list at
  the bottom of the `p01` analysis file. If it's blank or short, stop and tell Josh
  exactly which OQ numbers are unanswered — don't ask in chat, don't guess.

## Rule files (load automatically by glob)

Canonical location is `.agents/rules/`. Claude Code reads them via the
`.claude/rules/` symlink — same content, either path resolves. Antigravity IDE (Gemini) reads them directly from `.agents/`.

| File | Triggers on |
|---|---|
| `.agents/rules/write-permissions.md` | `knowledge/**`, `raw-resume-docs/**`, `job-descriptions/**` |
| `.agents/rules/naming-convention.md` | `job-descriptions/**`, `resume-outputs/**` |
| `.agents/rules/bank-facts.md` | `resume-outputs/p02/**`, `resume-outputs/p03/**` |
| `.agents/rules/resume-format.md` | `resume-outputs/p02/*_draft.md`, `resume-outputs/p03/*_final.md` |
| `.agents/rules/cover-letter-voice.md` | `resume-outputs/p03/*_cover_letter_final.md` |
| `.agents/rules/checkpoint.md` | `resume-outputs/p01/*_analysis.md`, `resume-outputs/p02/**` |
| `.agents/rules/pass-sequence.md` | reference — read when unclear which pass owns what |
| `.agents/rules/model-effort.md` | reference — read when picking `/model` and effort |

## Skills

Skills live in `.agents/skills/<name>/SKILL.md`, symlinked into `.claude/skills/`.
Claude and Gemini load matching ones on their own; front-load each description within ~250
characters. Which skill runs at which pass is in
`.agents/rules/pass-sequence.md`.

## Prompts

Copy-paste per-pass invocations from `PROMPTS.md`. Run in order. Do not batch.
