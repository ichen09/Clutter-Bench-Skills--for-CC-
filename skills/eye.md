---
fresh_as_of: 2026-05-27
trust_level: canonical
invalidated_if_modified:
  - scripts/eye/rank.py
  - scripts/eye/purpose.md
  - scripts/eye/refusals.md
  - instructions-registry.yaml
---

# /eye — Cross-file drift report

Finds contradictions between CLAUDE.md and manuals, philosophy-version lag, dependency cycles, undeclared inline prompts, missing addressee tags, hardcoded paths. Top 5 items, ranked by severity.

Not a linter (that's `doc_health.py`). The eye reads cross-file coherence; linters read single-file properties.

## Run

```bash
cd "$(git rev-parse --show-toplevel)"
python3 scripts/2026-04-17_1330_stale-scan.py --all-eyes --json | \
  python3 -m scripts.eye.rank --json-stdin --render
```

Relay the output verbatim — the ranking is the point. For each item, offer to act on the `-> Action:` line.

## Output shape

```
THE EYE — N of 5 ranked drift items   [intuition: 4/4]   [institution: 49 surfaces]

1. [SEV 10] contradiction: <path:line> <-> <partner_path:line>
   -> Action: <recommended action> (cite: BS27)
...
```

Severity: contradiction=10, hardcoded_path=9, cycle=8, philosophy_lag=7, inline_prompt_undeclared=6, addressee_issue=5.

## When

Before substantive work (catches drift before it poisons the session). Before commits touching identity files. On demand when Isaac wants the drift picture.
