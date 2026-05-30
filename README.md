# Clutter-Bench Skills

A set of [Claude Code](https://claude.com/claude-code) slash-command skills extracted from the Clutter-Bench research project — a benchmark studying how prompting techniques affect a model's authority/calibration judgment over cluttered substrates.

These skills encode a self-improving, drift-resistant agent workflow: orient at session start, detect cross-file philosophical drift, run autonomous improvement arcs, capture context, and publish results. They're shared here as reference patterns for anyone building agentic harnesses on top of Claude Code.

> **Note:** Each skill is a `.md` command file dropped into `.claude/commands/`. They reference project-internal scripts and paths (e.g. `scripts/eye/rank.py`, `emotions/bench/`), so they are published as *patterns to study and adapt*, not as a drop-in package. Adapt the paths to your own repo.

## The skills

| Skill | What it does |
|-------|--------------|
| [`boot`](skills/boot.md) | Session start — one command combining catch-up + saturate + health + tone-audit. Orients a fresh agent in seconds. |
| [`eye`](skills/eye.md) | Ranked cross-file philosophical-drift report. Where `/health` reports file-level decay, `/eye` reports cross-file coherence: contradictions, philosophy-version lag, dependency cycles, hardcoded paths. |
| [`improve`](skills/improve.md) | Run one self-improvement arc on the top drift finding — hypothesize, spawn blind verifiers, build the fix, cold-reader test, encode the lesson, open a PR. Composes with `/loop` for unattended runs. |
| [`session-arc`](skills/session-arc.md) | Autonomous improvement cycle chaining 5 skills: detect drift → fix → review → verify → commit. |
| [`trace`](skills/trace.md) | Implication tracer — scans intake notes and traces their implications through the system via parallel analysis agents. |
| [`intake`](skills/intake.md) | Zero-friction context capture — turns a 2-minute context dump into a structured intake note. |
| [`execute-verdict`](skills/execute-verdict.md) | Turn a confirmed brief into a reviewed diff via isolated workspace + implementation agents + verification. Never auto-promotes. |
| [`publish`](skills/publish.md) | Generate a self-contained static HTML results page from benchmark data. |

## Design philosophy

These skills share a few load-bearing ideas worth lifting:

- **Drift vs decay.** `/health` watches file mtime decay; `/eye` watches cross-file *content* coherence. Two different failure modes, two different tools.
- **Blind verification.** Improvement arcs spawn stateless verifier subagents that never see the orchestrator's conclusions — blindness is the feature.
- **Hypotheses before testing.** Commit root-cause hypotheses *before* verifying, so a wrong prior teaches you something instead of being retrofitted.
- **Never auto-promote.** Every production-touching change is drafted as "hold" and surfaced for human approval.
- **Tone discipline.** Claims use an observation template (`Observed at N=5`, not `confirmed`) to keep unreplicated results from misleading future readers.

## Usage

Copy any skill into your project's `.claude/commands/` directory, adjust the internal paths to match your repo, and invoke it as `/skill-name` in Claude Code.

## License

MIT — see [LICENSE](LICENSE).
