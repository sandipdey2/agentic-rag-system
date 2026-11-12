# Agentic AI System — Multi-Agent with MCP Tool Server

**Project 6 · Capstone**  
**GitHub:** https://github.com/sandipdey2/agentic-ai-system

Multi-agent workflow with an LLM Orchestrator that routes to specialized agents. Tools are exposed through an MCP-style Tool Server (register, list_tools, call_tool, sanitize). Final answers are the actual tool results.

## Evaluation (from notebook)

Held-out accuracy: **4/5 (80%)**.  
Successful: VAE, RAG, arithmetic (Result: 69), out-of-scope refusal.  
Failure: MNIST query was incorrectly refused by the LLM Orchestrator (reported honestly).

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
| agentic_system.ipynb | Main notebook |
| knowledge_base.csv | Public 10-fact knowledge base |
| module_summary.pdf | System design report (matches notebook results) |
| architecture_diagram.pdf | Architecture diagram |
| requirements.txt | Dependencies |
| README.md | This file |
