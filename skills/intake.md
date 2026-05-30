---
fresh_as_of: 2026-05-27
trust_level: operational
invalidated_if_modified:
  - memory/intake/INDEX.md
---

# /intake — Capture context

When Isaac shares info mid-session (meeting notes, realizations, corrections), capture it as an intake note. Zero friction.

## Steps

1. Create `memory/intake/YYYY-MM-DD_HHMM_short-description.md`
2. Write Isaac's words as-is. Include: who, what was said/decided, deadlines, new data. Don't restructure.
3. No YAML frontmatter — `/trace` adds `processed_at` when it runs.
4. Update `memory/intake/INDEX.md` (status: pending).
5. Suggest running `/trace` to analyze implications.

## Example

Isaac says "Jaffe wants results by end of May and the CSV has 8 sites":

```markdown
# Jaffe Call — April 14

- Real data deadline: end of May
- CSV will have 8 sites (3 new ones we haven't seen)
```

Short, honest, unstructured. Isaac's 2-minute context dump is the input.
