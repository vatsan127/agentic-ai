# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this repo is

A personal, hands-on **learning project** for agentic AI. The learner is picking
up Python and LangChain along the way. This is not production code — clarity and
explanation beat cleverness or optimization.

- Stack (planned): **Python + LangChain** (Phase 2 onward)
- Docs platform: **MkDocs Material** + GitHub Pages
- Approach: concepts first → raw provider SDK → LangChain abstractions
- Pace: ~1 hour/day, phase by phase

Authoritative plan: [`docs/roadmap.md`](./docs/roadmap.md). Overview: [`README.md`](./README.md).

## Current state

- Notes converted from `.txt` (root) to `.md` (under `docs/`), rendered by MkDocs
  Material. Diagrams use Mermaid; "Think of it as…" uses admonitions.
- **Phase 0** — Topics 1–4 written in `docs/`.
- **Phase 1** — Topic 5 (Autonomous vs Agentic) and Topic 6 (Anatomy of an Agent) written.
- `.venv/` and `site/` are gitignored. No Python code yet — Phase 2 starts that.

## Project structure

Current layout:

```
agentic-ai/
├── README.md                 ← repo intro (needs slim-down — see "Pending cleanup")
├── ROADMAP.md                ← legacy duplicate of docs/roadmap.md (delete)
├── CLAUDE.md                 ← this file
├── mkdocs.yml                ← site config
├── .gitignore                ← excludes .venv/, site/, .env
├── 01-basics.txt             ← legacy, duplicated in docs/ (delete)
├── ... through ...
├── 06-anatomy-of-an-agent.txt    ← legacy, duplicated in docs/ (delete)
├── docs/                     ← single source of truth for all notes
│   ├── index.md              ← landing page
│   ├── roadmap.md
│   └── 01-basics.md … 06-anatomy-of-an-agent.md
├── .venv/                    ← gitignored; MkDocs Material lives here
└── site/                     ← gitignored; mkdocs build output
```

Future additions (DO NOT create empty placeholders — only when actually used):

- `code/phase-2-setup/`, `code/phase-3-plain-llm/`, ... — Python source per roadmap phase.
- `.github/workflows/deploy-docs.yml` — auto-deploy to GitHub Pages on push.

## Docs site (MkDocs Material)

The notes are rendered as a static site using **MkDocs** (Python static site
generator) + the **Material for MkDocs** theme. Output is plain HTML in `site/`,
designed to be served by GitHub Pages.

**Why this stack:**

