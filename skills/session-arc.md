# /session-arc — One improvement cycle, start to finish

Detect drift → fix it → review → verify → commit. Chains `/eye` → `/improve` → code review → health check → commit.

## Steps

1. **Run `/eye`.** Zero findings at severity >=7? Stand down.

2. **Tone check.** Grep tracking docs for L-026 forbidden words. Fix overcertainty before anything else — it actively misleads cold readers.

3. **Run `/improve`** on the top finding. Pick what would most mislead a future session. Don't open a PR — the arc continues.

4. **Review the diff.** `git diff HEAD~1`. Fix any bugs before continuing.

5. **Re-check tone** on touched files only. Don't introduce new overcertainty while fixing drift.

6. **Health check.** `python3 scripts/2026-04-17_1330_stale-scan.py`. Did the fix increase staleness? Fix the chain.

7. **Commit.** Cite the finding, the fix, and the health delta.

8. **Loop or stop.** Running under `/loop`? Schedule next cycle. Otherwise wait for Isaac.

## Tone rule

Every claim in commits and reports uses the L-026 template: `[Observation] (N=<count>, <conditions>): <what happened>`. No "confirmed," "headline," "publishable."

## When

After `/boot`, to run one improvement cycle. With `/loop 30m /session-arc` for autonomous runs. When Isaac says "improve something" without specifying what. Not a substitute for directed work.
