# GAIA Benchmark — Structured Guide

> **GAIA (General AI Assistants)** is a benchmark that tests whether AI assistants can complete realistic tasks requiring reasoning, web browsing, multimodal understanding, information retrieval, and tool use. Introduced by Grégoire Mialon et al. (November 2023).

---

## Table of Contents

1. [Overview](#1-overview)
2. [Motivation and Objectives](#2-motivation-and-objectives)
3. [Dataset Composition](#3-dataset-composition)
4. [Difficulty Levels](#4-difficulty-levels)
5. [Capabilities Evaluated](#5-capabilities-evaluated)
6. [Evaluation Methodology](#6-evaluation-methodology)
7. [Leaderboards](#7-leaderboards)
8. [Model vs. Agent Architecture](#8-model-vs-agent-architecture)
9. [Running GAIA with Inspect AI](#9-running-gaia-with-inspect-ai)
10. [Running GAIA with AgentQuest](#10-running-gaia-with-agentquest)
11. [Failure Analysis](#11-failure-analysis)
12. [Limitations](#12-limitations)
13. [GAIA vs. Other Benchmarks](#13-gaia-vs-other-benchmarks)
14. [Research Opportunities](#14-research-opportunities)
15. [Best-Practice Checklist](#15-best-practice-checklist)
16. [Conclusion](#16-conclusion)
17. [References](#17-references)

---

## 1. Overview

| Item | Detail |
|---|---|
| **Full name** | General AI Assistants (GAIA) |
| **Authors** | Mialon et al. (Nov 2023), arXiv:2311.12983 |
| **Focus** | End-to-end task completion by an AI *system* (not just a model) |
| **Task nature** | Often easy for humans, hard for AI |
| **Inputs** | Natural-language questions, sometimes with files (images, documents) |
| **Output** | A precise final answer, checked against a reference |

Unlike benchmarks of isolated questions or specialized academic tasks, GAIA evaluates the assistant operating as a system: interpreting a request, choosing tools, gathering external information, processing files/images, and producing an exact answer.

### Official resources

| Resource | Purpose | Link |
|---|---|---|
| Original paper | Motivation, task design, evaluation, findings | https://arxiv.org/abs/2311.12983 |
| Hugging Face organization | Datasets, viewer, public results | https://huggingface.co/gaia-benchmark |
| Official leaderboard | Submission and comparison interface | https://huggingface.co/spaces/gaia-benchmark/leaderboard |
| Princeton HAL leaderboard | Accuracy, per-level results, cost, reproducibility | https://hal.cs.princeton.edu/gaia |
| AgentQuest | GAIA implementation guide | https://github.com/nec-research/agentquest/blob/main/agentquest/benchmarks/gaia/README.md |
| Inspect Evals | GAIA implementation for Inspect AI | https://ukgovernmentbeis.github.io/inspect_evals/evals/gaia/index.html |

---

## 2. Motivation and Objectives

### 2.1 Why GAIA?

Traditional benchmarks stress isolated skills (recall, math, coding, QA). A general assistant must combine skills.

**Example request:** *"Find the latest annual report of a company, identify its reported revenue, compare it with the previous year, and return the result in a spreadsheet."*

Required steps:

1. Understand the outcome and constraints
2. Search the web for the correct reports
3. Open and inspect PDFs
4. Extract the relevant figures
5. Calculate and verify
6. Produce a spreadsheet

Fluent text alone does not solve this. The original paper reported a large human–AI gap: **92% (humans) vs. 15% (GPT-4 with plugins)** — figures from the original study, *not* current model performance.

### 2.2 Research objectives

- Evaluate general-purpose assistant capabilities on practical tasks
- Measure reasoning combined with tools and multimodal inputs
- Test whether agents independently select and use appropriate resources
- Judge exact task completion, not just fluent responses
- Provide a common benchmark for comparing assistant architectures and configurations

### 2.3 Core principle

> A capable general AI assistant must do more than generate plausible answers: it must reliably complete tasks requiring reasoning and interaction with the outside world.

Relevant to: tool-using LLM agents, research assistants, document-processing agents, multimodal systems, agentic workflows.

---

## 3. Dataset Composition

| Source | Reported size |
|---|---|
| Original paper | **466** questions; answers to **300** retained for leaderboard evaluation |
| Princeton HAL | **450**-question benchmark; public validation set of **165** |

> These counts come from different representations of the benchmark — do **not** treat them as interchangeable. Check dataset versions and subsets before reproducing results.

### 3.1 Characteristics

| Characteristic | Description |
|---|---|
| Benchmark type | General-purpose AI assistant evaluation |
| Input | Natural-language questions, possibly with files |
| Expected output | A specific answer |
| Required capabilities | Reasoning, browsing, tool use, multimodal processing |
| Task format | Individual tasks with reference answers |
| Evaluation | Automated answer comparison and scoring |
| Subjects | LLM-based assistants and tool-using agents |

### 3.2 What makes a GAIA task distinctive

- Clearly defined outcome, but a non-obvious path to it
- May require finding info in a document, following a chain of references, processing an image, or combining facts
- Humans can usually solve them without specialized procedures
- Wording can be simple while the **execution** is complex

---

## 4. Difficulty Levels

| Level | Complexity | Description | Typical demands |
|---|---|---|---|
| **Level 1** | Lower | Basic assistant tasks; direct reasoning, simple lookup, one or few operations | Interpret question → retrieve/inspect a source → apply simple reasoning → answer |
| **Level 2** | Moderate | Multiple operations; several tools or sources | Decompose task → search/inspect files → combine intermediate results → verify constraints |
| **Level 3** | Higher | Advanced autonomy; complex dependencies between operations | Plan multi-step workflow → choose tools dynamically → resolve dependencies → stay correct throughout |

> These are conceptual guides, not official definitions. Difficulty isn't just step count — a short task can be hard if it needs a specific tool, an obscure source, or precise interpretation.

### 4.1 Why per-level reporting matters

An agent scoring 80% / 50% / 20% on Levels 1 / 2 / 3 hides where its workflow breaks if only the aggregate is reported. **Report aggregate accuracy alongside per-level accuracy.**

---

## 5. Capabilities Evaluated

| # | Capability | What it involves |
|---|---|---|
| 5.1 | **Reasoning & decomposition** | Understand requirements, identify dependencies, combine evidence, sequence actions |
| 5.2 | **Web browsing & retrieval** | Search, open pages, follow references, compare sources, extract facts |
| 5.3 | **Tool-use proficiency** | Select and operate suitable tools (search, code execution, file access, parsers) |
| 5.4 | **Multimodal understanding** | Interpret images/documents and link them to the question |
| 5.5 | **Extraction & synthesis** | Find relevant evidence, ignore noise, combine into the requested output |
| 5.6 | **Precision & verification** | Final answer must match the requested entity, value, or date |

> These overlap: a single task may test several at once, and the final score doesn't reveal which capability caused a success or failure.

---

## 6. Evaluation Methodology

### 6.1 General pipeline

```
1. Load benchmark tasks        → question, metadata, associated files
2. Execute the agent           → reasoning, planning, tool selection
3. Gather & process evidence   → browse, inspect files, run tools, combine results
4. Produce the final answer    → in the requested format
5. Score & analyze             → compare with reference, aggregate
```

Exact details depend on the agent framework and benchmark implementation.

### 6.2 Exact-match accuracy

```
Accuracy = (1 / N) · Σ 1( ŷᵢ = yᵢ )
```

- N = number of evaluated tasks
- ŷᵢ = agent's final answer
- yᵢ = reference answer
- 1(·) = 1 if correct, else 0

The implementation's answer-normalization and scoring rules determine what counts as correct. *Example: 70 of 100 correct → 70%.*

### 6.3 Per-level accuracy

```
A_l = (C_l / N_l) × 100
```

C_l = correct answers at level l; N_l = evaluated tasks at level l.

### 6.4 Supplementary metrics

| Metric | Meaning |
|---|---|
| Execution cost | Total model + tool expenditure |
| Latency | Time to complete a task |
| Tool-call count | Number of external operations |
| Failure rate | Tasks that fail or give no valid answer |
| Run-to-run variation | Accuracy differences across repeated runs |

These are engineering metrics, separate from the core correctness score.

### 6.5 Correctness vs. efficiency

| Agent | Correct | Avg. tool calls/task |
|---|---|---|
| A | 70 / 100 | 4 |
| B | 70 / 100 | 12 |

Same accuracy, different efficiency. Report correctness and resource usage **separately**, not as an undocumented combined score.

---

## 7. Leaderboards

### 7.1 Fields to examine in a leaderboard entry

| Field | What it tells you |
|---|---|
| Agent / system | Complete assistant configuration |
| Primary model | Main language model used |
| Accuracy | Proportion answered correctly |
| Level 1 / 2 / 3 scores | Performance by difficulty |
| Cost | Reported evaluation expenditure |
| Number of runs | How many runs were submitted |
| Verification status | Whether independently reproduced |

An entry usually represents an **agent system**, not just a foundation model (LLM + browsing, retrieval, file processing, code execution, planning…). Differences can't automatically be attributed to the LLM alone.

### 7.2 Hugging Face vs. HAL

| | Hugging Face leaderboard | Princeton HAL |
|---|---|---|
| Role | Official public submission/results interface | Additional evaluation platform |
| Reports | Model/agent results | System accuracy, difficulty breakdowns, cost, run counts, verification |
| Note | Live interface may change | Updates paused while the team focuses on measuring agent reliability |

> **Important:** don't compare scores as if from identical conditions. Check dataset subset, agent scaffold, model version, available tools, execution limits, and scoring procedure.

---

## 8. Model vs. Agent Architecture

A foundation model generates and interprets text; a general assistant needs more components to finish real-world tasks.

```
User query + task context
        ↓
Agent orchestration layer
  (task planning · tool selection · context management · stopping conditions)
        ↓
   ┌────────────┐     ┌─────────────────────────┐
   │    LLM     │ ←→  │       Tool layer        │
   │ reasoning/ │     │ search, files, code, API│
   │ generation │     └─────────────────────────┘
   └────────────┘
        ↓
Evidence integration + final answer
```

### 8.1 Components

| Component | Responsibility |
|---|---|
| Foundation model | Interprets instructions, reasons, generates responses |
| Planner | Breaks complex requests into steps |
| Tool router | Chooses which tools to invoke |
| Browser / search | Retrieves external information |
| File processor | Reads PDFs, spreadsheets, images |
| Code executor | Calculations and structured data processing |
| Memory / context manager | Keeps relevant information across steps |
| Final-answer generator | Produces the requested result |
| Evaluator | Checks answer against reference |

Not every implementation needs every component.

### 8.2 Why it matters

Two systems with the **same model** — one with a browser, PDF parser, and code interpreter, the other text-only — will differ in performance because of the **system configuration**, not the model. For reproducibility, document both the model and the agent infrastructure.

---

## 9. Running GAIA with Inspect AI

**Inspect AI** is an evaluation framework built around tasks, models, solvers, and scorers. **Inspect Evals** provides the GAIA implementation.
Guide: https://ukgovernmentbeis.github.io/inspect_evals/evals/gaia/index.html

### 9.1 Installation

```bash
python -m venv .venv

# Windows (PowerShell)
.venv\Scripts\Activate.ps1
# Linux / macOS
source .venv/bin/activate

pip install inspect-ai inspect-evals
inspect --help
```

Package requirements and task names can change — check the official guide first.

### 9.2 Configure model access

```powershell
$env:OPENAI_API_KEY = "your-api-key"
```

- Never commit API keys or share notebooks with real credentials
- Record provider, model identifier, tool configuration, and reasoning/token limits

### 9.3 Identify and run the task

```bash
inspect eval --help
inspect eval <verified-gaia-task-name> \
  --model <provider/model-name>
```

The task name is a placeholder on purpose — verify it in the installed version.

### 9.4 What to examine in the logs

- Input question and associated files
- Intermediate model responses
- Tool calls and returned results
- Final answer and assigned score
- Errors, timeouts, execution failures

This helps separate reasoning failures from retrieval failures, tool errors, and wrong final answers.

### 9.5 Suggested experimental protocol

1. Select a defined GAIA dataset split
2. Fix the model and agent configuration
3. Confirm required tools are available
4. Run with documented limits
5. Save logs and task-level results
6. Calculate aggregate and per-level accuracy
7. Record cost and latency
8. Repeat selected runs to check variability

For papers, also document package versions, model version, dataset revision, evaluation subset, and scoring implementation.

---

## 10. Running GAIA with AgentQuest

**AgentQuest** is another agent-evaluation framework with a GAIA-specific guide:
https://github.com/nec-research/agentquest/blob/main/agentquest/benchmarks/gaia/README.md

The README is the source of truth for dependencies, setup, dataset handling, and invocation syntax.

### 10.1 Workflow

| Step | Action |
|---|---|
| 1 | **Set up AgentQuest** — clone the repo, install documented dependencies |
| 2 | **Configure the evaluation** — follow the GAIA README for benchmark and model/agent |
| 3 | **Prepare data and tools** — verify access to questions, files, supported tools |
| 4 | **Run the benchmark** — execute the documented GAIA command |
| 5 | **Analyze results** — compare accuracy, per-level performance, execution characteristics |

### 10.2 Inspect AI vs. AgentQuest

| Aspect | Inspect Evals | AgentQuest |
|---|---|---|
| Main purpose | Structured model and agent evaluations | Agent benchmarking within AgentQuest |
| GAIA resource | Dedicated evaluation docs | GAIA-specific README |
| Interface | Inspect CLI and evaluation APIs | Repository-specific workflow |
| Trace analysis | Inspect logs and task records | Depends on the implementation |
| Best starting point | Projects already using Inspect | Projects already using AgentQuest |

Verify details against current versions. For fair cross-framework comparison, check equivalent datasets, model configurations, tool access, execution limits, and scoring rules.

---

## 11. Failure Analysis

Accuracy shows *how often* an agent failed, not *why*.

### 11.1 Proposed error taxonomy

*(Not official GAIA scoring categories.)*

| Failure category | Description | Example |
|---|---|---|
| Question understanding | Misinterprets the request | Returns a summary when a specific value was requested |
| Planning | Unsuitable sequence of steps | Calculates before gathering the data |
| Retrieval | Fails to find the required evidence | Uses an irrelevant or incomplete source |
| Tool execution | Tool call fails or is malformed | Document parser can't process the file |
| Multimodal processing | Misreads information | Wrong value extracted from an image |
| Evidence synthesis | Combines information incorrectly | Mixes values from different reporting periods |
| Final-answer formatting | Violates expected answer format | Extra text breaks strict comparison |
| Stopping / resource limits | Stops early or exhausts budget | Hits the tool-call limit |

### 11.2 Procedure for each incorrect task

1. Inspect the question and expected answer
2. Read the execution trace
3. Identify the first incorrect or unsuccessful operation
4. Determine whether it originated in the model, tool, data, or orchestration layer
5. Record the failure category and evidence
6. Test a targeted fix on a held-out subset

*Example:* if retrieval is correct but formatting is wrong, improving final-answer validation beats swapping the model.

---

## 12. Limitations

| Area | Consideration |
|---|---|
| **Coverage** | A finite dataset can't represent every task, domain, language, or interaction; high GAIA scores don't guarantee production performance |
| **Tool/infrastructure dependence** | Web access, file libraries, execution environments, and APIs affect results |
| **Dataset/version differences** | Paper, official resources, and third-party platforms may expose different subsets; interpret counts in context |
| **Answer-based scoring** | Reproducible, but doesn't show whether the process was sound — two agents can reach the same answer via reliable vs. unreliable reasoning; supplement with traces |
| **Cost and latency** | More tool calls, bigger models, or longer reasoning may raise accuracy but also expenditure and response time |
| **Reproducibility** | Model updates, changing websites, API behavior, tool versions, and randomness affect results |

---

## 13. GAIA vs. Other Benchmarks

| Benchmark category | Main focus | Difference from GAIA |
|---|---|---|
| General knowledge / reasoning | Answering questions, solving reasoning problems | Often less external tool interaction |
| Function-calling | Correct selection and invocation of functions | Isolates tool-call correctness more directly |
| Web-agent | Navigating websites, browser tasks | More focused on web interaction |
| Software engineering | Modifying code, resolving repo issues | Focused on coding environments |
| **GAIA** | End-to-end general assistant tasks | Combines reasoning, tools, information gathering, multimodal capabilities |

Categories can overlap; GAIA's defining focus is general-purpose assistant task completion.

---

## 14. Research Opportunities

| Direction | Question | Evaluate with |
|---|---|---|
| **Adaptive tool selection** | Can agents choose tools by task needs rather than a fixed sequence? | Accuracy, unnecessary tool calls, cost, failure rate |
| **Retrieval & evidence verification** | Do source verification, ranking, and cross-checks improve correctness? | Accuracy, evidence relevance, unsupported claims, retrieval latency |
| **Multi-agent collaboration** | Single agent vs. specialized retrieval/reasoning/verification agents? | Accuracy, cost, latency, coordination overhead, error recovery |
| **Reliability-aware agents** | Can agents recognize uncertainty, retry, and abstain? | Accuracy, recovery success, run variance, unsupported-answer rate |
| **Accuracy–cost optimization** | How do model choice, tool budgets, caching, adaptive reasoning affect performance under a fixed budget? | Accuracy at fixed cost, cost per correct task, latency |

### Example experimental design

| Configuration | Description |
|---|---|
| **Baseline** | Fixed tools, simple execution loop |
| **Improved retrieval** | Baseline + better source selection and evidence verification |
| **Adaptive agent** | Improved retrieval + task planning and dynamic tool selection |

Keep the evaluation subset and conditions consistent; report aggregate and per-level accuracy, cost, latency, and error categories; use a separate development subset for tuning to avoid contaminating the final evaluation. The goal is to find which system changes produce measurable gains, rather than crediting everything to the LLM.

---

## 15. Best-Practice Checklist

- [ ] Record the benchmark dataset revision and evaluated split
- [ ] Document model provider, model version, and generation settings
- [ ] Record agent framework and dependency versions
- [ ] Document available tools, permissions, and configurations
- [ ] Use the documented GAIA scoring procedure
- [ ] Report aggregate accuracy **and** per-level accuracy
- [ ] Record evaluation cost and latency where available
- [ ] Inspect failed tasks and preserve execution traces
- [ ] Repeat selected runs to measure variability
- [ ] Separate development experiments from final evaluation

---

## 16. Conclusion

GAIA evaluates practical, general-purpose assistant capability — tasks that combine reasoning, information retrieval, tool use, and multimodal processing. Its three difficulty levels characterize performance as complexity grows; the Hugging Face and Princeton HAL leaderboards offer complementary views; Inspect Evals and AgentQuest provide implementation routes.

The most informative use of GAIA is not a single accuracy number, but evaluating the **complete agent configuration**, analyzing task-level failures, examining cost and latency, and documenting conditions so results are reproducible and correctly interpreted.

---

## 17. References

1. Mialon, G., et al. (2023). *GAIA: a benchmark for General AI Assistants.* arXiv:2311.12983 — https://arxiv.org/abs/2311.12983
2. GAIA Benchmark — Hugging Face organization — https://huggingface.co/gaia-benchmark
3. GAIA Benchmark — Hugging Face leaderboard — https://huggingface.co/spaces/gaia-benchmark/leaderboard
4. Princeton HAL — GAIA leaderboard — https://hal.cs.princeton.edu/gaia
5. NEC Research — AgentQuest GAIA README — https://github.com/nec-research/agentquest/blob/main/agentquest/benchmarks/gaia/README.md
6. UK AISI — Inspect Evals: GAIA — https://ukgovernmentbeis.github.io/inspect_evals/evals/gaia/index.html

> *Note:* for implementation-specific commands, exact dataset revisions, and current leaderboard results, consult the linked official resources before running experiments or reporting results.