- Source stays Markdown — portable, git-friendly, readable raw.
- Native Mermaid diagrams (` ```mermaid ` fences) and admonitions.
- One Python dependency (`mkdocs-material`) — fits the Phase 2 Python stack.
- `mkdocs serve` gives a hot-reload local server; `mkdocs build` produces static
  HTML that any host (GitHub Pages, Netlify, etc.) can serve.
- The de facto docs stack for major Python projects (FastAPI, Pydantic, Typer).

**Where things live:**

- `mkdocs.yml` — site config: theme, nav, markdown extensions, palette.
- `docs/` — all Markdown source files. Nav is explicit in `mkdocs.yml`.
- `.venv/` — Python virtualenv with MkDocs Material 9.7.6 installed (gitignored).
- `site/` — `mkdocs build` output (gitignored).

**Key Markdown extensions enabled in `mkdocs.yml`** (so notes can use them):

- `admonition` + `pymdownx.details` — `!!! tip`, `!!! note`, etc.
- `pymdownx.superfences` with the Mermaid custom fence — ` ```mermaid ` blocks.
- `pymdownx.tabbed`, `pymdownx.tasklist`, `tables`, `toc` (with permalinks).

**Recreating the venv from scratch** (if `.venv/` is ever lost):

```bash
"C:/Users/vatsan/AppData/Local/Python/pythoncore-3.14-64/python.exe" -m venv .venv
.venv/Scripts/python.exe -m pip install --upgrade pip mkdocs-material
```

**GitHub Pages deployment** (not yet set up — deferred):

- Target URL: `https://vatsan127.github.io/agentic-ai/`
- Plan: a workflow at `.github/workflows/deploy-docs.yml` running `mkdocs gh-deploy`
  on every push to `master`. Two manual GitHub UI steps will be required:
  - Settings → Pages → Source: **GitHub Actions**.
  - Settings → Actions → General → Workflow permissions: **Read and write**.
- Alternative: run `mkdocs gh-deploy` manually from the repo each time.

## Pending cleanup (next session)

The user explicitly deferred these to the next session. When asked to proceed:

1. **Delete legacy duplicates at root:**
   - `01-basics.txt`
   - `02-models.txt`
   - `03-tokens-and-language.txt`
   - `04-customizing-models.txt`
   - `05-autonomous-vs-agentic.txt`
   - `06-anatomy-of-an-agent.txt`
   - `ROADMAP.md` (lives at `docs/roadmap.md` now)
2. **Rewrite `README.md`** to contain only:
   - Brief project description (1 short paragraph)
   - Project structure tree (current layout, post-cleanup)
   - Commands: `mkdocs serve` (dev server) and `mkdocs build` (static build)
   - A link to the live GitHub Pages URL once published
3. **Do NOT yet** create `code/`, `.github/`, or any other deferred folders.

## Common commands

All assume the venv at `.venv/` (created earlier):

```bash
# Live dev server with hot reload at http://127.0.0.1:8000/
.venv/Scripts/python.exe -m mkdocs serve

# Static build to site/ (also what GitHub Pages deploys)
.venv/Scripts/python.exe -m mkdocs build

# Strict build (fail on warnings — use before publishing)
.venv/Scripts/python.exe -m mkdocs build --strict
```

## How to collaborate here

Per the roadmap's "How we'll work each session":

1. Explain the concept first (everyday analogies welcome).
2. Write a small, runnable snippet (from Phase 2 onward).
3. Run it, observe, discuss what happened.
4. Tick the box in `docs/roadmap.md`. Move to the next item.

Be willing to **teach** as you go. The user is new to AI and to Python. Don't
assume familiarity with LangChain, agent loops, embeddings, or vector stores —
introduce them when they appear. Explain Python idioms and standard-library
pieces when they're not obvious. Use plain analogies (everyday objects, simple
mental models) rather than analogies to other programming languages.

Default to small, working examples over large scaffolds. Avoid pulling in extra
libraries, frameworks, or abstractions beyond what the current phase needs.

## Notes file conventions

All notes live under `docs/` as `.md` files, rendered by MkDocs Material:

- **Filename:** `NN-topic-name.md` (zero-padded, kebab-case). Numbering continues
  the existing sequence.
- **Title:** `# Topic N: Title Case Title` at the top.
- **Sections:** `## Heading`, `### Sub-heading`. No ALL CAPS headings.
- **Lists:** `-` for bullets; 4-space indent for sub-bullets.
- **Diagrams:** Mermaid (` ```mermaid `) for flowcharts and relationships. Plain
  fenced code blocks (` ```text `) for ASCII illustrations only when Mermaid
  doesn't fit. **No images.**
- **Analogies:** wrap "Think of it as…" in a `!!! tip "Think of it as"` admonition.
  Use plain, everyday analogies — not analogies to other programming languages.
- **Examples/warnings:** use `!!! example` / `!!! warning` / `!!! info` admonitions
  where appropriate.
- **Tables:** use Markdown tables for side-by-side comparisons (don't recreate
  ASCII alignment).
- **Cross-links:** link to other topics with relative paths
  (e.g. `[Topic 3](03-tokens-and-language.md)`).

See `docs/06-anatomy-of-an-agent.md` as the reference for current layout style.

## Roadmap and README

- Keep checklists in `docs/roadmap.md` as `- [ ]` / `- [x]`. Tick items only when
  the user confirms the deliverable is done.
- Don't reformat or restructure `README.md` / `docs/roadmap.md` without being
  asked — they reflect the learner's voice. The README rewrite is explicitly
  pre-authorized in "Pending cleanup" above.

## Things to avoid

- Don't introduce code, dependencies, or tooling ahead of the current phase
  (no LangChain code before Phase 5, no agent frameworks before Phase 6).
- Don't replace the raw-SDK exercises with framework shortcuts — feeling the
  raw API is the point of Phases 3–4.
- Don't commit secrets. API keys belong in `.env` (gitignored) once Phase 2 lands.
- Don't create new markdown docs (design notes, summaries, plans) unless asked.
- Don't create empty placeholder folders (e.g. `code/`, `.github/`) before they
  are actually needed.
