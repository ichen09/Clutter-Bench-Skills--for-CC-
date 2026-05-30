---
fresh_as_of: 2026-05-08
trust_level: canonical
philosophy_version: eye@v1.0
addressee: orchestrator
altitude: operational
invalidated_if_modified:
  - scripts/eye/rank.py
  - scripts/eye/purpose.md
  - scripts/eye/refusals.md
  - instructions-registry.yaml
exempt_reason: null
---

# /eye — Ranked drift report at intuition-depth

Cross-file philosophical-drift report over every instruction surface registered in `instructions-registry.yaml`. Where `/health` reports file-level mtime decay, `/eye` reports cross-file coherence: contradictions between CLAUDE.md and the manuals, philosophy-version lag, dependency cycles, undeclared inline prompts in scripts, missing addressee tags, and hardcoded paths. Invoke manually before substantive work, after `/catch-up`, and before any commit that touches identity files. The two compose: `/health` for decay, `/eye` for drift.

## Steps

1. **[main]** Run the pipeline:
   ```bash
   cd "$(git rev-parse --show-toplevel)"
   python3 scripts/2026-04-17_1330_stale-scan.py --all-eyes --json | \
     python3 -m scripts.eye.rank --json-stdin --render
   ```
2. **[main]** Relay the rendered report to Isaac verbatim. Do not summarize — the ranking is the point, and any compression below the eye's own ≤5 cut destroys signal.
3. **[main]** For each surfaced item, offer to act on the `→ Action:` line (most are one-file edits with concrete brainstorm citations).

## What you'll see

The exact markdown shape `scripts/eye/rank.py::render_report` produces:

```
THE EYE — N of 5 ranked drift items   [intuition: 4/4]   [institution: 49 surfaces]

1. [SEV 10] contradiction: <path:line> ↔ <partner_path:line>
   → Action: <recommended action> (cite: BS27)
2. [SEV 9] hardcoded_path: <path:line>
   → Action: Replace with PROJECT_ROOT env or Path(__file__).resolve().parents[N]
...

Self-check: registry fresh_as_of <date>. Eye refused <count> (all N cited).
```

Severity table: contradiction=10, hardcoded_path=9, cycle=8, philosophy_lag=7, inline_prompt_undeclared=6, addressee_issue=5. Maximum five items, sorted by severity desc then path asc.

## What it refuses

See `scripts/eye/refusals.md` for the five load-bearing refusals: ≤5 items per report; every drift cited with `path:line`; halt if `instructions-registry.yaml` is itself stale; coverage gaps are first-class drift; recommendations grounded in a brainstorm-ID or shipped manual.

## When to use

- Manually, on demand, when Isaac wants the current cross-file drift picture.
- At session start, after `/catch-up` orients the new agent and before substantive work begins — drift caught at session-start does not poison the session.
- Before any commit that touches identity files (`CLAUDE.md`, manuals, `purpose.md`, `bootstrap-context.md`, `instructions-registry.yaml`).

## What this is NOT

Not a linter. Not a CI hook. Not a duplicate of `/health` — `/health` reads mtime metadata, `/eye` reads cross-file content coherence. Not a detector itself: the detection layer lives in `scripts/eye/{claims,contradictions,philosophy,inline_prompts,addressee,cycles}.py`, run inside `stale-scan.py --all-eyes`. This command only ranks and renders.

**See also:** `/health` — for file-level mtime decay and frontmatter consistency.
