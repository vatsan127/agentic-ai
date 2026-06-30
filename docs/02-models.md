# Foundation Models, LLMs & How They Generate

This topic covers the "what" and the "how":

- **Part A** — the conceptual building blocks: Foundation Models and LLMs.
- **Part B** — the mechanics of how they actually produce output (text & images).

For practical API concerns (tokens, context windows, NLP terms), see
[Topic 3](03-tokens-and-language.md). For customizing these models, see
[Topic 4](04-customizing-models.md).

## Part A — Foundation Models & LLMs (the "what")

### What is Generative AI?

- Generative AI (GenAI) is a subset of Deep Learning.
- Used to generate new data that is similar to the data it was trained on.
- Can generate: text, image, audio, code, video.

### Foundation Model (FM)

- A large AI model that is **pre-trained on a massive amount of data** (text, images,
  etc.) so it can perform a broad range of general tasks — text generation,
  summarization, Q&A, image generation, chatbot, and more.
- Trained on a wide variety of input data; may cost **tens of millions of dollars** to
  train.
- Example: GPT-4o is one of the foundation models behind ChatGPT. (ChatGPT has used
  several OpenAI models over time: GPT-3.5, GPT-4, GPT-4o, and newer.)
- Wide selection from companies: OpenAI, Meta, Amazon, Google, Anthropic.

!!! tip "Think of it as"
    A base / starting point that you can then customize for your specific needs.

Licensing varies:

- **Open-source (Apache 2.0):** Google BERT
- **Source-available / free under Meta's own license** (not strictly OSI open-source):
  Meta Llama
- **Commercial license only:** OpenAI (GPT), Anthropic (Claude), etc.

Not all Foundation Models are generative:

- **Non-Generative FM** — only analyzes/understands data (e.g. BERT — classifies,
  extracts info).
- **Generative FM** — analyzes **and** creates new content (e.g. GPT-4, Claude,
  Stable Diffusion).

### Large Language Models (LLM)

- A type of Foundation Model specifically designed to understand and generate
  human-like text.
- Called "Large" because it has **billions of parameters** and is trained on massive
  text data (books, articles, websites).
- Can perform: translation, summarization, question answering, content creation.
- Notable example: GPT-4 (ChatGPT / OpenAI).

### FM vs GenAI vs LLM — how they relate

```mermaid
flowchart TB
    FM["Foundation Model (FM)<br/><i>all large pre-trained models</i>"]
    GENFM["Generative FMs<br/><i>can CREATE new content (GenAI)</i>"]
    NONGEN["Non-Generative FMs<br/><i>only ANALYZE / understand</i>"]
    LLM["LLM — generates text<br/><i>(GPT-4, Claude, Llama)</i>"]
    IMG["Image Models — generates images<br/><i>(Stable Diffusion)</i>"]
    MULTI["Multimodal Models — text + images + audio<br/><i>(GPT-4o, Amazon Titan)</i>"]
    BERT["BERT — understands & classifies text<br/><i>(but can't create)</i>"]
    FM --> GENFM
    FM --> NONGEN
    GENFM --> LLM
    GENFM --> IMG
    GENFM --> MULTI
    NONGEN --> BERT
```

- **FM** = the broad category of all large pre-trained models.
- **GenAI** = a capability — FMs that can generate new stuff.
- **LLM** = a specific type of FM focused on text.

!!! tip "Think of it as"
    - **FM** = athletes (all types)
    - **Generative FM** = athletes who can create new plays/moves (creative players)
    - **LLM** = athletes who specialize in cricket (text as the primary data type)

    Modern LLMs increasingly handle images/audio too (**multimodal** — like a cricketer
    who can also bowl and field well). Pure "text-only" LLMs are becoming rarer.

## Part B — How GenAI Generates Output (the "how")

This zooms into the mechanics of **how** models produce output — useful for
understanding temperature, determinism, and why the same prompt can give different
answers.

### How LLMs generate text

- You give the model a prompt (e.g. *"After the rain, the streets were…"*).
- The model generates a list of possible next words with probabilities:
  `"wet" → 40%`, `"flooded" → 25%`, `"slippery" → 15%`, `"empty" → 10%`, …
- An algorithm selects a word from that list based on probability (randomly).
- Then it repeats for the next word, and the next…
- This is **non-deterministic** — the same prompt can produce different text on each
  call (when temperature > 0).

!!! note "Why this matters when building with an LLM"
    - **Temperature** in API calls controls how random this selection is
      (0 = always pick the top word, 1+ = sampling allowed).
    - For deterministic outputs (tests, reproducible results): `temperature = 0`.
    - For creative outputs (marketing copy, brainstorming): higher temperature.

### GenAI for images

Three common task types:

- **Text → Image** — generate images from text prompts
  (e.g. *"Generate a blue sky with white clouds"*).
- **Image → Image** — generate images from images
  (e.g. *"Transform this image in Japanese anime style"*).
- **Image → Text** — generate text from images
  (e.g. *"Describe how many apples you see in the picture"*).

Image generation uses **Diffusion Models** (e.g. Stable Diffusion):

```mermaid
flowchart LR
    subgraph Training["Training — Forward diffusion"]
        IMG1["Clear image"] -->|add noise| NOISE1["Pure noise"]
    end
    subgraph Generating["Generating — Reverse diffusion"]
        NOISE2["Pure noise"] -->|remove noise step by step| IMG2["New image"]
    end
```

!!! info
    Unlike LLMs (which generate token by token), diffusion models generate the **whole
    image at once** through a series of denoising steps.
