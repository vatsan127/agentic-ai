# Agentic AI — Learning Project

A personal, hands-on project to learn **agentic AI** from the ground up — concepts first,
then Python + LangChain. Built by a Java developer new to AI. The notes are written as a
documentation site (MkDocs Material) with Mermaid diagrams.

📖 **Read the notes:** live at **<https://vatsan127.github.io/agentic-ai/>** — topics
1–8, with source under [`docs/`](docs/).

## Project structure

```
agentic-ai/
├── README.md          # This file
├── CLAUDE.md          # Guidance for Claude Code
├── mkdocs.yml         # Site config (theme, nav, Mermaid)
├── docs/              # All notes (single source of truth)
│   ├── index.md           # Landing page
│   ├── 01-basics.md       # AI / ML / DL / GenAI taxonomy
│   ├── 02-models.md       # Foundation Models, LLMs, how they generate
│   ├── 03-tokens-and-language.md   # Tokens, context windows, NLP terms
│   ├── 04-customizing-models.md    # Fine-tuning, distillation
│   ├── 05-genai-pipeline.md        # The 7 stages: raw data → deployed product
│   ├── 06-autonomous-vs-agentic.md # Autonomy spectrum
│   ├── 07-anatomy-of-an-agent.md   # The agent loop, four components, harness, context
│   ├── 08-tools-and-mcp.md         # Tool anatomy and the MCP standard
│   └── stylesheets/extra.css       # Full-width layout override
├── .venv/             # Python virtualenv (gitignored)
└── site/              # mkdocs build output (gitignored)
```

## Setup

Create the virtualenv and install MkDocs Material (one-time):

```bash
py -m venv .venv
.venv/Scripts/python.exe -m pip install --upgrade pip mkdocs-material
```

## Run

```bash
# Live dev server with hot reload → http://127.0.0.1:8000/agentic-ai/
.venv/Scripts/python.exe -m mkdocs serve

# Static build to site/
.venv/Scripts/python.exe -m mkdocs build
```
