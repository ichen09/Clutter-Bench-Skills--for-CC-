---
fresh_as_of: '2026-04-17'
invalidated_if_modified:
- scripts/2026-04-15_1534_parse-verdict-brief.py
- scripts/2026-04-15_1534_execute-verdict.py
- scripts/2026-04-15_1534_verify-implementation.py
- memory/intake/briefs/INDEX.md
trust_level: operational
exempt_reason: null
---

# Execute a confirmed Judgement Day verdict

After a brief in `memory/intake/briefs/` has been reviewed and confirmed by Isaac, execute the verdict to implement the recommended changes.

## Steps

1. Ask Isaac which brief to execute, or find the most recent brief with a confirmed verdict
2. Run the parser to extract findings:
   ```
   python3 scripts/2026-04-15_1534_parse-verdict-brief.py BRIEF_PATH --json
   ```
3. If verdict is not "confirmed" or "redirected", stop and tell Isaac the brief needs a verdict first
4. Show Isaac the parsed findings summary and ask: "Ready to create workspace and start implementation?"
5. If yes, run the orchestrator:
   ```
   python3 scripts/2026-04-15_1534_execute-verdict.py BRIEF_PATH
   ```
6. The script creates a task workspace with:
   - Source file copies (agents edit these, never production files)
   - Improvement events for each finding
   - Implementation agent prompts grouped by blast radius cluster
7. Review the generated agent prompts with Isaac
8. Spawn implementation agents — one per change cluster (typically 3-5 agents in parallel)
   - Each agent works ONLY on files in the workspace source-files/ directory
9. After agents complete, run verification:
   ```
   python3 scripts/2026-04-15_1534_verify-implementation.py WORKSPACE_PATH
   ```
10. If verification passes, present the diff:
    ```
    python3 scripts/2026-04-15_1534_execute-verdict.py --present-diff WORKSPACE_PATH
    ```
11. Isaac reviews the diff and says "apply", "reject", or requests changes
12. If apply: copy modified files from workspace/source-files/ to production locations
13. Run final verification on production files
14. Record execution:
    ```
    python3 scripts/2026-04-15_1534_execute-verdict.py --record WORKSPACE_PATH
    ```

## When to use

- After `/trace` produces a brief and Isaac marks it confirmed (checks the `[x] Confirmed` box)
- When Isaac says "execute", "do it", "proceed" after reading a brief
- When Isaac wants to act on Judgement Day findings

## Safety

- All changes happen in workspace copies first — production files are never touched until Isaac approves
- Improvement decisions are always drafted as "hold" — never auto-promoted
- Verification runs before Isaac sees the diff
