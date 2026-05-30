---
fresh_as_of: '2026-04-17'
invalidated_if_modified:
- scripts/2026-04-14_1532_run-judgement-day.py
- memory/intake/INDEX.md
- memory/intake/briefs/INDEX.md
trust_level: operational
exempt_reason: null
---

# Run implication tracer on pending intake notes

Scan `memory/intake/` for unprocessed notes and trace their implications through the system.

## Steps

1. Run: `python3 scripts/2026-04-14_1532_run-judgement-day.py`
2. If no unprocessed notes found, tell Isaac "No pending intake notes. Drop context with /intake first."
3. If notes found, the script will:
   - Detect which system surfaces are affected (25 surfaces, 15 concepts)
   - Generate a brief template in `memory/intake/briefs/`
   - Mark the note as processed
4. Show Isaac the surface hits and concept hits from the script output
5. Ask: "Want me to run the 5 implication-tracing agents for deep analysis?"
6. If yes, run the script with `--show-prompts` flag, then spawn 5 agents:
   - Contradiction finder
   - Missing capability finder
   - Assumption change detector
   - Blast radius mapper
   - Priority assessor
7. Collect agent results and update the brief with real findings
8. Present the completed brief to Isaac with the verdict checkboxes

## When to use

- After `/intake` captures new context
- At session start to check for notes dropped between sessions
- When Isaac says "what does X mean for the system?"
