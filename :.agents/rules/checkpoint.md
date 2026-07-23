---
description: Human Checkpoint mechanism. Numbered "Open Questions for Josh" list at the bottom of the p01 analysis file. Josh answers in the file, not chat. Pass 2 must verify completeness before proceeding.
globs: ["resume-outputs/p01/*_analysis.md", "resume-outputs/p02/**"]
---

# Human Checkpoint

The checkpoint is **file-based, not a chat exchange.** Between Pass 1 and Pass 2,
Josh answers by editing the `p01` analysis file directly.

## Pass 1 output requirement

Pass 1 ends the analysis file with a numbered `## Open Questions for Josh`
section, followed by:

```
---
*Next gate: Human Checkpoint (blocks Pass 2). Awaiting Josh's answers below.*

1. 
2. 
[one blank numbered line per OQ, in the same order]
```

At minimum the OQs cover: lane/base resume confirmation, any content gaps that
need a real fact, any logistics/willingness items relevant to this JD, whether to
use pivot framing (bank Section 6) if relevant, whether a cover letter is needed,
and a catch-all for any new fact Josh wants considered.

Do not ask the questions in chat. Stop after writing the file.

## Josh's answer format

Josh writes short numbered answers under `Awaiting Josh's answers below.`, one
per line, matching OQ numbers by position — answer 1 → OQ-1, answer 2 → OQ-2, and
so on.

## Pass 2 gating

Before doing anything else, Pass 2 opens the `p01` file, finds the numbered answer
list, and checks:

- Does the list exist?
- Is it at least as long as the OQ count?
- Is any numbered position blank?

If any check fails, stop and tell Josh exactly which OQ numbers are still
unanswered. Do not proceed on assumptions. Do not re-ask the questions in chat.

## New facts surfaced in answers

If an answer supplies a real new fact not currently in the bank:

1. Append it to `raw-resume-docs/checkpoint_log.md` as a candidate addition, with
   the OQ number, the JD it came up under, and the fact as stated.
2. Flag it to Josh in chat as a candidate for `master_experience_bank.md`
   promotion.
3. Do **not** use it in the Pass 2 resume or cover letter. Staging is not
   verification — see `.agents/rules/bank-facts.md`.

The only exception is a stated willingness/logistics fact (`relocating to
Chicago`, `open to travel`) — that can appear in the cover letter, labeled as
such in the Verification notes / Fact-rule check subsection. It still does not
enter the resume without going through the bank.
