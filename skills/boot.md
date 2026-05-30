# /boot — Session start (combines catch-up + saturate + health + tone-audit)

One command to start a session. Replaces the 4-command sequence of /catch-up → /saturate → /health → /tone-audit.

## Steps

### Phase 1 — Orient (what /catch-up did)

Read these files in order, extract the "where we are" from each:
1. `purpose.md` — why this project exists (30 seconds)
2. `MEMORY.md` — latest state block (what was done last)
3. `emotions/.internal/STATUS.md` — current bench status
4. `emotions/tasks/lessons.md` — L-019+ (recent lessons, scan last 10)
5. `corrections-log.md` — any recent corrections

Output: 3-sentence orientation. What the project is, where we left off, what's next.

### Phase 2 — Load (what /saturate did, but targeted)

Read the task workspace index for the most recent task:
```bash
ls -t emotions/tasks/ | head -5
```
Read that workspace's `index.md`. Read the most recent session log. Read the most recent intake note.

Do NOT read all 10,226 files. Read the 5-8 files that tell you what happened last session and what's queued.

### Phase 3 — Check (what /health did, but useful)

Run the stale scanner but ONLY report:
- Files that changed since last session AND have stale deps (actually broken, not cosmetically stale)
- Invalid deps (broken references — these are real bugs)

Skip: unmanaged count (meaningless), exempt count (irrelevant), total stale count (misleading).

### Phase 4 — Tone check (what /tone-audit did)

Run the forbidden-word grep but ONLY on files modified since last session. Don't audit the whole repo — audit what's fresh.

### Output

```
SESSION BOOT — [date]
Orientation: [3 sentences]
Last session: [workspace name] — [1-sentence summary]
Queued: [next actions from last workspace]
Health: [N actually-broken files] [N invalid deps]
Tone: [N overcertain instances in recent files]
Ready.
```

## What this replaces

- `/catch-up` (1,528 words of skill definition for "read 7 files")
- `/saturate` (1,131 words for "read everything" — too broad)
- `/health` (reports 2,536 unmanaged files nobody cares about)
- `/tone-audit` on its own (useful but should be scoped to recent work)

## What this is NOT

- Not a full context load. A session that needs deep history reads specific files.
- Not a substitute for Isaac saying what to work on. This orients; Isaac directs.
