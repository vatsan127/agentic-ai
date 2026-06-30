# Topic 6: Anatomy of an Agent

What is actually inside an "AI agent"? This note covers the foundational mental model:
the chat-to-agent leap, the loop that drives an agent, the four components every agent
is built from, the platform it runs on (the "harness"), how to feed it the right
information (prompt vs. context engineering), and how it reaches out to the world
(tools and MCP).

These are Phase 1 building blocks. They are independent of any specific framework — the
same patterns show up in Claude Code, LangChain, Manus, plain SDK code, anything.

## Chat vs Agent — the Conceptual Leap

The cleanest one-line definition:

| Chat Model | Agent |
|---|---|
| question → answer | goal → result |

Side by side, the difference is in **who does the work**:

```mermaid
flowchart TB
    subgraph CHAT["💬 CHAT — question → answer"]
        direction TB
        C1["You ask"] --> C2["AI replies"] --> C3["YOU do the work"]
    end
    subgraph AGENT["🤖 AGENT — goal → result"]
        direction TB
        A1["You"] --> A2["Agent<br/><i>plans & executes</i>"] --> A3["Done"]
    end
```

In chat, the AI hands the work back to you. In an agent, the work comes back done.

- A **chat model** is ping-pong. You ask, it answers, you do the work, you ask again.
- An **agent** is delegation. You hand over a goal; it plans, picks tools, takes steps,
  and only comes back when the work is done.

!!! tip "Think of it as"
    The difference between *advice* and *delegation*. A chat model gives you advice. An
    agent does the job.

!!! note "For a Java dev"
    - A chat model is a pure function. `String -> String`. Stateless. Idempotent.
    - An agent is closer to a Spring `@Service` you hand a task to. It calls other beans
      (tools), does I/O, keeps working until the task is done, then returns a result.

This is the leap from "AI you talk to" to "AI that works for you."

## The Agent Loop

An agent is not a single LLM call. Internally it is a **loop** that runs until the work
is judged complete:

```mermaid
flowchart LR
    YOU["You<br/><i>hand over a goal</i>"] --> OBS
    OBS["1. Observe<br/><i>read context, files,<br/>previous tool results</i>"] -->|next step| THINK
    THINK["2. Think<br/><i>reason about what to<br/>do next, plan approach</i>"] -->|decide action| ACT
    ACT["3. Act<br/><i>call a tool, edit a<br/>file, run a command</i>"] -->|get result| OBS
    THINK -->|task complete| FINAL["Generate Final Response<br/><i>synthesize + format answer</i>"]
    FINAL --> OUT["Output to User"]
```

- **Observe** — gather context: the prompt, files in the workspace, results from
  earlier tool calls. Whatever is relevant to the current step.
- **Think** — decide what to do next. "I don't know X yet, so I need to call tool Y."
- **Act** — actually do something: call a tool, write a file, run code, send an email.
- **Repeat** — feed the action's result back into Observe. Keep looping.
- **Stop** — when the model judges that the task's exit criteria are met.

The exit criteria come from the prompt. *"Compile 10 sources and produce a PowerPoint"*
gives the agent a concrete finish line: 10 sources gathered **and** PowerPoint exists.

!!! example "Worked example"
    Prompt: *"build a minimalist portfolio site for Greg Isenberg, then host it locally
    so I can see it."*

    1. **Observe** — prompt loaded; workspace empty; no info on Greg.
    2. **Think** — "I don't know who Greg is; need to research."
    3. **Act** — call a web-search tool.
    4. **Observe** — research returned; now I know who Greg is.
    5. **Think** — "next I should write a plan."
    6. **Act** — create `plan.md`.
    7. **Think** — "now I should write the HTML."
    8. **Act** — create `index.html`.
    9. **Think** — "the task says host it locally and verify."
    10. **Act** — start a local server, screenshot the page.
    11. **Observe** — screenshot looks right; task complete.
    12. **Stop** — return summary to the user.

!!! note "For a Java dev"
    This is a `while` loop with a strategy choosing the next action and a registry of
    callable services (tools). The exit condition is not a counter — it is a model
    judgment. Always set a hard `maxIterations` safety so it cannot spin forever on a
    confused goal.

## The Four Components of an Agent

