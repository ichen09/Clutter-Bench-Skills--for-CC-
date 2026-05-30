---
fresh_as_of: 2026-05-09
trust_level: operational
philosophy_version: identity@v1.2
addressee: orchestrator
altitude: operational
invalidated_if_modified:
  - docs/exemplars/self-improvement-arc.md
  - brainstorm/42_2026-05-09_self-improving-harness-pointed-at-itself.md
  - CLAUDE.md
  - scripts/eye/rank.py
exempt_reason: null
---

# /improve — Run a self-improvement arc

Run one self-improvement arc on the current top drift finding. Reads the latest `/eye` output, picks the smallest top-finding (severity-weighted), forms explicit hypotheses, spawns blind verifiers, builds the fix, runs cold-reader test, encodes the lesson, opens a PR. Composes with `/loop`: `/loop 30m /improve` runs unattended at 30-minute cadence with Isaac-checkpoint gates at merge time.

**Reference exemplar:** `docs/exemplars/self-improvement-arc.md` (two variants: small arc 5-min/1-PR + large arc 5-hour/5-PR). The arc's wave count adapts to scope — don't over-structure small arcs; don't under-encode large ones.

**Background story:** `brainstorm/42_2026-05-09_self-improving-harness-pointed-at-itself.md` (the discovery + open questions about autonomous-loop wiring).

## When to use

- After invoking `/eye` and seeing a finding worth fixing.
- At session start, after `/saturate` (or `/catch-up`), to fire one improvement cycle on whatever the eye found.
- Composed with `/loop` for unattended improvement runs (`/loop 30m /improve`).

## When NOT to use

- When `/eye` reports zero findings of severity ≥7 (nothing worth firing the arc on).
- When you're in active turn-by-turn collaboration with Isaac (the arc is for autonomous-or-semi-autonomous fixes, not pair-programming).
- When you've already run `/improve` this session and there's no fresh drift (re-running picks the same finding, wastes cycles).

## Steps

### 1. Read latest `/eye` output

```bash
cd "$(git rev-parse --show-toplevel)"
python3 scripts/2026-04-17_1330_stale-scan.py --all-eyes --json | python3 -m scripts.eye.rank --json-stdin --render > /tmp/eye-output.md
cat /tmp/eye-output.md
```

If output reports "0 drift items" or all top-5 are severity <7, **stand down** — nothing worth running an arc on. Surface to Isaac and stop.

### 2. Pick the smallest top-finding

Among the top-5, prefer:
- **Lower wave count** if scope allows (single-file fixes faster than cross-file).
- **Lower blast radius** (one cluster affected vs many).
- **Higher severity** when blast radius is equal (severity 10 over severity 7).
- **Diverse cluster** if last `/improve` arc fixed a finding in cluster X — pick a different cluster this time to avoid cluster-bias compounding.

Output a `selected_finding` line citing path:line + severity + cluster.

### 3. Form explicit hypotheses BEFORE verifying

Per CLAUDE.md "After fan-out, the cold-reader test" + BS41 lesson: commit hypotheses BEFORE testing them. Write down:
- 2-4 hypotheses about the finding's root cause.
- For each: prior strength (high/medium/low), and the smallest resolve command that would confirm/refute.

This is visible commitment. If a prior is wrong, you learn something — better than retrofitting the verdict to match what the source actually says.

### 4. Spawn blind verifiers

Per `docs/exemplars/verifier-output.md` Variant B (hypothesis-checker stance):
- Spawn 1-3 Sonnet 4.6 subagents in parallel, one per hypothesis (or one per file if hypotheses cluster).
- Each verifier: read-only, file:line citations, returns verdict (which hypothesis survived) + smallest fix specification.

If verifiers disagree, escalate to Isaac. Otherwise: synthesis is the verified hypothesis.

### 5. Build the fix

Per CLAUDE.md "Going deep" + the self-improvement-arc exemplar:
- **Integration step?** Orchestrator-direct. Don't fan out the substance work.
- **Structurally parallel?** Spawn workers per parallel scope, with Block 7 EXEMPLARS in every prompt.
- **One-file fix?** Just edit it.

Build outputs: file edits + tests if applicable + verification command.

### 6. Cold-reader test (large arc only; skip for small arcs)

Per `docs/exemplars/verifier-output.md` Variant A (cold-reader stance):
- Spawn 1 Sonnet 4.6 with NO project context.
- Give it the artifact and its `purpose.md` / `refusals.md` / spec only.
- Ask it to react: does the shape work for a fresh reader?

