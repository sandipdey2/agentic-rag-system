# Changes (round 4): aligned with Sandip's latest run (Python 3.13.9)

Repository: https://github.com/sandipdey2/agentic-rag-system

## Values from the latest run (the first output of each code cell)
- **Python:** 3.13.9 Anaconda (`/opt/anaconda3/bin/python`).
- **Examples:**
  - GAN: grounded 0.72. The answer added "that compete against each other".
  - 12 * 8 + 5: 101, grounded 0.83.
  - Transformer: grounded 1.00.
  - Two-step Transformer question: **"9 years after 2014"**, using `2023 - 2014`, with rag_search and safe_calculate called in the same turn. Wrong, but grounded 0.88.
  - Weather: refused by the model.
  - Injection: blocked by the guardrail.
  - 5 tool-server calls.
- **Evaluation:** 5/5 (100%).
  - VAE: 1.00.
  - RAG: 0.57, flagged, but still PASS.
  - 69: 1.00.
  - France: refused.
  - MNIST: 1.00.
  - Tool-server calls: 4 in the evaluation, 9 in total.
- **Metrics:** 3/3 answered; Faithfulness 0.91 (GAN 0.72, VAE 1.00, MNIST 1.00); Answer Relevance 1.00; Context Precision 1.00.

## Changes
- **agentic_system.ipynb:**
  - Only markdown cell 16 (Notebook Summary) changed. It had Faithfulness 0.98, now 0.91 with per-query values.
  - It is exactly 5 sentences and includes the repo link. Cell 0 already had the link.
  - No code cells or outputs were touched.
- **module_summary.md / .pdf:**
  - Kept the exact rubric headings.
  - Added the GAN row (0.72) and changed the two-step row to the wrong "9 years" answer.
  - Faithfulness is now broken down per query.
  - "Wrong answer labelled grounded" is the main failure, and the ethics section is grounded in it plus the RAG 0.57 case.
  - Removed "3 years" and the "earlier run" wording.
  - PDF regenerated: 6 pages, diagram embedded.
- **README.md:** the failure bullet now says "9 years" and Faithfulness has per-query values. Python 3.13.9 and the package versions match `requirements.txt`. The repo link is present.
- **architecture_diagram.png / .pdf:**
  - The SVG source has a footer, "Source: https://github.com/sandipdey2/agentic-rag-system".
  - The PDF subtitle carries the repo link.
  - The Reviewer bullet now describes this run's wrong answer.
- **requirements.txt:** Sandip's file, unchanged. It was checked for Python 3.13 wheels and resolves cleanly.
- **REVIEW_CHECK.md:** rewritten as a strict table.

## Checks
- `consistency_v4.py`: ALL OK. Every number matches across the notebook text, md, pdf (pdftotext), README and diagram PDF.
- The repo link appears in the notebook, md, pdf, README, diagram PDF/PNG, CHANGES and REVIEW_CHECK.
- No stale 3.13.5, 0.98, 0.93, "3 years", old package versions, or 4/5.

## Still open
Cells 11, 13 and 15 contain leftover output streams from the previous run. Fix: clear all outputs before Run All (VS Code: "Clear All Outputs" button; JupyterLab: Kernel → Restart Kernel and Clear All Outputs), or run `jupyter nbconvert --to notebook --execute --inplace agentic_system.ipynb` from a terminal with the key exported. If any number changes, update the documents again.
