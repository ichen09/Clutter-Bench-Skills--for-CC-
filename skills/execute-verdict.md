---
fresh_as_of: 2026-05-27
trust_level: operational
invalidated_if_modified:
  - scripts/2026-04-15_1534_parse-verdict-brief.py
  - scripts/2026-04-15_1534_execute-verdict.py
  - scripts/2026-04-15_1534_verify-implementation.py
---

# /execute-verdict — Implement a confirmed brief

After `/trace` produces a brief and Isaac marks it confirmed, this executes the verdict.

## Steps

1. Find the confirmed brief in `memory/intake/briefs/`, or ask Isaac which one.
2. Parse findings: `python3 scripts/2026-04-15_1534_parse-verdict-brief.py BRIEF_PATH --json`
3. Not confirmed yet? Stop — brief needs a verdict first.
4. Show Isaac the parsed summary. "Ready to create workspace?"
5. Create workspace: `python3 scripts/2026-04-15_1534_execute-verdict.py BRIEF_PATH`
6. Review generated agent prompts with Isaac, then spawn implementation agents (one per change cluster, 3-5 parallel). Agents work on workspace copies, never production files.
7. Verify: `python3 scripts/2026-04-15_1534_verify-implementation.py WORKSPACE_PATH`
8. Present diff. Isaac says apply, reject, or change.
9. If apply: copy workspace files to production. Final verification. Record: `--record WORKSPACE_PATH`.

## Safety

All changes in workspace copies first. Improvement decisions always draft as "hold." Verification before Isaac sees the diff.
