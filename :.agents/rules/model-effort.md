---
description: Recommended /model and effort per pass for Claude Code, Codex, and Antigravity CLI. Sourced from model-effort-selection-guide-v1.0.md (verified 2026-07-03). Reference — consult when picking a model/effort for a pass.
globs: []
---

# Model & Effort per Pass

Sourced from `model-effort-selection-guide-v1.0.md` (last verified 2026-07-03).
Re-check that document if this feels stale, per its §8.

## Recommended settings

| Pass | Claude Code | Codex | Antigravity CLI |
|---|---|---|---|
| Prompt 0 — Log JD | Haiku 4.5 | gpt-5.4-mini, low | Flash (Low) |
| Pass 1 — Analysis | Sonnet 5, medium | gpt-5.5, medium | Flash (Medium) |
| Human Checkpoint | N/A (manual) | — | — |
| Pass 2 — Rewrite | Sonnet 5, **high** | gpt-5.5, medium | Flash (Medium) |
| Pass 3 — Audit + Voice | Sonnet 5, **high** | gpt-5.5, medium | Flash (Medium) |

**Why Pass 2 and Pass 3 sit at `high` on Claude Code:** both chain multiple skills
against a dense rule set (bank-facts, resume-format, cover-letter-voice), and Pass
3 runs two parallel tracks with an exacting self-check on the cover letter. That
matches the guide's `Shell/tool orchestration, multi-step tool chains` row, not
the generic `everyday coding` default.

## Escalation ladder (decide between runs, not mid-pass)

- Pass 1 missed a bank fact, mis-marked a GAP, or missed a hard rule → Sonnet 5,
  high next run.
- Pass 2 or Pass 3 had a rule-compliance miss caught only downstream → Sonnet 5,
  xhigh next run. If it happens repeatedly, switch to Opus 4.8 xhigh rather than
  raising effort again.

## Rule of thumb (from guide §4)

> Start at the cheapest model that can plausibly understand the task, not the
> cheapest model that can merely attempt it. Raise effort when the model is close
> but under-validating. Switch models when it misunderstands the repo, patches
> symptoms instead of root causes, or needs stronger judgment. Stop escalating
> when the blocker is environment, missing requirements, flaky tests, quota, or
> unavailable context rather than reasoning — no model/effort change fixes those.
