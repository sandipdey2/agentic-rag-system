# Changes (round 3): aligned with Sandip's Python 3.13.9 re-run

Source of truth: Sandip's re-run notebook (Python 3.13.9, Anaconda) and his regenerated `requirements.txt` (14 pins). Both are kept as he supplied them, except that the final summary markdown cell is rewritten. See `REVIEW_CHECK.md` for the rubric grades.

## Values taken from his run
- Python 3.13.9 (cell 2).
- Example cell:
  - 12 * 8 + 5 → 101, grounded 0.83;
  - Transformer explanation, grounded 1.00;
  - two-step Transformer question: `rag_search` and `safe_calculate` in one turn, expression `2017 - 2014`, answer "3 years" (correct), grounded 0.88;
  - weather refused by the model;
  - injection blocked by the guardrail;
  - 5 tool-server calls.
- Evaluation:
  - 5/5 (100%);
  - RAG: low grounding 0.57 with warning, still PASS;
  - 15 + 27 * 2 → 69, 1.00;
  - France refused;
  - MNIST 1.00;
  - 4 tool calls in the evaluation, 9 in total.
- Metrics: 3/3 answered; Faithfulness 0.91 (VAE 1.00, MNIST 1.00); Answer Relevance 1.00; Context Precision 1.00.
- **Changed since the box run:** the "9 vs 3 years" failure did not recur (his run computed `2017 - 2014`), and Faithfulness fell from 0.98 to 0.91. The documents no longer present "9 years" as this run's failure. It is mentioned only as an earlier run of the same code.

## Files
- **agentic_system.ipynb:** only markdown cell 17 (Notebook Summary) changed. It had stale 3.13.5 values and the old failure; it is now exactly 5 sentences with his values. No code cells and no outputs were changed.
- **module_summary.md / .pdf:** rewritten with the exact rubric headings:
  - Overview
  - System Architecture and Design (including the single-agent justification)
  - Decision Logic and Behavior
  - Safety, Reliability, and Transparency
  - Observed Behavior and Limitations
  - Ethical and Responsible Use Considerations
  - Future Improvements
  - References

  All numbers come from his run. The ethics section is grounded in the RAG 0.57 pass and the Transformer 0.88 cases. The four citations are intact. The PDF is regenerated with the diagram embedded (6 pages).
- **README.md:** Python 3.13.9 (Anaconda), the package versions from his `requirements.txt`, and his run's values. Configuration, data and prompt are documented.
- **architecture_diagram.pdf:** one bullet changed. It had said the saved run marked a wrong multi-step answer grounded, which is no longer true. The PNG is unchanged (his copy is identical).
- **requirements.txt:** his file, unchanged.
- **REVIEW_CHECK.md:** rewritten as a strict rubric table.

## Consistency check
`/workspace/build/consistency_v3.py` reads his run's output streams and checks every number in the notebook markdown, md, pdf text (`pdftotext`) and README. Result: ALL OK. No stale 3.13.5, 0.98, 0.93, 4/5 (80%), or old package versions remain in any document.

## Open issue (blocking for Execution, Organization, and Integrity)
In notebook cells 12, 14 and 16, the first output stream is left over from the earlier box execution (Python 3.13.5), and his run's output for the first query of each of those cells is missing. The cells therefore show contradictory results. Fix: in JupyterLab or Notebook, use Kernel → Restart Kernel and Clear All Outputs, then Run All, then Save. If any number changes, rerun the consistency check and update the documents. Also consider removing or commenting out the `%pip freeze > requirements.txt` cell, which overwrites the curated 14-pin file on every Run All.
