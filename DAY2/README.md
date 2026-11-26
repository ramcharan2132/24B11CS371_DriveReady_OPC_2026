# 📚 Day 2: Prompts, Memory, Structured Output & Building an Agent

Welcome to **Day 2** of the Agentic AI series. In this module, we transition from single, isolated API calls to building an interactive, stateful conversational agent with multi-turn memory, few-shot classification, and schema-enforced structured outputs.

---

## 🎯 Objectives & Overview

- **Domain:** *BrightPage Stationery & Office Supplies* — an intelligent customer support and ordering assistant for a retail stationery business.
- **Core Goal:** Master the 4-pillar system prompt design, implement a few-shot intent classifier, extract structured JSON orders, and build manual sliding-window conversation memory with error resilience.

---

## 🧠 Key Agentic Architecture Components

```
                    ┌────────────────────────────┐
                    │    Incoming User Message   │
                    └─────────────┬──────────────┘
                                  │
                 ┌────────────────┴────────────────┐
                 ▼                                 ▼
      [Few-Shot Intent Classifier]      [4-Pillar Conversational Loop]
      (Order / Inquiry / Complaint)     (Role · Facts · Rules · Format)
                 │                                 │
                 ▼                                 ▼
      [JSON Schema Extraction]          [Sliding-Window Memory Loop]
      (Structured Order Object)         (Stateful History with MAX_TURNS)
```

---

## 🛠️ Step-by-Step Implementation Breakdown

### 1. The 4-Pillar System Prompt Framework
A robust system prompt establishes clear boundaries and deterministic behavior:
- **Pillar 1: Role** — Friendly, professional customer service agent for BrightPage Stationery.
- **Pillar 2: Facts** — Explicit inventory pricing:
  - A4 Paper Ream (500 sheets): ₹320
  - Gel Pens Pack (10 pens): ₹150
  - Spiral Notebooks: ₹85
  - Pastel Highlighters (Set of 6): ₹120
  - Store policies: Free shipping on orders over ₹500 (standard delivery: ₹50); 10% discount for orders of 10+ items of a single product.
- **Pillar 3: Rules** — Never invent unlisted products; for out-of-stock or unlisted items, strictly respond: *"I'll check with the store manager and get back to you."* Never promise same-day delivery.
- **Pillar 4: Format** — Keep responses concise (under 60 words) and use clean bullet points when listing two or more items.

### 2. Few-Shot In-Context Prompting (Intent Classifier)
- Provides representative customer conversation examples directly within the prompt context.
- Classifies incoming messages into exact single-token labels: `order`, `inquiry`, or `complaint`.
- Dramatically increases classification precision compared to zero-shot queries.

### 3. Structured JSON Output Extraction
- Converts unstructured customer natural language into machine-readable JSON schemas:
  ```json
  {
    "customer_name": "Ramu",
    "items": [
      { "item": "A4 Paper Ream", "quantity": 2 },
      { "item": "Gel Pens Pack", "quantity": 1 }
    ],
    "delivery_required": true,
    "special_notes": "Deliver before 5 PM"
  }
  ```
- Ensures downstream systems (databases, order fulfillment pipelines) receive clean, validated data.

### 4. Python Foundations for Autonomous Agents
- Implements production-ready code patterns:
  - Typing hints (`typing.List`, `typing.Dict`, `typing.Optional`)
  - Product catalogue dictionary mappings
  - A reusable execution logging decorator (`@log_call`) that logs function entry, execution duration, and results.

### 5. Manual Stateful Memory & Resilient Chat Loop
- **The Problem:** LLM APIs are stateless; they retain zero memory between requests.
- **The Solution:** A custom conversation history buffer maintaining a list of message pairs.
- **Sliding-Window Pruning:** Automatically trims history when conversation length exceeds `MAX_TURNS` (e.g., keeping the last 3-4 turns) to avoid exceeding token context limits.
- **Fault-Tolerance:** Wraps model calls in `try / except` blocks catching HTTP 429 (`RateLimitError`) and network exceptions with user-friendly fallbacks.

### 6. Interactive UI Dashboard
- Built with `ipywidgets` for interactive testing in Jupyter and Google Colab:
  - Interactive multi-turn chat widget with dynamic chat bubbles.
  - Policy preset buttons (e.g., *"Test ₹500 Free Delivery Rule"*, *"Test Out-of-Stock Fallback"*).
  - Live Intent Classifier testing panel.
  - Live JSON Order Extraction panel.

### 7. Student's 10% Verification Suite
Hands-on validation experiments:
- **Experiment 1 (Temperature Dial):** Comparing $T=0.2$ (factual, strict adherence) vs $T=1.2$ (creative, prone to hallucination).
- **Experiment 2 (Hallucination Boundaries):** Testing model resistance to unlisted item requests (e.g., asking for fountain pens or laminators).
- **Experiment 3 (Memory Boundary Test):** Verifying that older dialogue beyond `MAX_TURNS` is correctly pruned while recent context is preserved.

---

## 📁 Files in This Directory

| File | Description |
| :--- | :--- |
| [`agenticAI_Day2.ipynb`](agenticAI_Day2.ipynb) | Complete Jupyter notebook with code implementations, UI widgets, and verification tests. |

---

## 🚀 How to Run

### Option A: Google Colab
1. Upload `agenticAI_Day2.ipynb` to [Google Colab](https://colab.research.google.com/).
2. Under the **Secrets** panel (🔑), ensure `GROQ_API_KEY` is added and **Notebook access** is ON.
3. Run all cells (`Runtime` -> `Run all`).

### Option B: Local Setup
1. Activate your virtual environment and install requirements:
   ```bash
   pip install langchain-groq openai pydantic ipywidgets python-dotenv
   ```
2. Ensure your root `.env` has your Groq API key:
   ```env
   GROQ_API_KEY=gsk_your_key_here
   ```
3. Launch Jupyter:
   ```bash
   jupyter notebook agenticAI_Day2.ipynb
   ```
