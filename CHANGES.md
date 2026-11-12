# Changes (round 2): checked against the original five reviewer notes

Round 1 (wording fixes from the second reviewer's spec) is superseded by this round. The round-1 output is kept at `/workspace/capstone_fixed_v1`. See `REVIEW_CHECK.md` for before and after grades with line evidence. `requirements.txt` is unchanged (Sandip regenerates it).

## Notebook (`agentic_system.ipynb`): rewritten agent, executed with real outputs

- **Note 2 fix.** The router that returned a 3-way label (USE_RAG/USE_CALC/REFUSE) is replaced by a tool-calling agent: gpt-4o-mini (temperature 0) with `llm.bind_tools([rag_search, safe_calculate])`. Both are LangChain `@tool` functions whose bodies call `tool_server.call_tool`.
- **Graph.** `input_guard` → (`guardrail` → END) or (`agent` ⇄ `tools` loop → `reviewer` → END). The loop runs while the model returns tool calls, capped at `MAX_AGENT_TURNS = 4`.
- **Reviewer** now actually checks the answer: it computes the content-word overlap between the answer and the tool outputs and arguments. At 0.6 or above the answer is marked grounded; below that, a warning is appended. Refusals are labelled; answers given without any tool get a warning; hitting the turn limit gives a refusal.
- **Input guardrail.** A regex for instruction-override phrases. A match refuses without calling the model.
- **Fallback.** The no-key fallback is kept as a rule-based tool caller that emits the same tool calls.
- **Metrics cell.**
  - Faithfulness is now measured against the retrieved facts; it is not 1.0 by construction because the LLM writes the answer.
  - Answer relevance uses punctuation-free content tokens, which fixes the "gan?" versus "gan:" artifact.
  - Context precision now really is the share of retrieved facts whose topic matches the query. The never-called `ragas_context_precision` is removed.
  - Refusals are excluded from the averages and counted separately.
- **Evaluation check.** Now keyword-based on the answer, with refusal detection.
- **Examples.** A two-step demo query was added.
- **Unchanged cells.** The cells for the Python version, key note, imports and knowledge base, and ToolServer are unchanged (ToolServer keeps its round-1 rename).
- **Execution.** Executed end-to-end with `jupyter nbconvert --execute` in `/tmp/v313` (**Python 3.13.5**) with Sandip's Vocareum key from the environment. No errors. The key appears in no file: grepping the notebook and zip for the key and its prefix finds nothing.
- **Kernelspec metadata** is set to a generic `python3` so the notebook opens in his Jupyter.

### Mocked smoke test (before the real run)
Run outside the notebook in `/tmp/v313` with no key. Script: `/workspace/build/nb/smoke.py`; log: `/workspace/build/smoke_log.txt`. It ran every code cell with the rule-based caller, then six scripted `FakeMessagesListChatModel` scenarios:
1. a two-step loop (rag_search, then safe_calculate, then the answer; 3 agent turns, verdict grounded);
2. a model refusal;
3. a model that calls tools forever (stopped at 4 turns);
4. an answer with no tool call (warning);
5. a hallucinated answer after retrieval (low grounding, 0.17);
6. an injection attempt (guardrail, 0 model calls).

All assertions passed.

## Real-run results now used everywhere (notebook outputs = md = pdf = README)
- **Evaluation:** 5/5 (100%). MNIST is now answered; the previous version scored 4/5.
- **Tool-server calls:** 4 in the evaluation, 9 in total up to the end of the evaluation cell.
- **Metrics:** Faithfulness 0.98 (0.93/1.00/1.00), Answer Relevance 1.00, Context Precision 1.00; 3/3 answered.
- **Reviewer verdicts:** GAN 0.93, 12*8+5 → 101 at 0.83, Transformer 1.00, VAE 1.00, RAG **0.57 (flagged)**, 15+27*2 → 69 at 1.00, MNIST 1.00.
- **Observed failure:** on "In what year was the Transformer introduced, and how many years after 2014 is that?" the model called both tools in one turn, computed `2023 - 2014`, and answered "9 years". The Reviewer marked it grounded (0.88). This is reported in the notebook summary, the report, the README and the diagram page, and it is the basis of the ethics section.
- **No `[UPDATE AFTER RUN]` placeholders remain:** the run succeeded and every number comes from its outputs.

## module_summary.md / module_summary.pdf
- **Architecture table** covers persona, reasoning loop (ReAct-style, cites Yao et al., 2023), memory and state, tools, input guardrail and Reviewer. Diagram embedded as Figure 1.
- **Design section** rewritten: "Why One Tool-Calling Agent Rather Than Separate Agents". It answers the reviewer's question directly: the design changed because separate agents did not beat one agent with two tools. It gives three reasons and a stated limit, citing Wooldridge (2009).
- **New sections:** an observed-behaviour table from the saved run, plus evaluation and metrics sections with honest caveats (keyword-based eval, saturating relevance, lexical faithfulness).
- **Ethics:** "a confident wrong answer labelled grounded", from the real run. The old MNIST refusal appears only as a previous-version observation. Mitigations are listed.
- **References:** Yao et al. (2023), ICLR, added with all 7 authors. All 4 references are cited in the text.
- **CSV columns** stay corrected to `topic`, `fact`, `source`.
- **PDF** regenerated (python-markdown + WeasyPrint, 5 pages).

## README.md
Rewritten for the new design and real results. Configuration covers:
- Python **3.13.5** and the package versions of the saved run;
- the model and temperature;
- `SYSTEM_PROMPT`, `MAX_AGENT_TURNS`, `GROUNDING_THRESHOLD`;
- the fallback;
- the CSV schema;
- per-query state.

## architecture_diagram.pdf / .png
Redrawn: user query → input guardrail (blocked → guardrail refusal → END) → agent (persona, gpt-4o-mini + bind_tools, `MAX_AGENT_TURNS = 4`). The agent ⇄ tool node loop is labelled "act" and "observe". It also shows the ToolServer with `rag_search` and `safe_calculate` and `call_log`, the Reviewer (lexical, 0.6), Final Answer + Trace, and the AgentState panel ("per-query graph state, no memory across queries"). The "What the code does" bullets describe the real-run Reviewer miss.

## Still open
`requirements.txt` does not match the run environment (see `REVIEW_CHECK.md`, last section).
