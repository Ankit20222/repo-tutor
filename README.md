# Repo Tutor

A [Claude](https://claude.com/claude-code) skill that turns any git repository into a guided, interactive course.

Point it at a codebase and it builds a structured learning plan, teaches each section grounded in the *real* code (with clickable `file:line` references), quizzes you after every section, and helps you build a spaced-repetition flashcard deck — all of which is saved to disk so you can stop and resume across sessions.

## What it does

The tutor runs a deliberate loop instead of dumping explanations:

**survey → assess → plan → teach a section → quiz → flashcards → record progress → next section**

- **Surveys the repo first** — reads the README, manifests, entry points, tests, and git history to build an accurate map before teaching anything.
- **Assesses you** — asks about your goal (contribute? understand architecture? interview prep?), experience, and pace, then tailors the course.
- **Builds a saved learning plan** — divides the material into dependency-ordered sections (foundations first, leaf features last) and lets you reorder or trim before starting.
- **Teaches the underlying concepts, not just the code** — names the patterns, algorithms, and protocols the code rests on, and when it hits a deep topic in its own right (e.g. RAG, Raft, OAuth2 PKCE, CRDTs) it *asks* whether you want a proper detour to learn that concept or just the gist.
- **Shows the alternatives** — for each significant design decision, explains what else the authors could have used and the tradeoff, so you learn to *evaluate* the code, not just read it.
- **Quizzes after each section** — 3–5 retrieval questions (conceptual, code-location, trace/predict, tradeoff) with supportive grading.
- **Saves flashcards** — proposes atomic Q→A cards (prioritizing whatever you stumbled on), lets you edit them, and saves to both human-readable markdown and Anki-importable CSV.
- **Persists progress** — resumes exactly where you left off in a later session.

## Installation

Copy the skill into your Claude skills directory:

```bash
git clone https://github.com/<your-username>/repo-tutor.git
mkdir -p ~/.claude/skills
cp -R repo-tutor ~/.claude/skills/repo-tutor
```

(The folder must contain `SKILL.md` at its root.)

## Usage

In Claude Code, from inside or alongside a repo you want to learn:

> "Teach me this repo."
> "Help me understand this codebase — I want to be able to contribute."
> "I just cloned this project, where do I start?"

The skill triggers automatically when your goal is understanding a codebase deeply rather than making one quick change.

## Files it creates

Everything the tutor saves lives in a `.repo-tutor/` folder inside the repo you're learning (add it to `.gitignore`):

| File | Purpose |
|------|---------|
| `plan.md` | The ordered learning plan |
| `progress.md` | Completed sections, quiz scores, and skipped concept detours |
| `flashcards.md` | Human-readable deck, grouped by section |
| `flashcards.csv` | `Question,Answer,Section` — importable into Anki |

See [`references/templates.md`](references/templates.md) for the exact formats.

## Repository layout

```
repo-tutor/
├── SKILL.md                  # the skill definition (workflow + triggering)
├── references/
│   └── templates.md          # on-disk formats for plan, progress, flashcards
└── README.md
```
