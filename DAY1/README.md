# 🧁 Day 1: LLM Fundamentals, API Integration & Safety Guardrails

Welcome to **Day 1** of the Agentic AI series. This module covers the foundational mechanics of interacting with Large Language Models (LLMs) programmatically, establishing reliable API connections, and integrating automated safety guardrails.

---

## 🎯 Objectives & Overview

- **Domain:** *Sweet Crumbs Home Bakery* — an online conversational assistant for a custom artisanal bakery.
- **Core Goal:** Move from conceptual understanding of LLMs to making robust, secured API calls with structured prompt instructions and real-time input/output filtering.

---

## 🧠 Concepts Explored

1. **LLM Mechanics:**
   - **Next-Token Prediction:** How generative models sample probability distributions over a token vocabulary.
   - **Statelessness:** Understanding that LLM APIs do not remember previous interactions natively; every call requires full context.
   - **Temperature Dial:** Balancing determinism ($T=0.2$ for precise facts) versus creativity ($T=1.2$ for varied language).
   - **Context Window Limits:** Managing input and output token consumption.

2. **API Inference with Groq & LangChain:**
   - Utilizing LangChain's `ChatGroq` integration for ultra-low latency inference using open-weights models (`openai/gpt-oss-120b`).
   - Loading environment variables reliably via `python-dotenv` for local environments and Colab Secrets for cloud execution.

3. **Input/Output Safety & Guardrails:**
   - Integrating **Meta Llama Prompt Guard** (`meta-llama/llama-prompt-guard-2-22m`).
   - Inspecting and classifying user inputs to detect prompt injection, jailbreaks, and unsafe content before invoking the main assistant.

---

## 🛠️ Step-by-Step Implementation Breakdown

### 1. Environment & API Setup
- Installs `langchain_groq`, `groq`, and `python-dotenv`.
- Securely pulls `GROQ_API_KEY` from local `.env` or Google Colab Secrets.

### 2. System Prompt Architecture
The assistant is bounded by strict domain rules:
- **Role:** Customer assistant for Sweet Crumbs Home Bakery.
- **Facts:** All items are baked to order; orders require a minimum of 24 hours notice; eggless versions are available for all cakes.
- **Catalogue & Pricing:** Defined pricing for cupcakes, sourdough loaves, signature cheesecakes, and custom celebration cakes.

### 3. Model Invocation
- Connects through LangChain's `ChatGroq`.
- Implements the `ask(question)` helper function wrapping system instructions and human queries into typed message tuples `[("system", SYSTEM), ("human", question)]`.

### 4. Safety Guardrail Integration
- Connects to the Groq API using the native `Groq` client.
- Evaluates incoming queries with `meta-llama/llama-prompt-guard-2-22m` to assign safety scores and flag adversarial inputs.

### 5. Interactive Testing Interface
- Built with `ipywidgets` and `IPython.display`.
- Features an interactive notebook UI with preset testing scenarios:
  - Policy checks (e.g., *"Can I get my cake in 4 hours?"*)
  - Dietary inquiries (e.g., *"Do you make gluten-free or eggless cupcakes?"*)
  - Edge cases and prompt injection tests.

---

## 📁 Files in This Directory

| File | Description |
| :--- | :--- |
| [`agenticAI_Day1.ipynb`](agenticAI_Day1.ipynb) | Complete Jupyter notebook with code cells, explanations, and interactive widgets. |
| [`.env.example`](.env.example) | Template file showing the required `GROQ_API_KEY` environment variable. |

---

## 🚀 How to Run

### Option A: Google Colab
1. Upload `agenticAI_Day1.ipynb` to [Google Colab](https://colab.research.google.com/).
2. Open the **Secrets** panel (🔑 icon on the left sidebar).
3. Add a secret named `GROQ_API_KEY` with your Groq API key and toggle **Notebook access** to ON.
4. Run all cells (`Ctrl + F9`).

### Option B: Local Setup
1. Ensure your virtual environment is active and dependencies are installed:
   ```bash
   pip install langchain-groq groq python-dotenv ipywidgets
   ```
2. Verify `.env` exists in the project root with your key:
   ```env
   GROQ_API_KEY=gsk_your_key_here
   ```
3. Open the notebook in VS Code or JupyterLab:
   ```bash
   jupyter notebook agenticAI_Day1.ipynb
   ```
