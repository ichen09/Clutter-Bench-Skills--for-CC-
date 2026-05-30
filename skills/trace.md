---
fresh_as_of: 2026-05-27
trust_level: operational
invalidated_if_modified:
  - scripts/2026-04-14_1532_run-judgement-day.py
  - memory/intake/INDEX.md
---

# /trace — Process pending intake notes

Scan `memory/intake/` for unprocessed notes and trace their implications through the system.

## Steps

1. Run: `python3 scripts/2026-04-14_1532_run-judgement-day.py`
2. No pending notes? Tell Isaac "Nothing to process. Drop context with /intake first."
3. Notes found: script detects affected surfaces (25 surfaces, 15 concepts), generates a brief in `memory/intake/briefs/`, marks the note processed.
4. Show Isaac the surface hits and concept hits.
5. Ask: "Want me to run the 5 implication-tracing agents?" If yes, spawn: contradiction finder, missing capability finder, assumption change detector, blast radius mapper, priority assessor.
6. Update the brief with findings. Present to Isaac with verdict checkboxes.

## When

After `/intake`. At session start to check for notes dropped between sessions. When Isaac asks "what does X mean for the system?"
