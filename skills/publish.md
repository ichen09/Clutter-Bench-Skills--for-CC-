# /publish — Static results page

Turn raw benchmark results into a self-contained HTML page. No dependencies, no JavaScript frameworks, opens in any browser.

## Steps

1. **Gather.** Read raw outputs from `emotions/bench/results/v2.2-calibration-primary/raw/`. Extract: run_id, question_id, confidence, correctness (checked against expected answers in question JSONL files).

2. **Build tables.** Group by question x technique. Per cell: N, correct rate, mean confidence, mean Brier.

3. **Generate HTML** at `emotions/bench/publish/index.html`:
   - Title: "Clutter-Bench v2.2 — Prompting Technique Effects on Authority Judgment"
   - Methodology section (substrate, question design, technique descriptions)
   - Main results table (3 questions x 2 conditions)
   - Per-question detail (what it asks, correct answer, baseline vs T06)
   - Limitations (synthetic substrate, N=5, authorial ground truth)
   - Raw data link to git branch

4. **Tone.** L-026 template for all claims. "Observed at N=5" not "confirmed."

## When

When Isaac says "publish" or "website." After a replication batch lands and numbers are stable. Not a paper, not a dashboard, not a hosting service.