If cold-reader catches an issue, treat as a sub-finding. Either fix it inline (small additional edit) or file it as the next arc's input.

For small arcs (single-file fix, ~10 LOC, no new substance), skip this step — the cycle's overhead exceeds the value.

### 7. Encode the lesson

If the fix REVEALED a recurring pattern (median-aesthetic from fan-out, INDEX coverage gap, etc.), encode it. Three surfaces, in priority order:
- **CLAUDE.md section** — operating rule, narrative voice, ~150 LOC. Highest leverage (auto-read at session start).
- **docs/exemplars/<artifact>.md** — 2-3 worked variants. Required if the artifact type is new.
- **brainstorm/<NN>_<date>_<slug>.md** — the story + open questions. Optional but recommended for arcs that surface a new pattern.

If the fix DOESN'T reveal a pattern (it's a one-off finding, no recurrence shape), skip encoding. Don't over-canonize.

### 8. Ship

- Branch: `claude/<arc-slug>-<date>`.
- Commit message: detailed, references PR-numbers if part of a cascade, cites BS-numbers for any encoded discipline.
- Push.
- Open PR via `mcp__github__create_pull_request`.
- **Isaac-checkpoint:** by default, STOP at PR creation. Surface the PR URL. Wait for Isaac to merge or redirect. (Future: `--auto-merge` flag if cold-reader passed and CI is green; not yet shipped.)

After Isaac merges:
- Pull main + sync local + roll main-backup.
- If running under `/loop`, the next cycle picks up.

## Failure modes

If your arc fails any of these, surface the failure rather than continuing:

- **Verifiers disagree on which hypothesis survived** → escalate to Isaac. Don't ship a fix on contested ground.
- **Cold-reader catches an issue you can't address inline** → file as next arc's input, ship the partial fix only if Isaac approves the deferral.
- **Cost estimate exceeds budget** → stand down. Surface to Isaac with the cost projection.
- **Three consecutive arcs ship the same kind of fix** → saturation; surface to Isaac that the eye's ranker may need tuning, don't auto-continue.
- **`/eye` itself is stale** (registry's `fresh_as_of` is older than CLAUDE.md / mission.md / etc.) → re-run `/saturate` then `/eye` before picking a finding.

## Output

A PR open on GitHub, with a body that:
- Cites the eye finding that triggered the arc (`[SEV N] category: path:line`)
- Lists the hypotheses tested + which survived
- Names the fix (file edits + LOC)
- Includes cold-reader test verdict (if large arc)
- Cites any encoded lessons (CLAUDE.md section, exemplar, brainstorm)
- Links the breadcrumb path

## Composition with `/loop`

```bash
# Run /improve every 30 minutes, unattended, Isaac-checkpoint at merge time
/loop 30m /improve

# Stop the loop
# (Isaac sends "stand down" or kills the loop manually)
```

Per BS42 §"Open questions": loop termination conditions are not yet automatic. Don't run `/loop /improve` for >24 hours without checking in. Cost cap, drift-saturation, and Isaac-redirect-handling all need future-session work.

## Why this skill exists

Per BS28 "infinite-time-self target" — building what Isaac would have built if he had infinite time. The discovery process tonight (PR #11→#17) demonstrated the loop CAN run on itself when:
1. The drift detector is autonomous (the eye)
2. The protocol is encoded (this skill body + the exemplar + BS42)
3. Isaac-checkpoint gates compress to "approve PR or redirect" at merge time

Compute multiplies by running more arcs, not bigger ones. This skill makes that compounding structural rather than human-driven.

The asymmetry resolves: 1 hour Isaac approves PRs, 23 hours compute runs arcs. The framework gets better at being itself, on a schedule, without requiring Isaac's presence to drive each step.

## Maintenance

Update this skill when:
- The arc's structural shape changes (new wave added, wave reordered).
- The exemplar at `docs/exemplars/self-improvement-arc.md` adds a new variant.
- The eye's severity ranking changes (current: contradiction=10, hardcoded_path=9, cycle=8, philosophy_lag=7, inline_prompt_undeclared=6, addressee_issue=5).
- BS42's open questions get resolved (especially: PR-merge gate granularity, loop termination, lesson encoding without Isaac).

The frontmatter `invalidated_if_modified` flags this skill stale when its dependencies change.
