# Agentic AI Learning Roadmap — Python + LangChain

**Learner:** Java developer, new to AI. (Python basics via own crash course.)
**Language:** Python + **LangChain** (the famous framework; Python-only).
**Approach:** Concepts -> raw API understanding -> LangChain abstractions.
**Pace:** ~1 hour/day.
**Goal:** Deeply understand how agents work AND become fluent in Python + LangChain.

> Why Python: LangChain (the framework you want) is Python-only. The hard part isn't
> Python — it's the agentic AI concepts, and those are the same in any language. The
> skill is the **agent loop**, not the framework.
>
> Java note (for work, later): LangChain4j and Spring AI are the Java equivalents —
> easy to pick up once these concepts click.

---

## Phase 0 — GenAI Fundamentals  ✅ (self-directed)

**Owned by the learner.** Revising old notes from the `generative-ai-fundamentals`
repo and writing them down here. Covers: AI/ML/DL/GenAI basics, foundation models &
LLMs, how GenAI generates, tokens & context windows, fine-tuning & distillation, the
GenAI pipeline, NLP terms.

> This roadmap's guided plan begins at Phase 1, assuming Phase 0 fundamentals are done.

---

## Phase 1 — From LLM to Agent (the conceptual leap)

**~2-3 sessions.** Goal: the agent-specific mental model. NO code yet.

- [ ] Statelessness: why an LLM has **no memory** — history is replayed every call
- [ ] Message roles: system / user / assistant
- [ ] Temperature in the agent context (focused vs creative)
- [ ] The leap LLM -> Agent: **tools + loop + autonomy**
- [ ] The agent loop diagram (memorize it): act -> observe -> decide -> repeat -> finish
- [ ] **Autonomous AI vs Agentic AI** — the spectrum of how much the system decides
      vs. how much a human/script decides; where "agentic" sits and what "fully
      autonomous" adds (and the risks it brings)
- [ ] Glossary: RAG, multi-agent, MCP, inference, hallucination

-> Deliverable: you can explain "what is an agent" to a colleague in 2 minutes.

---

## Phase 2 — Setup & API access

**~2 sessions.** Goal: a Python project that can call an LLM.

- [ ] Decide API access:
    - Option A: Anthropic API key (cleanest; small cost, has free credit)
    - Option B: OpenAI key (if you have one)
    - Option C: Local model via **Ollama** (free, runs locally, great for practice)
- [ ] Create a project folder + virtual environment (`venv`)
- [ ] `pip install` the provider SDK (and later, `langchain`)
- [ ] Store the API key safely (env var / `.env`, never in git)
- [ ] Verify with a "hello model" script

-> Deliverable: `python hello.py` and the model replies.

---

## Phase 3 — Plain LLM call (raw SDK, no LangChain yet)

**~3 sessions.** Goal: feel the raw request/response and statelessness BEFORE the framework.

- [ ] Single prompt -> single response via the raw provider SDK
- [ ] Change the system prompt, see behavior change
- [ ] Play with temperature (0 vs 1)
- [ ] Observe: a second call knows NOTHING about the first

-> Deliverable: a script that takes a question and prints the answer.

---

## Phase 4 — Conversation & memory

**~3 sessions.** Goal: understand that YOU manage memory.

- [ ] Build a multi-turn chat by resending history each time (raw)
- [ ] See the history/token cost grow
- [ ] Then: do the same with LangChain's memory — see what it automates
- [ ] Experiment: drop old messages, see the model "forget"

-> Deliverable: a chatbot that remembers across turns.

---

## Phase 5 — Enter LangChain + Tools (function calling)

**~4 sessions.** Goal: LangChain basics AND the model calling YOUR code.

- [ ] LangChain core ideas: models, prompts, chains, output parsers
- [ ] Concept: tool = name + description + param schema + your implementation
- [ ] Define ONE tool in LangChain (`@tool`), bind it to the model
- [ ] Watch the model decide to call it and use the result
- [ ] Add a second tool; see the model choose between them

-> Deliverable: an LLM that answers "what's 17% of 940?" by calling your calc tool.

---

## Phase 6 — The Agent Loop (LangChain agents)

**~5 sessions.** Goal: a real autonomous, multi-step agent.

- [ ] The loop: act -> observe -> decide -> repeat -> finish
- [ ] Build a LangChain agent (e.g. with LangGraph / AgentExecutor)
- [ ] Chain multiple tool calls to solve one task
- [ ] Guardrails: max iterations, error handling on tool failures
- [ ] Reflect: where LangChain runs the loop for you vs. where you control it

-> Deliverable: an agent that looks something up, then does math on it, in steps.

---

## Phase 7 — Going further (choose your adventure)

**Ongoing.** Pick based on interest/job needs.

- [ ] **RAG** — answer from YOUR documents (LangChain + a vector store)
- [ ] **Multi-agent** — CrewAI / LangGraph: specialized agents collaborating
- [ ] **MCP** — standardized tool/data integration (Model Context Protocol)
- [ ] **A real project** — your own idea, end to end
- [ ] (For work) map it back to **LangChain4j / Spring AI** in Java

---

## How we'll work each session

1. I explain the concept (with a Java/Python analogy where helpful).
2. We write a small, runnable piece of code (from Phase 2 on).
3. You run it, we observe, we discuss what happened.
4. Tick the box. Next day, next box.

## Notes conventions (matching the author's style)

- Title at top: `# Topic N: Title`.
- Section headers as `## Heading` / `### Sub-heading`.
- `-` for bullets; 4-space indent for sub-bullets.
- Mermaid diagrams for flows and relationships; no images.
- "Think of it as…" analogies in `!!! tip` admonitions; Java-dev framing where useful.
