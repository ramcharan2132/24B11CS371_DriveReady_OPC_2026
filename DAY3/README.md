# 🧩 Day 3: Composable LCEL Chains, Automated Evaluation & Agent Foundations

Welcome to **Day 3** of the Agentic AI series. In this module, we advance from raw API calls to building production-grade **LangChain Expression Language (LCEL)** composable chains, implementing test-driven prompt evaluation suites, analyzing temperature determinism, and exploring autonomous multi-step reasoning agents powered by the **Google Gemini API**.

---

## 🎯 Objectives & Overview

- **Domains:** 
  - *Sweet Crumbs Home Bakery* (Chain & evaluation baseline)
  - *BrightPage Stationery & Office Supplies* (Prompt optimization & decision framework)
- **Core Goal:** Master LCEL chain construction (`prompt | llm | output_parser`), systematically evaluate prompt accuracy using automated CSV assertion suites, explore the `create_agent` loop, and learn when to use plain code vs single LLM calls vs autonomous agents.

---

## 🧠 Architecture & Engineering Concepts

### 1. The Composable LCEL Pipeline
```
┌─────────────────────────────────┐
│     ChatPromptTemplate          │
│  - System Instructions          │
│  - MessagesPlaceholder(history) │
│  - Human Input                  │
└────────────────┬────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│     ChatGoogleGenerativeAI      │
│     (gemini-3.5-flash-lite)     │
└────────────────┬────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│        StrOutputParser          │
│     (Clean Python string)       │
└─────────────────────────────────┘
```

### 2. Test-Driven Prompt Evaluation
Testing prompts with ad-hoc manual messages leads to regressions when prompt instructions change. We implement an automated evaluation engine that pairs realistic customer inquiries with deterministic `must_include` assertions to calculate quantitative pass rates (`passed / total`).

### 3. Temperature & Response Determinism
- **`temperature=0.3`**: Enforces strict adherence to catalogue prices, delivery rules, and exact fallback phrasing.
- **`temperature=1.0`**: Introduces response variability, useful for creative tasks but susceptible to drift in policy compliance.

### 4. Architectural Decision Framework: Choosing the Right Tool
| Business Job (BrightPage Stationery) | Optimal Approach | Why |
| :--- | :---: | :--- |
| Calculate cart subtotal, 10% bulk discount, and ₹40 delivery fee | **Plain Code** | Deterministic arithmetic — exact, zero latency, and free of cost. |
| Classify customer message as `order`, `inquiry`, or `complaint` | **One LLM Call** | Natural language semantic understanding needed; tool execution loops are redundant. |
| Check stock, compute bulk discounts, and place an order end-to-end | **Autonomous Agent** | Multi-step reasoning required — deciding which tools to call dynamically based on context. |

---

## 🛠️ Step-by-Step Implementation Breakdown

### 1. Environment & Gemini API Initialization
- Configures `langchain` and `langchain-google-genai`.
- Initializes `ChatGoogleGenerativeAI` using the high-efficiency `gemini-3.5-flash-lite` model.
- Automatically resolves `GOOGLE_API_KEY` across Google Colab Secrets, local `.env`, or user prompts.

### 2. Composable Chain & Multi-Turn Memory (`ask` helper)
- Uses `ChatPromptTemplate.from_messages` with:
  - System prompt enforcing bakery business rules.
  - `MessagesPlaceholder("history")` dynamically injecting conversational context.
  - Human message template `"{question}"`.
- Pipes directly to `llm` and `StrOutputParser()`:
  ```python
  chain = prompt | llm | StrOutputParser()
  ```
- Maintains state across turns while converting raw AI messages into clean Python strings.

### 3. Customer Policy Verification
- Tests multi-turn interactions against core business policies:
  - Advance notice requirements (24-hour baking schedule).
  - Dietary restrictions (eggless options available for all cakes).
  - Out-of-bounds requests (checking with the owner for non-local delivery).

### 4. Automated Evaluation Engine (`tests.csv`)
- Implements `run_tests(csv_path)` evaluating the prompt across 10 benchmark questions.
- Checks if expected keywords/numbers (e.g., `500`, `eggless`, `24 hours`) appear in model responses.
- Generates a pass/fail summary table and total percentage score.

### 5. Agent Sneak Peek (`create_agent`)
- Explores LangChain's agent abstraction:
  ```python
  from langchain.agents import create_agent
  agent = create_agent(model=llm, tools=[], system_prompt=SYSTEM)
  ```
- Understands how the model transitions from a passive pipeline into an agent that plans and selects tools dynamically.

### 6. Applied Prompt Engineering (BrightPage Stationery)
- Scales prompt engineering to a 10-item retail catalogue.
- Measures prompt effectiveness against [`brightpage_tests.csv`](brightpage_tests.csv).
- Compares prompt versions (`V1` baseline vs `V2` optimized) to quantitatively verify performance improvements.
- Tests temperature stability by running identical test suites under $T=1.0$.

---

## 📁 Files in This Directory

| File | Description |
| :--- | :--- |
| [`task.ipynb`](task.ipynb) | Complete Day 3 notebook containing LCEL chains, test evaluation functions, and agent preview cells. |
| [`tests.csv`](tests.csv) | Automated test suite containing 10 benchmark queries and assertions for Sweet Crumbs Bakery. |
| [`brightpage_tests.csv`](brightpage_tests.csv) | Automated test suite containing 10 benchmark queries and assertions for BrightPage Stationery. |
| [`README.md`](README.md) | Technical guide, architectural diagrams, and run instructions (this file). |

---

## 🚀 How to Run

### Option A: Google Colab (Recommended)
1. Upload [`task.ipynb`](task.ipynb), [`tests.csv`](tests.csv), and [`brightpage_tests.csv`](brightpage_tests.csv) to Google Colab.
2. In the left sidebar, click the **Secrets (🔑)** panel.
3. Add a secret named **`GOOGLE_API_KEY`** with your API key from [Google AI Studio](https://aistudio.google.com/app/apikey) and toggle **Notebook access** to ON.
4. Run all cells (`Runtime` -> `Run all` or `Ctrl + F9`).

### Option B: Local Setup
1. Activate your virtual environment and install dependencies:
   ```bash
   pip install langchain langchain-google-genai python-dotenv
   ```
2. Add your Gemini API key to your root `.env` file:
   ```env
   GOOGLE_API_KEY=your_gemini_api_key_here
   ```
3. Launch Jupyter:
   ```bash
   jupyter notebook task.ipynb
   ```
