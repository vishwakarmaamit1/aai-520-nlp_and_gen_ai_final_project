# AAI-520 Final Project — Multi-Agent Financial Analysis and Investment Research System

**Course:** AAI-520-IN2 — Introduction to Natural Language Processing and GenAI  
**Team:** Group 7  
**Members:** Rajkumar T, Amit Vishwakarma

---

## Overview

This project implements a multi-agent financial analysis and investment research prototype in a Jupyter Notebook. Given a stock ticker and a historical period, the system gathers market and company information, processes available news, produces a structured research report, evaluates report quality, and records useful information for later runs.

The project focuses on agentic workflow patterns and observable execution rather than autonomous trading. It is an educational prototype and **not financial advice**.

### Project objectives

- Demonstrate a **Plan-and-Execute** pattern by creating a research plan before executing its tasks.
- Use specialist agents for market history, company/financial information, and news.
- Demonstrate **prompt chaining** for news processing: Ingest → Preprocess → Classify → Extract → Summarize.
- Route tasks to the appropriate specialist using a router and registered tools.
- Use an **Evaluator–Optimizer** loop to check report completeness and attempt a bounded recovery action when news retrieval is empty or fails.
- Persist run summaries in JSON memory and use previous run notes to inform later planning.
- Visualize historical closing prices, rolling annualized volatility, and drawdown when sufficient market data is available.
- Test core behavior with synthetic inputs and mocked tools where possible.

**Implementation note:** Planning, routing, evaluation, optimization, and memory-informed adaptation are implemented with explicit Python logic and rules. The system does not use an LLM to autonomously reason through every step, and its memory is persistent note-taking—not model training.

---

## Repository Structure

```text
.
├── AAI_520_Multi_Agent_Investment_Research.ipynb
├── research_memory.json
└── README.md
```

- `AAI_520_Multi_Agent_Investment_Research.ipynb` — Main project notebook, including agent implementations, workflow execution, visualizations, trace output, and automated tests.
- `research_memory.json` — JSON-backed memory containing summaries from prior runs. The notebook can create or update this file when executed.
- `README.md` — Project overview and setup instructions.

If you add a `requirements.txt` file to the repository, you can use it for a reproducible dependency installation; the commands below install the notebook's main Python dependencies directly.

---

## Agent Architecture and Workflows

### Agent functions

1. **Planning:** Creates an ordered research plan based on the ticker, analysis period, and relevant prior memory.
2. **Tool use and execution:** Executes planned tasks, invokes registered data tools, and records task outcomes and errors.
3. **Self-evaluation and refinement:** Evaluates report sections and structured research evidence. When appropriate, the optimizer retries news retrieval once, regenerates the report, and reevaluates it.
4. **Adaptation across runs:** Stores evaluation scores, news status/counts, tool errors, unresolved checks, and actions. A later run can add a recovery task when a previous run had a news gap or tool error.

### Workflow patterns

- **Plan-and-Execute:** Build the plan first, then execute tasks in order and record the trace.
- **Prompt Chaining:** Process news through ingestion, preprocessing, classification, entity extraction, and summarization.
- **Routing to Specialists:** Direct market-history, company-profile/financial, and news tasks to the corresponding specialist.
- **Evaluator–Optimizer:** Check report quality, perform a bounded corrective action where feasible, regenerate the report, and evaluate again.

---

## Setup

### Requirements

- Python
- Jupyter Notebook or JupyterLab
- Internet access for live Yahoo Finance data retrieval

### Create and activate a virtual environment

**macOS/Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

**Windows PowerShell**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

### Install dependencies

```bash
pip install jupyter yfinance pandas numpy matplotlib
```

### Launch the notebook

From the repository directory, run:

```bash
jupyter notebook
```

Open `AAI_520_Multi_Agent_Investment_Research.ipynb` and run the cells **from top to bottom**.

The notebook uses `NVDA` and a one-year history period as its default example. You can change the `SYMBOL` and `PERIOD` variables in the setup cell. Other symbols may be used, subject to Yahoo Finance symbol conventions and data availability.

---

## How to Run and Interpret the Results

1. Run the notebook from the first cell to the last.
2. Review the plan table to see which research tasks were scheduled.
3. Review the execution trace and error output to determine which tasks and tools succeeded.
4. Review the synthesized investment research report, including company overview, market performance, financial indicators, recent news, and data limitations.
5. Review the evaluation history and optimizer actions to see whether a retry was attempted and whether it improved news coverage.
6. Review the visualizations and automated test output.
7. Run the notebook again to observe how the JSON memory from a prior run can influence planning.

Market values and news results are retrieved dynamically and may change between runs. A missing news result does not necessarily mean that no relevant news exists; the provider may return no records, change its response format, or be temporarily unavailable.

---

## Key Results and Deliverables

The notebook demonstrates:

- An explicit plan followed by sequential task execution.
- Specialist routing and registered data-tool invocation.
- A five-stage news-processing chain.
- Structured report generation from current research data rather than hard-coded sample metrics.
- Evaluator checks for report sections, market history, company profile, news status, usable news articles, and limitations.
- A bounded optimizer recovery attempt for empty or failed news retrieval, followed by report regeneration and reevaluation.
- JSON-backed memory that can inform a later run.
- Visualizations for historical closing price, rolling annualized volatility, and drawdown when enough valid market observations are available.
- Automated unit-style tests using synthetic data and mocked tools where possible.

The automated tests validate core program behavior; they do not guarantee that external data providers will always be available or that retrieved financial data is complete.

---

## Limitations and Responsible Use

- Yahoo Finance data availability, fields, and response formats may change; rate limits or network failures can occur.
- News classification uses a lightweight, rule-based approach and can miss context, sarcasm, or nuanced sentiment.
- Financial indicators require careful interpretation in the context of sector, reporting period, and data availability.
- A high report-quality score measures completeness checks; it does **not** indicate that a stock is a good investment.
- The system does not execute trades and does not provide individualized financial advice.
- Review outputs critically and verify important information with reliable, independent sources before making financial decisions.

Potential future improvements include SEC EDGAR filing integration, benchmark-relative performance, a validated sentiment model, broader data-source fallback, and human approval gates.

---

## Team Contributions

**Group 7 — Rajkumar T and Amit Vishwakarma**

The project deliverable brings together agent and workflow design, financial/news data processing, specialist routing, report synthesis, evaluation and optimization, persistent memory, visualizations, testing, and documentation.

Please update this section with the exact division of work agreed upon by the team before submission.

---

## Project Repository

GitHub: <https://github.com/vishwakarmaamit1/aai-520-nlp_and_gen_ai_final_project>
