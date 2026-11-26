# 🤖 Agentic AI Workshop & Implementations

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![LangChain](https://img.shields.io/badge/LangChain-Integration-green.svg)](https://www.langchain.com/)
[![Groq](https://img.shields.io/badge/LLM-Groq%20Cloud-orange.svg)](https://groq.com/)
[![Google Gemini](https://img.shields.io/badge/LLM-Google%20Gemini-blue.svg)](https://ai.google.dev/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-orange.svg)](https://jupyter.org/)

Hands-on repository tracking the progressive architecture of autonomous LLM agents — from single API calls and safety guardrails to composable LCEL chains, test-driven evaluation, and multi-step reasoning agents.

---

## 🗂️ Daily Modules & Navigation

| Module | Focus & Domain | Documentation | Notebook |
| :--- | :--- | :---: | :---: |
| **Day 1** | **LLM Fundamentals, Groq API & Prompt Guard**<br>Sweet Crumbs Home Bakery Assistant | [📖 Day 1 Guide](DAY1/README.md) | [💻 Day 1 Notebook](DAY1/agenticAI_Day1.ipynb) |
| **Day 2** | **Prompts, Memory & Structured Agent Architecture**<br>BrightPage Stationery & Office Supplies Assistant | [📖 Day 2 Guide](DAY2/README.md) | [💻 Day 2 Notebook](DAY2/agenticAI_Day2.ipynb) |
| **Day 3** | **LCEL Chains, Systematic Evaluation & Agent Sneak Peek**<br>Composable Chains, CSV Evaluations & Google Gemini | [📖 Day 3 Guide](DAY3/README.md) | [💻 Day 3 Notebook](DAY3/task.ipynb) |

---

## 🚀 Quickstart

1. **Clone & Set Up Virtual Environment:**
   ```bash
   git clone https://github.com/harsha-medapati/Agentic_Ai.git
   cd Agentic_Ai
   python -m venv venv
   .\venv\Scripts\activate   # On Windows
   # source venv/bin/activate # On Linux/macOS
   ```

2. **Configure API Keys:**
   ```bash
   cp .env.example .env
   ```
   Add your keys inside `.env`:
   ```env
   GROQ_API_KEY=gsk_your_groq_api_key_here
   GOOGLE_API_KEY=your_gemini_api_key_here
   ```
   *(For Google Colab, add them under the **Secrets (🔑)** panel and toggle **Notebook access** to ON).*

3. **Install Dependencies & Launch:**
   ```bash
   pip install langchain langchain-groq langchain-google-genai openai pydantic ipywidgets python-dotenv
   jupyter notebook
   ```

---

## 📁 Repository Structure

```text
.
├── .env.example                  # Environment variables template (Groq & Gemini)
├── .gitignore                    # Git ignore rules for secrets & caches
├── README.md                     # Main repository hub (this file)
├── DAY1/
│   ├── .env.example              # Local day-level env template
│   ├── README.md                 # Detailed Day 1 technical breakdown
│   └── agenticAI_Day1.ipynb      # Sweet Crumbs Bakery implementation
├── DAY2/
│   ├── README.md                 # Detailed Day 2 technical breakdown
│   └── agenticAI_Day2.ipynb      # BrightPage Stationery implementation
└── DAY3/
    ├── README.md                 # Detailed Day 3 LCEL & Evaluation guide
    ├── task.ipynb                # Composable chains, evaluation & agent notebook
    ├── tests.csv                 # 10 test cases for Sweet Crumbs Bakery
    └── brightpage_tests.csv      # 10 test cases for BrightPage Stationery
```
