# /boot — Session start

One command to orient. Read the minimum files, check health, report what's next.

## Steps

### 1. Orient

Read in order:
1. `purpose.md` — why this project exists
2. `MEMORY.md` (auto-memory) — durable architecture only, NOT current state
3. `emotions/.internal/STATUS.md` — current state, authoritative (`fresh_as_of`-governed)
4. `emotions/tasks/lessons.md` — last 10 lessons
5. `corrections-log.md` — recent corrections. Cite >=1 relevant C-NNN or say "none apply."

**Staleness cross-check (auto-memory loads before this skill and rots):** compare auto-memory's stated date against `STATUS.md`'s `fresh_as_of`. If auto-memory is older, **trust STATUS.md** for current state and flag the gap in the output. Auto-memory is durable architecture; STATUS.md is what's actually happening.

Output: 3-sentence orientation. What the project is, where we left off, what's next.

### 2. Load

```bash
ls -t emotions/tasks/ | head -5
```

Read that workspace's `index.md` + most recent intake note. 5-8 files max, not the whole repo.

### 3. Health

Run `python emotions/bench/scripts/doc_health.py` if in emotions mode. Only report actually-broken stuff. Skip cosmetic counts.

### 4. Tone

Grep for L-026 forbidden words in files modified since last session. Don't audit the whole repo.

### Output

```
SESSION BOOT — [date]
Orientation: [3 sentences]
Last session: [workspace] — [1-sentence summary]
Queued: [next actions]
Memory: [auto-memory date vs STATUS fresh_as_of — "aligned" or "STALE by N days, trusting STATUS"]
Health: [N broken files] [N invalid deps]
Tone: [N overcertain instances]
Ready.
```
