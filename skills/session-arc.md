# /session-arc — Autonomous improvement cycle chaining 5 skills

Run the full session improvement loop: detect drift → fix it → review the fix → verify nothing broke → commit. Chains `/eye` → `/improve` → `/code-review` → `/health` → commit.

## Steps

### 1. Run `/eye`

```bash
python3 scripts/2026-04-17_1330_stale-scan.py --all-eyes --json | python3 -m scripts.eye.rank --json-stdin --render
```

If zero findings at severity ≥7: stand down. Report "nothing worth fixing" and stop.

### 2. Run `/tone-audit`

Scan all tracking docs for L-026 forbidden words BEFORE doing any other work. If overcertain instances exist from prior sessions, fix them first — they're higher priority than eye findings because they actively mislead cold readers.

### 3. Run `/improve` on the top finding

From eye output OR tone-audit output, pick the finding that would most mislead a future session's decision. Prefer:
- Claims that would change what a future session builds (over cosmetic metadata)
- Contradictions between files a session actually reads (STATUS, MEMORY, lessons) over files it skips
- Overcertain language on unreplicated results (over frontmatter mismatches)

Follow the `/improve` skill steps: pick → hypothesize → verify → fix → encode lesson if pattern.

Do NOT open a PR — the arc continues.

### 4. Run `/code-review` on the fix

```bash
git diff HEAD~1
```

Review the diff. If any CONFIRMED bugs found, fix them before continuing.

### 5. Run `/tone-audit` again on touched files

Re-scan only the files modified in this arc. If any new overcertain language was introduced by the fix itself, correct it before committing.

### 6. Run `/health` to verify nothing broke

```bash
python3 scripts/2026-04-17_1330_stale-scan.py
```

Compare stale count before vs after. If the fix increased staleness, fix the chain.

### 7. Commit and report

Commit with a message citing the eye finding, the fix, and the code-review/health results.

Report to Isaac:
```
/session-arc complete:
  Finding: [SEV N] category: path:line
  Fix: <what changed>
  Code review: <N findings, M fixed>
  Health delta: <stale count before → after>
  Commit: <sha>
```

### 8. Loop decision

If Isaac said `/loop /session-arc`: call ScheduleWakeup with 1800s delay and re-enter.
Otherwise: stop and wait for Isaac.

## Tone discipline

Every claim in the commit message and report uses the L-026 tracking-doc template:
`[Observation] (N=<count>, <conditions>): <what happened>`

No "confirmed," "headline," "publishable," "the paper." Numbers only.

## When to use

- At session start after `/catch-up`, to run one improvement cycle
- Composed with `/loop` for autonomous improvement: `/loop 30m /session-arc`
- When Isaac says "improve something" without specifying what

## What this is NOT

- Not a substitute for directed work. If Isaac has a specific task, do that instead.
- Not unlimited. One cycle per invocation. `/loop` handles repetition.
