# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this repo is

A personal, hands-on **learning project** for agentic AI. The learner is a Java
developer picking up Python and LangChain along the way. This is not production
code — clarity and explanation beat cleverness or optimization.

Authoritative plan: [`ROADMAP.md`](./ROADMAP.md). Overview: [`README.md`](./README.md).

- Stack (planned): Python + LangChain
- Approach: concepts first → raw provider SDK → LangChain abstractions
- Pace: ~1 hour/day, phase by phase

## Current state

- Phase 0 (GenAI fundamentals) is captured in `0X-*.txt` notes.
- Phase 1 has begun with `05-autonomous-vs-agentic.txt`.
- No Python code, no `requirements.txt`, no virtual environment yet — those
  arrive in Phase 2.

## How to collaborate here

Per the README's "How we'll work each session":
1. Explain the concept first (Java/Python analogies welcome).
2. Write a small, runnable snippet (from Phase 2 onward).
3. Run it, observe, discuss what happened.
4. Tick the box in `ROADMAP.md`. Move to the next item.

Be willing to **teach** as you go. The user is new to AI and to Python; framing
new ideas against Java equivalents (e.g., interfaces, dependency injection,
Spring) is often valuable. Don't assume familiarity with LangChain, agent
loops, embeddings, or vector stores — introduce them when they appear.

Default to small, working examples over large scaffolds. Avoid pulling in extra
libraries, frameworks, or abstractions beyond what the current phase needs.

## Notes file conventions (must match)

When adding or editing `.txt` notes, mirror the existing style:

- Title at top in ALL CAPS with an `======` underline.
- Section headers in ALL CAPS, framed by `==========` lines above and below.
- `-` for bullets; two-space indent for sub-bullets.
- ASCII/indented diagrams — no images, no Mermaid.
- "Think of it as…" analogies; Java-dev framing where it clarifies.
- File naming: `NN-topic-name.txt` (zero-padded, kebab-case). Numbering
  continues the existing sequence.

See `05-autonomous-vs-agentic.txt` as the reference for layout.

## Markdown files (README, ROADMAP)

- Keep checklists in `ROADMAP.md` as `- [ ]` / `- [x]`. Tick items only when
  the user confirms the deliverable is done.
- Don't reformat or restructure `README.md` / `ROADMAP.md` without being
  asked — they reflect the learner's voice.

## Things to avoid

- Don't introduce code, dependencies, or tooling ahead of the current phase
  (e.g., no LangChain code before Phase 5, no agent frameworks before Phase 6).
- Don't replace the raw-SDK exercises with framework shortcuts — feeling the
  raw API is the point of Phases 3–4.
- Don't commit secrets. API keys belong in `.env` (gitignored) once Phase 2 lands.
- Don't create new markdown docs (design notes, summaries, plans) unless asked.