Every agent is built from exactly four parts. They all wire into the agent:

```mermaid
flowchart LR
    LLM["The LLM<br/>• reasoning engine<br/>• understands language<br/>• makes decisions"]
    TOOLS["The Tools<br/>• read files<br/>• run code<br/>• search web<br/>• call APIs<br/>• edit files"]
    LOOP["The Loop<br/>• keeps going until task done<br/>• doesn't stop after one response"]
    CTX["The Context<br/>• CLAUDE.md<br/>• conversation history<br/>• memory files<br/>• skills"]
    AGENT(["AI Agent"])
    LLM --> AGENT
    TOOLS --> AGENT
    AGENT --> LOOP
    AGENT --> CTX
```

In words:

1. **LLM** — the brain. Picks the next step and writes the outputs.
   (Claude, GPT, Gemini, a local model via Ollama, …)
2. **Loop** — the act-observe-decide cycle. The thing that makes an agent an agent
   instead of a one-shot chat reply.
3. **Tools** — the hands. Functions the model can call: web search, file write, send
   email, run code, query a database.
4. **Context** — what the model knows about you and the situation: the system prompt,
   files in the workspace, memory of past sessions.

Take any one away and you don't have an agent:

| Remove… | …and you get |
|---|---|
| No LLM | It's just a script |
| No loop | It's a chat model — one reply and done |
| No tools | It can plan but cannot do anything in the world |
| No context | It gives generic answers that ignore who you are |

Keep this list in mind whenever an agent surprises you. The surprise usually traces to
one of these four being wrong — bad context, missing tool, loop ended too early, or the
model itself made the wrong call.

## The Agent Harness

The harness is the application that runs the loop and wires the four components
together. It is not the agent — it is the *environment* the agent runs in.

Examples (late 2025 / 2026):

- **Claude Code** (Anthropic; CLI + IDE integration)
- **Codex** (OpenAI)
- **Antigravity** (Google)
- **Co.work** (workspace-style)
- **Manus** (web-based)
- **Perplexity Computer** (web-based)
- Open-source CLI agents
- …and many more, with new ones appearing regularly

Different harnesses ship different features — some have built-in memory, some schedule
tasks, some have nicer UIs — but the engine underneath is the same loop calling the same
kinds of tools against the same kinds of context.

!!! tip "Think of it as"
    Harnesses are like different cars — Toyota, Range Rover, Tesla. Once you learn to
    drive (steer, brake, accelerate), you can drive any of them. Some have heated seats
    and cruise control. The fundamentals are identical.

!!! note "For a Java dev"
    The harness is the runtime, like the JVM plus your application server (Tomcat,
    Spring Boot, etc.). Your agent's configuration — its prompts, tools, context files —
    is the portable code that can run on any compatible runtime.

Practical consequence: don't over-commit to one harness. Learn the concepts first; the
harness becomes interchangeable.

## Prompt Engineering vs Context Engineering

A real shift has happened in how people get good results out of LLMs.

The old hotness — **prompt engineering**:

- Craft the perfect 17-paragraph prompt.
- "You are a world-class expert in X. Follow these 23 rules. Use this format…"
- The prompt does all the work. Without it, the model is generic.

The new hotness — **context engineering**:

- Load the agent with rich, accurate background up front.
- Now prompts can be stupidly simple. "Write a cold email."
- The model has everything it needs already; it just executes.

Same model, same task, very different result depending on what's in the room:

| Setup | Result |
|---|---|
| no context + "write a cold email" | generic placeholder text and a barrage of clarifying questions |
| full context + "write a cold email" | on-brand draft for your real ICP, ready to send |

### The mechanism

Every harness supports an auto-loaded markdown file at the project root that is read at
session start:

| Harness | File the harness auto-loads |
|---|---|
| Claude Code | `CLAUDE.md` |
| Gemini CLI | `GEMINI.md` |
| Codex / others | `AGENTS.md` (becoming the de facto standard) |

This file is the agent's "always-on" briefing. Typical contents:

- **The role:** "you are my executive assistant"
- **Who I am:** role, company, what we sell
- **Working preferences:** tone of voice, formats, what never to do
- **Tools available:** what's connected and what it's for

