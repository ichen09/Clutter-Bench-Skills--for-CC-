---
fresh_as_of: '2026-04-17'
invalidated_if_modified:
- memory/intake/INDEX.md
- CLAUDE.md
trust_level: operational
exempt_reason: null
---

# Capture context into intake

When Isaac shares context from a meeting, call, realization, or any external information during conversation, capture it as an intake note.

## Steps

1. Create a markdown file at `memory/intake/YYYY-MM-DD_HHMM_short-description.md` using the current date and time
2. Write Isaac's context as-is — preserve his words, don't sanitize or restructure. Include:
   - Who was involved (professor, lab partner, etc.)
   - What was said or decided
   - Any deadlines, constraints, or priority shifts
   - Any new data, sites, or requirements mentioned
3. Do NOT add YAML frontmatter — the implication tracer adds `processed_at` when it runs
4. Update `memory/intake/INDEX.md` with the new note entry (status: pending)
5. Tell Isaac the note was captured and suggest running `/trace` to analyze implications

## Example

If Isaac says "Jaffe wants results by end of May and the CSV has 8 sites":
```markdown
# Jaffe Call — April 14

- Real data deadline: end of May
- CSV will have 8 sites (3 new ones we haven't seen)
```

The note should be short, honest, and unstructured. Isaac's 2-minute context dump is the input. Zero friction.
