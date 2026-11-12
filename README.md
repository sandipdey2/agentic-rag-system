# Agentic AI System — Tool-Calling Agent with Guardrail, Reviewer, and In-Process Tool Server

**Project 6 · Capstone**  
**GitHub:** https://github.com/sandipdey2/agentic-rag-system

A single gpt-4o-mini agent chooses and calls two tools itself (`bind_tools`) in a LangGraph loop: agent → tools → agent, at most 4 agent turns.

- A regex input guardrail blocks instruction-override text before the model.
- A Reviewer checks the final answer's word overlap with the tool outputs.
- Tools run through an in-process class that follows the MCP register–list–call pattern; it is not an MCP server process.

There is no separate Agent Gateway.

## Evaluation (saved notebook run)

- Held-out accuracy: **5/5 (100%)**: VAE, RAG, arithmetic (69), out-of-scope refusal, MNIST.
- The Reviewer flagged the RAG answer for an unsupported added sentence (overlap 0.57).
- Tool-server calls: 4 in the evaluation, 9 in total up to the end of the evaluation cell.
- Simplified RAGAS-style metrics (3/3 answered; see report for caveats): Faithfulness 0.98, Answer Relevance 1.00, Context Precision 1.00.
- Observed failure, reported in the notebook and report: on a two-step question the agent called both tools in one turn, computed 2023 − 2014 instead of 2017 − 2014, and the lexical Reviewer still marked the wrong answer grounded (0.88).
- The previous router-based version scored 4/5 (MNIST refused).

## How to run

```bash
pip install -r requirements.txt
export OPENAI_API_KEY=your-key
export OPENAI_API_BASE=https://openai.vocareum.com/v1
jupyter notebook agentic_system.ipynb
```

Do not hardcode API keys. The notebook reads them from the environment.

## Files

| File | Purpose |
|------|---------|
| agentic_system.ipynb | Main notebook (saved with outputs from the run described above) |
| knowledge_base.csv | Public 10-fact knowledge base |
| module_summary.md / .pdf | System design report (matches notebook results and code) |
| architecture_diagram.pdf / .png | Architecture diagram (guardrail, agent–tool loop, Reviewer, state) |
| requirements.txt | Dependencies |
| README.md | This file |

## Configuration

- **Python.** The saved notebook outputs were produced on Python 3.13.5 (CPython, Linux), with langchain-core 1.8.0, langchain-openai 1.7.4, langgraph 1.2.20, pandas 3.1.1, and scikit-learn 1.9.1. `requirements.txt` lists the pinned dependencies. Jupyter itself is not pinned (`pip install notebook`).
- **Agent LLM.** gpt-4o-mini, temperature 0, with `bind_tools`, through an OpenAI-compatible endpoint set by `OPENAI_API_BASE` (Vocareum).
- **Prompt and settings.** The system prompt (persona) is `SYSTEM_PROMPT` in the notebook's agent cell. The loop limit is `MAX_AGENT_TURNS = 4`, and the Reviewer threshold is `GROUNDING_THRESHOLD = 0.6`.
- **No-key fallback.** Without `OPENAI_API_KEY`, a rule-based tool caller emits the same tool calls so the graph runs. Results can differ, and the saved results use the LLM.
- **Data.** `knowledge_base.csv` has columns `topic`, `fact`, and `source` (10 rows).
- **State.** State exists for one query only; nothing is remembered between queries.
