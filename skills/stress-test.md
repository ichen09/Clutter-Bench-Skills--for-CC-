---
fresh_as_of: 2026-05-27
trust_level: operational
addressee: orchestrator
invalidated_if_modified:
  - emotions/bench/scripts/doc_health.py
---

# /stress-test — Find what's drifted

Cold-read a system from multiple angles. Fix what's broken. Verify the fixes. Build a linter so you don't have to do this again.

Refined through 22 hypothesis tests. Works on docs and code.

## When

After reorganizations, before shipping, when drift has accumulated. Not for <5 files (just read them) or same-session work (nothing's had time to rot).

## The move

`/stress-test [target] [--rounds 3] [--angles 10]`

Target defaults to whatever STATUS.md describes. If no STATUS.md, use the dir with the most recent git activity.

**Cost heads-up:** ~3-5 agent spawns for <50 files (depth mode), ~45 for 50+ files (breadth mode). Mention this before firing.

### 0. Baseline

Branch off (`git checkout -b stress-test/<slug>`). Run `doc_health.py` or count findings manually. This is your before-picture.

### 1. Design angles

Each angle = one sharp question with a falsifiable yes/no check against disk. "Is this good?" finds nothing. "Does each Next Action item appear in git log?" finds real bugs.

**5 mandatory categories:** cold bootstrap, write-path, broken references, redundancy, structural accuracy. For code: translate vocabulary (links→imports, redundancy→duplicated logic, structural accuracy→does the docstring match the module).

**Fill remaining budget** from: findability, archive discoverability, 3-month pre-mortem, independence, overcertainty. Scale to target: 5-7 for <15 files, 8-12 for 15-50, 10-15 for 50+.

**Always add one leaf-node angle.** "What file here has zero incoming references?" Traversal can't find disconnected nodes. Enumeration can.

### 2. Run

**<50 files — depth-first.** One deep reader applies all angles sequentially. One reader finds cross-file contradictions that parallel agents can't (requires reading 3+ files to notice evolution over time). Add 2-3 parallel agents only for leaf-node enumeration and disk-vs-doc verification.

**50+ files — breadth-first.** Full parallel army, one angle each, all Opus.

### 3. Synthesize

Scorecard table (angle / grade / key finding). Categorize into 6 drift types:

| Type | What | Fix shape |
|------|------|-----------|
| Mechanical | broken links, wrong paths | grep + sed |
| Semantic | contradictions, stale dates | sync sources |
| Structural | nav docs don't match disk | rewrite |
| Completeness | absent sections | add what's missing |
| Dependency | cross-file contracts diverged | update both sides |
| Behavioral | code changed, test didn't | run the code |

### 4. Fix

**Mechanical stuff (<10 edits): just do it yourself.** 2.6x faster than spawning agents for path swaps and date bumps.

**Judgment stuff: spawn fixer agents.** One per file cluster, no overlaps. After fixers finish, spawn one agent to diff-check for cross-file contradictions (fixers updating one file while another still references the old value — this actually happened).

### 5. Verify

Re-run same angles with fresh agents. FAIL→PASS = fixed. PARTIAL→PARTIAL = structural (design decision, not a bug). New finding = fixer introduced it.

### 6. Stop or continue

Stop when ≤2 new issues and all structural. Continue if mechanical/semantic drift remains. Stand down after 3 rounds with no improvement on a PARTIAL.

**All-PASS gates:** (1) Round 1 all-PASS = angles too soft, redo. (2) Angles must compare ≥2 data sources, not test trivially true properties.

### 7. Build the linter

If ≥3 recurring issue types, build or extend an automated checker. Army discovers what to check. Linter remembers. Army runs once after structural change (~$12-18). Linter runs every session ($0, 2 seconds).

### 8. Commit

Commit everything. Compare baseline to final state. That delta is the deliverable.

## Blind spots

- **Test fixtures** — can't tell planted rot from real rot. Check for manifest files before fixing.
- **Clean systems** — prior-FAIL gate may flag genuinely clean targets as INCONCLUSIVE. Trust sharp binary angles.
- **Audience drift** — right content, wrong reader. Needs human judgment.
- **Oscillation** — round N fixes X→Y, round N+1 reverts Y→X. Watch for it.
