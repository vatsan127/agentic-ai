# Tools & MCP

Tools are how an agent acts in the world — one of the [four components of an
agent](07-anatomy-of-an-agent.md). This note goes deeper: how tools were wired into
agents before, the problem that created, and the standard that solved it — the **Model
Context Protocol (MCP)**.

## What a tool is (recap)

A tool is a callable function with:

- **a name** (e.g. `send_email`)
- **a description** — what it does (the model reads this to decide when to use it)
- **a parameter schema** — what arguments it takes
- **an implementation** — your code, or somebody else's, that actually runs

!!! note "For a Java dev"
    A tool is an injected service the model can call. The description and schema are the
    model's view of the interface; the implementation is your code behind it.

## Before MCP

Every tool needed its own bespoke integration. The model had to "learn the language" of
each one. Custom adapter code per tool, per agent, per harness — so connecting the same
tool to a second agent meant writing the glue all over again.

## After MCP

Anthropic introduced the **Model Context Protocol (MCP)** as a standard that sits between
agent and tool. Tools expose themselves through MCP once; any MCP-capable agent can call
them without bespoke code.

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

## The marketplace, and where this is heading

Most modern harnesses ship an MCP "marketplace" — one-click connectors for the major
tools (Gmail, Calendar, Notion, Stripe, GitHub, …). The end-state most people aim for:
you stop opening individual apps. Everything goes through the agent, which calls the apps
via MCP.

!!! info
    In LangChain — introduced in Phase 5 — the equivalent unit is the `@tool`
    decorator. Same conceptual building block; MCP is the more universal,
    language-agnostic version.

## Key Takeaways

- A **tool** is a callable function the model can invoke: name, description, schema,
  implementation.
- **Before MCP**, every tool needed bespoke integration code per agent and per harness.
- **MCP** is a standard interface between agents and tools — expose a tool once, and any
  MCP-capable agent can call it. **MCP is the JDBC of tools.**
- Harnesses ship MCP marketplaces; the direction of travel is that the agent becomes the
  single front door to all your apps.
