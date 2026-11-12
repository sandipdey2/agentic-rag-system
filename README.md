# Agentic AI System — Multi-Agent with In-Process Tool Server

**Project 6 · Capstone**  
**GitHub:** https://github.com/sandipdey2/agentic-rag-system

An Orchestrator makes a single routing decision to a RAG agent, a Calculator agent, or a Guardrail agent. Tools are called through an in-process class that follows the MCP register–list–call pattern. There is no Agent Gateway and no multi-step Plan-Act-Observe loop.

## Evaluation (from notebook)

Held-out accuracy: **4/5 (80%)**.  
Successful: VAE, RAG, arithmetic (Result: 69), out-of-scope refusal.  
Failure: MNIST query was incorrectly refused by the Orchestrator (reported honestly).

## How to run

```bash
pip install -r requirements.txt
export OPENAI_API_KEY=your-key
export OPENAI_API_BASE=https://openai.vocareum.com/v1
jupyter notebook agentic_system.ipynb
```

Do not hardcode API keys. The notebook reads them from the environment.  
If you are on Python 3.13, regenerate `requirements.txt` with `pip freeze > requirements.txt` from the kernel you actually use.

## Files

| File | Purpose |
|------|---------|
| agentic_system.ipynb | Main notebook |
| knowledge_base.csv | Public 10-fact knowledge base |
| module_summary.pdf | System design report (matches notebook results and code) |
| architecture_diagram.pdf | Architecture diagram (matches the single-routing graph) |
| requirements.txt | Dependencies |
| README.md | This file |
