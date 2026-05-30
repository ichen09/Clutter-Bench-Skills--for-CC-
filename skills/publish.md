# /publish — Generate a static results page from benchmark data

Turn raw benchmark results into a publishable HTML page with comparison tables, methodology summary, and substrate description.

## Steps

1. **Gather data.** Read all raw outputs from `emotions/bench/results/v2.2-calibration-primary/raw/`. For each, extract: run_id, question_id, confidence, answer correctness (by checking against expected answers in the question JSONL files).

2. **Build comparison tables.** Group by question × technique. For each cell, compute:
   - N (number of runs)
   - Correct count and rate
   - Mean confidence
   - Mean Brier score (confidence vs correctness)

3. **Generate HTML.** Write a single self-contained HTML file to `emotions/bench/publish/index.html` with:
   - Title: "Clutter-Bench v2.2 — Prompting Technique Effects on Authority Judgment"
   - Methodology section: substrate description (501+ files, 10 ambiguity zones), question design (d7-d9), technique descriptions (baseline, T04, T06)
   - Main results table: the 3-question × 2-condition comparison
   - Per-question detail: what each question asks, what the correct answer is, what baseline vs T06 answers
   - Limitations: synthetic substrate, N=5, ground truth is authorial
   - Raw data link: point to the git branch

4. **Use no dependencies.** Pure HTML + inline CSS. No JavaScript frameworks, no build tools. The file should open in any browser.

5. **Tone.** Use the L-026 tracking-doc template for all claims. "Observed at N=5" not "confirmed." Include the uncertainty caveat about synthetic substrate ground truth.

## When to use

- When Isaac says "publish" or "website" or "make it public"
- After a replication batch completes and the numbers are stable

## What this is NOT

- Not a paper. No abstract, no related work, no references.
- Not a dashboard. Static HTML, not a live-updating page.
- Not a hosting service. Generates the file; Isaac decides where to host it.
