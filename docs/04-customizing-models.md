# Customizing Models

This topic covers how to **customize** an existing Foundation Model for your use case —
fine-tuning and distillation — without training one from scratch.

Customizing is just one step in shipping a real product. For the full sequence from raw
data to a deployed application, see [GenAI Pipeline](05-genai-pipeline.md). For the NLP
vocabulary (tokens, embeddings), see [Tokens & Language](03-tokens-and-language.md).

## Fine-Tuning & Distillation

Two broad techniques to customize a Foundation Model:

- **Fine-tuning** (adjust the model's weights) — has two flavours:
    - **Supervised fine-tuning** — learns from labelled input/output pairs.
    - **Reinforcement fine-tuning** — learns from a reward function scoring outputs.
- **Distillation** — make the model smaller and cheaper while keeping most of its
  behaviour.

### Fine-tuning a model

- Adapt a **copy** of a foundation model with your own data.
- Fine-tuning **changes the weights** of the base foundation model.
- Training data must:
    - Adhere to a specific format (usually prompt/completion pairs, often in JSONL).
    - Be stored somewhere the training job can reach (object storage, dataset hub, …).

!!! warning
    Not all models can be fine-tuned — check the model card.

### Supervised fine-tuning

- Improves the performance of a model on specific tasks.
- Further trained on a particular field or area of knowledge.
- Uses labeled examples that are input–output pairs:

```json
{
  "prompt": "Who is Stephane Maarek?",
  "completion": "Stephane Maarek is an AWS instructor..."
}
```

You give it the question **and** the correct answer — so it learns exactly what to say.

### Reinforcement fine-tuning

- Improves your FM using **feedback-based learning**.
- You provide the input data (training data / prompts).
- You define a **reward function** to evaluate responses and judge which are good:
    - **Objective tasks** → a deterministic scoring function in code
      (e.g. "does the output match the expected JSON schema?").
    - **Subjective tasks** → use another model as a judge, given evaluation instructions.
- The model learns iteratively from reward scores, trying to achieve high scores over
  time.

!!! example "Technical customer support chatbot"
    Prompt: *"My app is running very slowly"*

    | Response | Score | Why |
    |---|---|---|
    | "Restart the app" | 5.0 | Helpful but superficial |
    | "That sounds frustrating. Can you tell me when it started?" | 9.0 | Empathetic, diagnostic |
    | "Please open a support ticket" | 2.0 | Not helpful, pushes work to user |

    The model learns to give responses like the second one.

### Supervised vs reinforcement fine-tuning (key difference)

| | You provide | The model learns from |
|---|---|---|
| **Supervised** | BOTH input AND output | The exact answers you gave |
| **Reinforcement** | ONLY input | Scores on the multiple outputs it generated |

### Distillation

- Making models **smaller and faster**.
- Up to **75% less expensive** than original models.
- A decrease in accuracy, but potentially acceptable.
- A larger (**teacher**) model transfers knowledge to a smaller (**student**) model.
- You provide input data (e.g. prompts); produces a lighter model with similar behavior.
- Focuses on efficiency, speed, and cost reduction.

### Fine-tuning: good to know

- Re-training an FM requires a higher budget.
- Supervised fine-tuning is usually cheaper (less computation, less data needed).
- Requires experienced ML engineers to perform the task.
- Running a fine-tuned model is usually **more** expensive than using the base model
  (exact pricing depends on the provider).

### Fine-tuning use cases

- A chatbot with a particular persona or tone, or for a specific purpose
  (assisting customers, crafting advertisements).
- Training with more up-to-date information than the model previously accessed.
- Training with **exclusive** data (your historical emails, customer service records).
- Targeted use cases (categorization, assessing accuracy).

## Key Takeaways

- **Fine-tuning** adapts a copy of a foundation model by changing its weights on your
  data; **distillation** shrinks a big model into a smaller, cheaper one.
- **Supervised fine-tuning** learns from input/output pairs you provide; **reinforcement
  fine-tuning** learns from a reward function scoring the outputs it generates.
- Fine-tuning needs formatted data, budget, and ML expertise — and a fine-tuned model
  usually costs more to run than the base model.
- Customizing is one step; the full path from data to product is the
  [GenAI Pipeline](05-genai-pipeline.md).
