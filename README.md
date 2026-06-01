# Clutter-Bench Skills

A set of [Claude Code](https://claude.com/claude-code) slash-command skills extracted from the Clutter-Bench research project — a benchmark studying how prompting techniques affect a model's authority/calibration judgment over cluttered substrates.

These skills encode a self-improving, drift-resistant agent workflow: orient at session start, detect cross-file drift, run autonomous improvement arcs, stress-test a system from many angles, capture context, and publish results. They're shared here as reference patterns for anyone building agentic harnesses on top of Claude Code.

## Live showcase

A single self-contained page on the process & craft behind this framework — written from the project's own brainstorms and operating docs:

**→ https://ichen09.github.io/Clutter-Bench-Skills--for-CC-/**

(Source: [`docs/index.html`](docs/index.html) — no build step, no dependencies; open it directly in any browser.)

> **Note:** Each skill is a `.md` command file dropped into `.claude/commands/`. They reference project-internal scripts and paths (e.g. `emotions/bench/scripts/doc_health.py`, `me.md`, `STATUS.md`), so they are published as *patterns to study and adapt*, not as a drop-in package. Adapt the paths to your own repo.

## The skills

| Skill | What it does |
|-------|--------------|
| [`boot`](skills/boot.md) | Session start — one command combining catch-up + saturate + health + tone-audit. Orients a fresh agent in seconds. |
| [`think`](skills/think.md) | Load ideation context — reads the stuck stack, routing rubric, and collaboration theory so the agent is aligned before a brain-dump. |
| [`eye`](skills/eye.md) | Ranked cross-file drift report — contradictions, philosophy-version lag, dependency cycles, hardcoded paths. Where `/health` watches mtime decay, `/eye` watches content coherence. |
| [`improve`](skills/improve.md) | Run one self-improvement arc on the top drift finding — hypothesize, spawn blind verifiers, build the fix, cold-reader test, encode the lesson, open a PR. |
| [`session-arc`](skills/session-arc.md) | Autonomous improvement cycle chaining 5 skills: detect drift → fix → review → verify → commit. |
| [`stress-test`](skills/stress-test.md) | Cold-read a system from multiple angles to find what's drifted, fix it, verify, and build a linter so you don't have to do it again. Refined through 22 hypothesis tests; works on docs and code. |
| [`trace`](skills/trace.md) | Implication tracer — scans intake notes and traces their implications through the system via parallel analysis agents. |
| [`intake`](skills/intake.md) | Zero-friction context capture — turns a 2-minute context dump into a structured intake note. |
| [`execute-verdict`](skills/execute-verdict.md) | Turn a confirmed brief into a reviewed diff via isolated workspace + implementation agents + verification. Never auto-promotes. |
| [`publish`](skills/publish.md) | Generate a self-contained static HTML results page from benchmark data. |

## Design philosophy

These skills share a few load-bearing ideas worth lifting:

- **Drift vs decay.** `/health` watches file mtime decay; `/eye` and `/stress-test` watch cross-file *content* coherence. Different failure modes, different tools.
- **Sharp angles beat vague ones.** `/stress-test`'s biggest lesson from 22 hypothesis tests: every audit angle must be a falsifiable yes/no check against disk. "Is this good?" finds nothing; "Does each Next Action appear in git log?" finds real bugs.
- **Blind verification.** Improvement arcs spawn stateless verifier subagents that never see the orchestrator's conclusions — blindness is the feature.
- **Hypotheses before testing.** Commit root-cause hypotheses *before* verifying, so a wrong prior teaches you something instead of being retrofitted.
- **Never auto-promote.** Every production-touching change is drafted as "hold" and surfaced for human approval.
- **Build the linter.** Once an audit finds ≥3 recurring issue types, encode them in an automated checker. The army discovers what to check; the linter remembers.

## Usage

Copy any skill into your project's `.claude/commands/` directory, adjust the internal paths to match your repo, and invoke it as `/skill-name` in Claude Code.

## License

MIT — see [LICENSE](LICENSE).
