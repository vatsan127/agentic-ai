# Agentic AI — Learning Project

A personal, hands-on journey to learn **agentic AI**, built for a Java developer who is
new to AI and picking up Python along the way.

- **Language/stack:** Python + **LangChain**
- **Approach:** concepts first → raw API understanding → LangChain abstractions
- **Pace:** ~1 hour/day
- **Goal:** deeply understand how agents work *and* become fluent in Python + LangChain

See [`ROADMAP.md`](./ROADMAP.md) for the full phase-by-phase plan and checklist.

---

## Project structure

```
agentic-ai/
├── README.md                     # This file — overview & structure
├── ROADMAP.md                    # The full learning plan & checklist (start here)
│
├── 01-basics.txt                 # Phase 0 — AI / ML / DL / GenAI taxonomy,
│                                 #   supervised vs unsupervised, discriminative vs generative
├── 02-models.txt                 # Phase 0 — Foundation Models, LLMs, and how they generate
│                                 #   (text token-by-token, images via diffusion)
├── 03-tokens-and-language.txt    # Phase 0 — tokens, context windows, and core NLP terms
│                                 #   (corpus, document, vocabulary, token, embedding, vector)
├── 04-customizing-models.txt     # Phase 0 — fine-tuning & distillation, and the
│                                 #   end-to-end GenAI pipeline (7 stages)
│
└── 05-autonomous-vs-agentic.txt  # Phase 1 — autonomous AI vs agentic AI: the
                                  #   spectrum of autonomy, comparison, and guardrails
```

> Code and further notes are added per phase as the roadmap progresses (Phase 2 onward).

---

## Phase 0 — Fundamentals (notes above)

The `0X-*.txt` files are the GenAI fundamentals, written up by the learner (revised from
an earlier `generative-ai-fundamentals` study repo). They are prerequisites for the
agentic material and make this repo self-contained.

Notes conventions:
- Title at top in ALL CAPS with an `===` underline
- Section headers in ALL CAPS
- `-` for bullets; two-space indent for sub-bullets
- ASCII/indented diagrams, no images
- "Think of it as…" analogies; Java-dev framing where useful

## Phases 1+ — Agentic AI (guided)

The guided track starts at **Phase 1** (the LLM → Agent conceptual leap) and runs through
setup, plain LLM calls, conversation/memory, tools, the agent loop, and beyond. Details
and the checklist live in [`ROADMAP.md`](./ROADMAP.md).
