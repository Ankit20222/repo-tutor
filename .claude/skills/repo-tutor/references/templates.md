# File templates

These are the on-disk formats for the files Repo Tutor saves under `.repo-tutor/`.
They're plain markdown/CSV so the learner can read and edit them by hand and import
the deck into spaced-repetition tools.

---

## `plan.md` — the learning plan

```markdown
# Learning Plan: <repo name>

**Goal:** <what the learner wants to get out of this>
**Level:** <beginner / intermediate / advanced>
**Created:** <YYYY-MM-DD>

## Overview
<2–4 sentences: what this project does and the shape of the course.>

## Sections

### Section 1 — <title>
- **Objectives:** after this you'll be able to <…>
- **Covers:** <files / dirs / concepts, e.g. `src/models/`, the User aggregate>
- **Checkpoint:** quiz + flashcards

### Section 2 — <title>
- **Objectives:** …
- **Covers:** …
- **Checkpoint:** quiz + flashcards

<…more sections…>
```

---

## `progress.md` — progress tracker

```markdown
# Progress: <repo name>

| Section | Status      | Quiz   | Cards | Date       | Notes                        |
|---------|-------------|--------|-------|------------|------------------------------|
| 1. <…>  | ✅ complete | 5/5    | 4     | 2026-05-30 | solid                        |
| 2. <…>  | ✅ complete | 3/5    | 6     | 2026-05-30 | shaky on event dispatch      |
| 3. <…>  | 🔄 current  | —      | —     | —          |                              |
| 4. <…>  | ⬜ upcoming | —      | —     | —          |                              |
```

Status legend: ⬜ upcoming · 🔄 current · ✅ complete.

### Concept detours offered

Track deep-dives the learner declined so they can revisit them later.

```markdown
## Underlying concepts
- [x] RAG — taught in Section 2
- [ ] Vector index internals (HNSW) — offered in Section 2, learner skipped
- [ ] Raft consensus — offered in Section 4, learner skipped
```

---

## `flashcards.md` — human-readable deck

```markdown
# Flashcards: <repo name>

## Section 1 — <title>

**Q:** Why does the repository pattern wrap the ORM here instead of calling it directly?
**A:** To keep persistence swappable and to centralize query logic — see `src/repo/base.py:18`.

**Q:** Where is the database connection pool configured?
**A:** `src/db/pool.py:12`, sized via the `DB_POOL_SIZE` env var.

## Section 2 — <title>

**Q:** …
**A:** …
```

---

## `flashcards.csv` — Anki-importable deck

First line is the header. Quote any field containing commas, quotes, or newlines
per normal CSV rules (double a literal `"` to `""`).

```csv
Question,Answer,Section
"Why does the repository pattern wrap the ORM here?","To keep persistence swappable and centralize query logic (src/repo/base.py:18)","Section 1 — Data layer"
"Where is the DB connection pool configured?","src/db/pool.py:12, sized via DB_POOL_SIZE","Section 1 — Data layer"
```

To import into Anki: File → Import → select `flashcards.csv` → map column 1 to Front,
column 2 to Back (column 3 can become a tag).
