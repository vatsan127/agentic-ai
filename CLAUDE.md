# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this repo is

A personal, hands-on **learning project** for agentic AI. The learner is a Java
developer, new to AI, picking up Python along the way. This is not production
code — clarity and explanation beat cleverness or optimization.

- Stack (planned): **Python + LangChain** (Phase 2 onward). No Python code yet.
- Docs: notes written as Markdown under `docs/`, rendered by **MkDocs Material**.
- Approach: concepts first → raw provider SDK → LangChain abstractions.
- Pace: ~1 hour/day, phase by phase.

Authoritative plan: [`ROADMAP.md`](./ROADMAP.md). Overview: [`README.md`](./README.md).

## Project structure

```
agentic-ai/
├── README.md                 ← repo intro (structure + setup + run)
├── ROADMAP.md                ← the learning plan & checklist (authoritative)
├── CLAUDE.md                 ← this file
├── mkdocs.yml                ← site config: theme, nav, Markdown extensions
├── .gitignore                ← excludes .venv/, site/, .env
├── docs/                     ← single source of truth for all notes
│   ├── index.md              ← landing page
│   ├── 01-basics.md … 08-tools-and-mcp.md
│   └── stylesheets/extra.css ← full-width layout override
├── images/                   ← screenshots used while authoring notes
├── transcriptoin/            ← raw video transcript(s) behind the notes
├── .github/workflows/        ← deploy-docs.yml: auto-build + publish to GitHub Pages
├── .venv/                    ← gitignored; MkDocs Material lives here
└── site/                     ← gitignored; mkdocs build output
```

Notes so far: Phase 0 = notes 1–5 (AI/ML basics → customizing models → GenAI
pipeline), Phase 1 = notes 6–8 (autonomous-vs-agentic, anatomy of an agent,
tools & MCP).

Future folders — create only when actually used, never as empty placeholders:

- `code/phase-N-*/` — Python source per roadmap phase.

## Commands

Assume the venv at `.venv/`:

```bash
# Live dev server, hot reload → http://127.0.0.1:8000/agentic-ai/
.venv/Scripts/python.exe -m mkdocs serve

# Static build to site/
.venv/Scripts/python.exe -m mkdocs build

# Strict build (fail on warnings — run before publishing)
.venv/Scripts/python.exe -m mkdocs build --strict
```

Recreate the venv if `.venv/` is lost:

```bash
py -m venv .venv
.venv/Scripts/python.exe -m pip install --upgrade pip mkdocs-material
```

## Deployment

The site auto-deploys to GitHub Pages via `.github/workflows/deploy-docs.yml`.
Every push to `master` triggers a GitHub Actions run that installs
`mkdocs-material`, runs `mkdocs build --strict`, and publishes the result to
`https://vatsan127.github.io/agentic-ai/`. No manual step and no `gh-pages`
branch — the built site is uploaded straight to Pages as an artifact. Work on a
feature branch, then merge to `master` to publish. The workflow can also be run
by hand from the repo's Actions tab (`workflow_dispatch`).

One-time setup (done once in the GitHub repo UI): Settings → Pages → Build and
deployment → Source = "GitHub Actions". The `github-pages` environment allows
deploys from the default branch (`master`) out of the box.

One-time setup (done once in the GitHub repo UI): Settings → Pages → Build and
deployment → Source = "GitHub Actions".

## How to collaborate here

Per the roadmap's "How we'll work each session":

1. Explain the concept first (everyday analogies welcome).
2. Write a small, runnable snippet (from Phase 2 onward).
3. Run it, observe, discuss what happened.
4. Tick the box in `ROADMAP.md`. Move to the next item.

Be willing to **teach** as you go. The user is new to AI and to Python. Don't
assume familiarity with LangChain, agent loops, embeddings, or vector stores —
introduce them when they appear. Explain non-obvious Python idioms. Use plain,
everyday analogies, not analogies to other programming languages.

Default to small, working examples over large scaffolds.

## Notes file conventions

All notes live under `docs/` as `.md` files. See `docs/07-anatomy-of-an-agent.md`
as the reference for layout style.

- **Filename:** `NN-topic-name.md` (zero-padded, kebab-case), continuing the sequence.
- **Title:** `# Title Case Title` — plain, no "Topic N:" prefix and no number
  (the `NN-` lives in the filename only).
- **Sections:** `## Heading`, `### Sub-heading`. No ALL CAPS headings.
- **Closing:** end each note with a recap section (e.g. "Key Takeaways").
- **Lists:** `-` for bullets; 4-space indent for sub-bullets.
- **Diagrams:** Mermaid (` ```mermaid `) for flowcharts and relationships. **No images.**
  Inside Mermaid node labels use plain text + HTML (`<br/>`, `<i>`, `•`) — Markdown
  `**bold**`/`*italic*` does not render reliably there.
- **Admonitions:** `!!! tip "Think of it as"` for everyday analogies;
  `!!! note "For a Java dev"` for Java framing; `!!! example` / `!!! warning` /
  `!!! info` as appropriate.
- **Tables:** Markdown tables for side-by-side comparisons (no ASCII alignment).
- **Cross-links:** relative paths, e.g. `[Tokens & Language](03-tokens-and-language.md)`.

Tone is professional and explanatory — teaching voice is fine, slang is not.

## Things to avoid

- Don't introduce code, dependencies, or tooling ahead of the current phase
  (no LangChain code before Phase 5, no agent frameworks before Phase 6).
- Don't replace the raw-SDK exercises with framework shortcuts — feeling the
  raw API is the point of Phases 3–4.
- Don't commit secrets. API keys belong in `.env` (gitignored) once Phase 2 lands.
- Don't create new Markdown docs (design notes, summaries, plans) unless asked.
- Don't create empty placeholder folders before they are actually needed.
- Don't reformat or restructure `README.md` / `ROADMAP.md` without being asked —
  they reflect the learner's voice.

## Keep this file current

When you make a structural change to the repo, **register it in this file in the
same change** — treat `CLAUDE.md` as part of the deliverable. Structural changes
include: adding/removing/renaming/moving files or folders (update the structure
tree); changing the docs format or conventions; changing the MkDocs setup, venv
recipe, or commands. Don't reformat unrelated sections while doing so.