If the briefing gets large, split into a `context/` folder of markdown files
(`about-me.md`, `brand-voice.md`, `ideal-customer.md`, …) and add one line to the
auto-loaded file:

> "Before answering, read all files in `./context/` to learn who I am and how I work."

!!! note "For a Java dev"
    The auto-loaded file is `application.yml` for your agent — config read at startup so
    behaviour stays consistent without re-passing it each session. The `context/` folder
    is the rest of your config split across files. Same idea.

!!! tip "How to write a good context file"
    Open any chat model and say *"interview me to build my AGENTS.md file."* Let it ask
    the questions. Paste the result into the file.

!!! info "A side note that matters"
    Chat models like ChatGPT and Claude.ai have an *automatic* cloud-side memory.
    Helpful, but invisible and uncontrollable — and it bleeds context between unrelated
    projects. Your relationship advice can leak into your landing-page copy. Agents flip
    this around: memory is **explicit and file-based**. Less magic, but clean separation
    between projects. This is a feature, not a limitation.

## Tools and MCP

Tools are how an agent acts in the world. A tool is a callable function with:

- **a name** (e.g. `send_email`)
- **a description** — what it does (the model reads this to decide when to use it)
- **a parameter schema** — what arguments it takes
- **an implementation** — your code, or somebody else's, that actually runs

!!! note "For a Java dev"
    A tool is an injected service the model can call. The description and schema are the
    model's view of the interface; the implementation is your code behind it.

**Before MCP.** Every tool needed its own bespoke integration. The model had to "learn
the language" of each one. Custom adapter code per tool, per agent, per harness.

**After MCP.** Anthropic introduced the **Model Context Protocol (MCP)** as a standard
that sits between agent and tool. Tools expose themselves through MCP once; any
MCP-capable agent can call them without bespoke code.

Picture the language barrier. Claude speaks English; each tool "speaks" its own
language. Without a translator, the connections just don't work. With one MCP
translator in the middle, every tool is reachable:

```mermaid
flowchart LR
    subgraph BEFORE["❌ Before MCPs — every link is a broken language barrier"]
        direction LR
        CB["Claude<br/><i>(English)</i>"]
        CB -. "✗" .-> N1["Notion<br/><i>(Spanish)</i>"]
        CB -. "✗" .-> G1["Gmail<br/><i>(French)</i>"]
        CB -. "✗" .-> S1["Slack<br/><i>(Chinese)</i>"]
        CB -. "✗" .-> B1["Browser<br/><i>(Japanese)</i>"]
    end
    subgraph AFTER["✅ With MCPs — one translator, every tool reachable"]
        direction LR
        CA["Claude<br/><i>(English)</i>"] --> MCP["MCP Translator<br/><i>speaks every language</i>"]
        MCP --> N2["Notion"]
        MCP --> G2["Gmail"]
        MCP --> S2["Slack"]
        MCP --> B2["Browser"]
    end
```

Connect once, call anything.

!!! note "For a Java dev"
    MCP is JDBC. Before JDBC, every database had its own driver and API and you wrote
    bespoke client code per database. JDBC standardised the interface — new databases
    just ship a JDBC driver and existing code "just works." MCP is doing the same for
    tools meeting LLMs. Same shape of solution.

Most modern harnesses ship an MCP "marketplace" — one-click connectors for the major
tools (Gmail, Calendar, Notion, Stripe, GitHub, …). The end-state most people aim for:
you stop opening individual apps. Everything goes through the agent, which calls the apps
via MCP.

!!! info
    In LangChain — which the [Roadmap](roadmap.md) introduces in Phase 5 — the
    equivalent unit is the `@tool` decorator. Same conceptual building block; MCP is the
    more universal, language-agnostic version.

## What to Hold Onto

- **Agent = goal-to-result.** The leap from chat is the loop and the autonomy.
- The loop is **Observe → Think → Act → repeat → stop** on completion criteria.
- **Four components:** LLM, loop, tools, context. Diagnose problems against this list.
- The **harness** is the runtime. Learn the model, not the harness — they are
  interchangeable.
- **Context engineering > prompt engineering.** Load context once, keep prompts simple.
- Tools are how the agent does things. **MCP is the JDBC of tools** — one protocol, many
  connectors.
