---
fresh_as_of: 2026-05-30
trust_level: operational
invalidated_if_modified:
  - docs/exemplars/self-improvement-arc.md
  - scripts/eye/rank.py
---

# /improve — Fix the top drift finding

Run `/eye`, pick the worst finding, hypothesize the root cause, verify, fix, encode the lesson if it's a pattern. One arc, one PR.

Composes with `/loop`: `/loop 30m /improve` runs unattended. Isaac approves PRs at merge time.

Exemplar: `docs/exemplars/self-improvement-arc.md` (small 5-min arc + large 5-hour arc).

## Steps

### 1. Get the finding

```bash
cd "$(git rev-parse --show-toplevel)"
python3 scripts/2026-04-17_1330_stale-scan.py --all-eyes --json | python3 -m scripts.eye.rank --json-stdin --render
```

Nothing at severity >=7? Stand down.

### 2. Pick one

Prefer: smallest scope, highest severity, different cluster than last arc.

### 3. Hypothesize before verifying

Write 2-4 hypotheses about the root cause. For each: prior strength + one resolve command. Commit these BEFORE checking. Wrong priors teach more than retrofitted verdicts.

### 4. Verify

Spawn 1-3 blind verifiers (read-only, file:line citations, which hypothesis survived). Verifiers disagree? Escalate to Isaac.

### 5. Fix

One-file fix? Just edit it. Multi-file? Spawn workers per scope. Don't fan out substance work — the orchestrator does integration.

### 6. Cold-reader test (large arcs only)

Spawn one agent with NO project context. Give it the artifact + spec. "Does this work for a fresh reader?" Skip for single-file fixes.

### 7. Encode (if pattern)

Fix revealed a recurring pattern? Encode it in CLAUDE.md, an exemplar, or a brainstorm doc. One-off fix? Skip. Don't over-canonize.

### 8. Ship

Branch, commit, push, open PR. Stop at PR creation — Isaac merges or redirects.

## Stop conditions

- Verifiers disagree → escalate
- 3 consecutive arcs fix the same category → saturation, surface to Isaac
- Cost estimate exceeds budget → stand down (surface the projection to Isaac)
- `/eye` is stale → re-run before picking
