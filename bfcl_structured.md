# Berkeley Function Calling Leaderboard (BFCL) — Structured Guide

> A benchmark from UC Berkeley that measures how well LLMs use **functions, tools, APIs, and external resources** — not just whether they can write text.

---

## Table of Contents

1. [Overview](#1-overview)
2. [Background: Function Calling](#2-background-function-calling)
3. [Why BFCL Exists](#3-why-bfcl-exists)
4. [Version History](#4-version-history)
5. [Core Function-Calling Categories (V1)](#5-core-function-calling-categories-v1)
6. [Evaluation Methods](#6-evaluation-methods)
7. [Relevance and Irrelevance Detection](#7-relevance-and-irrelevance-detection)
8. [V2 — Live / Real-World Data](#8-v2--live--real-world-data)
9. [V3 — Multi-Turn Evaluation](#9-v3--multi-turn-evaluation)
10. [V4 — Holistic Agentic Evaluation](#10-v4--holistic-agentic-evaluation)
11. [V4 Scoring Methodology](#11-v4-scoring-methodology)
12. [Leaderboard Dimensions: FC vs Prompt, Cost, Latency](#12-leaderboard-dimensions-fc-vs-prompt-cost-latency)
13. [Evaluation Pipeline](#13-evaluation-pipeline)
14. [What BFCL Measures](#14-what-bfcl-measures)
15. [BFCL vs Conventional Benchmarks](#15-bfcl-vs-conventional-benchmarks)
16. [Role in Agentic AI](#16-role-in-agentic-ai)
17. [Strengths and Limitations](#17-strengths-and-limitations)
18. [Recommended Reporting Template](#18-recommended-reporting-template)
19. [BFCL in an LLM/VLM Agent Evaluation Framework](#19-bfcl-in-an-llmvlm-agent-evaluation-framework)
20. [Key Takeaways](#20-key-takeaways)
21. [References](#21-references)

---

## 1. Overview

| Item | Detail |
|---|---|
| **Name** | Berkeley Function Calling Leaderboard (BFCL) |
| **Developer** | UC Berkeley researchers (Gorilla project) |
| **Purpose** | Evaluate LLM tool use and function calling |
| **Current version** | **BFCL V4** |
| **Paper** | Patil et al., *BFCL: From Tool Use to Agentic Evaluation of LLMs*, ICML 2025 (PMLR vol. 267) |

**BFCL evaluates whether a model can determine:**

- **When** a tool should be used
- **Which** tool to select
- **What arguments** to supply
- **How** multiple tools are coordinated
- **How** tool use is maintained across multi-step interactions

---

## 2. Background: Function Calling

**Function calling** (also *tool calling*) is an LLM's ability to interact with external functions instead of answering purely from internal knowledge.

**Example:** *"What is the weather in Dhaka today?"*

```
User Query
   ↓
LLM  →  selects weather_tool
   ↓
weather_tool(location="Dhaka")
   ↓
Tool Result
   ↓
LLM  →  Final Answer
```

**Skills required of the model:**

| # | Skill |
|---|---|
| 1 | Understand the user's intention |
| 2 | Decide whether a tool is needed |
| 3 | Select the appropriate function |
| 4 | Identify required parameters |
| 5 | Generate valid parameter values |
| 6 | Execute or request the function |
| 7 | Interpret the returned result |
| 8 | Continue reasoning / make further calls if needed |

---

## 3. Why BFCL Exists

Traditional benchmarks cover QA, math, coding, knowledge retrieval, and language understanding. Agents, however, interact with **search engines, databases, APIs, file systems, business apps, memory systems, and enterprise services** — and a model strong on language benchmarks can still fail badly at tool use.

### Two core evaluation challenges (from the paper)

**3.1 Judging correctness of a call** — a call can be syntactically valid but semantically wrong.

```python
# User asked for Dubai, model produced London → valid syntax, wrong meaning
get_flight(origin="Dhaka", destination="London")   # ✗
get_flight(origin="Dhaka", destination="Dubai")    # ✓
```

**3.2 Obtaining diverse, realistic functions** — BFCL therefore uses expert-curated functions, user-contributed functions, real-world function descriptions, multiple programming languages, multi-turn interactions, and agentic tasks.

---

## 4. Version History

| Version | Main Focus |
|---|---|
| **V1** | Fundamental function calling |
| **V2** | Real-world / enterprise, community-contributed functions ("Live") |
| **V3** | Multi-turn and multi-step function calling |
| **V4** | Holistic agentic evaluation (web search, memory) |

V4 combines **agentic, multi-turn, live, non-live, and hallucination** evaluation; format sensitivity is reported separately.

---

## 5. Core Function-Calling Categories (V1)

| Category | Description | Example |
|---|---|---|
| **Simple** | One request, one function definition → generate the correct call | `book_flight(origin="Dhaka", destination="Singapore")` |
| **Multiple** | Several candidate functions; pick the right one | Choose among `search_flight`, `book_flight`, `cancel_flight`, `check_flight_status` |
| **Parallel** | One request needs several independent calls of the same kind | Weather in Dhaka **and** London → `get_weather("Dhaka")`, `get_weather("London")` |
| **Parallel Multiple** | Combination: select from several functions *and* issue multiple calls | Highest complexity of the four |

> *Multiple* tests **semantic function selection**, not merely valid JSON.

---

## 6. Evaluation Methods

### 6.1 AST (Abstract Syntax Tree) Evaluation

Generated calls are **parsed into a structured representation** rather than string-compared.

```
FunctionCall
├── Function: get_weather
└── Arguments
    ├── city = Dhaka
    └── unit = celsius
```

- Ignores superficial formatting differences
- Different text forms of the same call map to the same structure
- Scalable approach for function-call validation (a key BFCL contribution)

### 6.2 Execution-Based Evaluation

Functions are **actually executed** in applicable settings.

| Method | Checks |
|---|---|
| **AST** | Structure of the generated call |
| **Execution** | Whether the call behaves correctly when run |

An apparently correct call can still produce a wrong result when executed, so both are used.

---

## 7. Relevance and Irrelevance Detection

BFCL does **not** assume a tool should always be called.

**Example:** User asks *"Explain the theory of relativity."* while only `get_weather(city)` is available → the model should **not** call the tool.

| Term | Meaning |
|---|---|
| **Relevance** | The provided function fits the request |
| **Irrelevance** | The function does not apply; the model must abstain |

**Why it matters:** indiscriminate tool use causes incorrect actions, unnecessary API calls, higher cost, and potentially harmful behavior.

---

## 8. V2 — Live / Real-World Data

V2 Live contains **2,251** curated question–function–answer examples, selected from **67,000+** community-contributed real-world datapoints.

**Coverage:** simple, multiple, parallel, parallel-multiple, relevance and irrelevance detection, complex documentation, many candidate functions, nested parameters, varied query styles, specialized use cases, multilingual examples.

| Category | Examples |
|---|---:|
| Simple | 258 |
| Multiple | 1,053 |
| Parallel | 16 |
| Parallel Multiple | 24 |
| Irrelevance | 882 |
| Relevance | 18 |
| **Total** | **2,251** |

---

## 9. V3 — Multi-Turn Evaluation

V3 moves from isolated requests to **multi-turn interactions**, testing context retention, state management, multi-step reasoning, dynamic function selection, and handling of missing functions/parameters.

**Example flow:**

```
User:  Find my flight.
Agent: What is your booking number?
User:  AB123.
Agent: Your flight is Dhaka → London.
User:  Change it to tomorrow.
Agent: ...   (must remember AB123 and the route)
```

### Multi-turn categories

| Category | What it tests | Example |
|---|---|---|
| **Base** | Ordinary multi-turn tool use; reuse info from earlier turns | Coherent interaction across turns |
| **Missing Function** | Recognize when the needed tool doesn't exist; don't invent one | User: "Cancel my hotel" — only `search_hotel()` and `get_hotel_details()` exist, no `cancel_hotel()` |
| **Missing Parameter** | Detect absent required info; ask instead of fabricating | `book_hotel(city, date, number_of_guests)` but user never gave guest count |
| **Long Context** | Retain and retrieve info from lengthy conversations, many tools, large retrieved content, intermediate states | Agents facing long histories |

---

## 10. V4 — Holistic Agentic Evaluation

V4 evaluates function calling on real-world data plus broader **agentic** capabilities.

```
BFCL V4
├── Agentic
│   ├── Web Search
│   └── Memory
├── Multi-Turn
│   ├── Base
│   ├── Missing Function
│   ├── Missing Parameter
│   └── Long Context
├── Live
│   ├── Simple
│   ├── Multiple
│   ├── Parallel
│   └── Parallel Multiple
├── Non-Live
│   ├── Python
│   ├── Java
│   ├── JavaScript
│   ├── Multiple
│   ├── Parallel
│   └── Parallel Multiple
├── Hallucination Measurement
│   ├── Irrelevance
│   └── Relevance
└── Format Sensitivity   (reported separately)
```

### 10.1 Agentic — Web Search

- **100 human-crafted multi-hop questions**
- Standardized search interface so all models use the same search surface

```
Question → Search 1 → Intermediate info → Search 2 → More info → Reasoning → Final answer
```

### 10.2 Agentic — Memory

The agent must: **store** information → **retrieve** it later → **distinguish** relevant from irrelevant → **use** it in later interactions. Critical for persistent assistants and long-running agents.

### 10.3 Hallucination Measurement

Checks whether a model wrongly invokes tools when it shouldn't.

- Non-Live Irrelevance
- Live Irrelevance
- Relevance-related evaluation

```
Tool useful      → call tool
Tool not useful  → do NOT call tool
```

### 10.4 Format Sensitivity

Tests stability when function/prompt/output representation changes (JSON, XML, Markdown, plain text). Supported specifically for **prompt-based / non-FC models**.

### 10.5 Non-Live Component

Traditional function calling with predefined functions and test cases.

| Group | Categories |
|---|---|
| Language-specific | Python Simple AST, Java Simple AST, JavaScript Simple AST |
| Language-independent | Multiple AST, Parallel AST, Parallel Multiple AST |

Checks whether tool calling generalizes across programming-language representations.

### 10.6 Live Component

Real-world, community-contributed examples: **Live Simple AST, Live Multiple AST, Live Parallel AST, Live Parallel Multiple AST**, plus relevance/irrelevance detection.

---

## 11. V4 Scoring Methodology

### 11.1 Category weights

| Major Category | Weight |
|---|---:|
| Agentic | **40%** |
| Multi-Turn | **30%** |
| Live | **10%** |
| Non-Live | **10%** |
| Hallucination Measurement | **10%** |
| **Total** | **100%** |

**Formula:**

```
Overall = 0.40·A + 0.30·M + 0.10·L + 0.10·N + 0.10·H
```

A = Agentic, M = Multi-Turn, L = Live, N = Non-Live, H = Hallucination Measurement.

> **Overall Accuracy is *not* the average of every individual test case.**

### 11.2 Weighted vs. unweighted averaging

| Type | How it works | Used for |
|---|---|---|
| **Unweighted** | Subcategories count equally regardless of size: `(Score₁ + Score₂) / 2` | Prevents large subcategories dominating |
| **Weighted** | Weighted by number of test cases | The **Live** category |

Simply averaging visible leaderboard columns will **not** necessarily reproduce the Overall Accuracy.

---

## 12. Leaderboard Dimensions: FC vs Prompt, Cost, Latency

### FC vs Prompt

| Mode | Meaning |
|---|---|
| **FC** | Native function calling — model has built-in structured tool-call support |
| **Prompt** | Prompt-based — model is instructed to emit the call format via normal text generation |

These are different operating conditions; compare like with like.

### Practical metrics

| Metric | Unit | Meaning |
|---|---|---|
| **Cost** | USD | Estimated cost of running the full benchmark |
| **Latency** | Seconds | Response time |

This enables studying the **accuracy ↔ cost ↔ latency** trade-off for production agents.

---

## 13. Evaluation Pipeline

```
                User Query
                     │
                     ▼
              Available Tools
                     │
                     ▼
                  ┌─────┐
                  │ LLM │
                  └─────┘
        ┌────────────┼─────────────┐
        ▼            ▼             ▼
    Function      Multiple       No tool
    selection     functions      required
        │            │             │
        ▼            ▼             ▼
    Arguments     Parallel      Abstention
                   calls
        └────────────┬─────────────┘
                     ▼
              Generated Calls
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
    AST Evaluation      Execution / Agentic
          └──────────┬──────────┘
                     ▼
              Category Score
                     ▼
            Overall BFCL Score
```

In V4 the pipeline extends into multi-turn and agentic scenarios (web search, memory).

---

## 14. What BFCL Measures

BFCL is **not** just "can the model output JSON."

| Capability | What is evaluated |
|---|---|
| Function selection | Choosing the appropriate tool |
| Argument generation | Supplying correct parameters |
| Multiple calls | Choosing among multiple functions |
| Parallel calling | Producing multiple independent calls |
| Semantic correctness | Correct meaning of the invocation |
| Abstention | Avoiding inappropriate tool calls |
| Context management | Maintaining information across turns |
| Missing-info handling | Identifying missing parameters |
| Missing-tool handling | Recognizing unavailable capabilities |
| Long-context reasoning | Keeping relevant context |
| Web search | Multi-hop information retrieval |
| Memory | Storing and retrieving information |
| Format robustness | Handling different tool/prompt formats |

---

## 15. BFCL vs Conventional Benchmarks

| Conventional QA | BFCL |
|---|---|
| `Question → LLM → Answer` | `Question → LLM → reason about tools → select tool → generate arguments → execute → observe result → reason again → (maybe call another tool) → final answer` |

**Most relevant to:** AI agents, tool-augmented LLMs, autonomous assistants, enterprise AI, API-based applications, retrieval-and-action systems, multi-agent systems, long-running agents.

---

## 16. Role in Agentic AI

```
LLM
 ├── Search
 ├── Database
 ├── Calculator
 ├── Code Executor
 ├── APIs
 ├── Memory
 ├── File System
 └── External Applications
```

The LLM acts as the **decision-making layer** between natural language and external resources. Errors propagate:

```
Wrong Function → Wrong API → Wrong Tool Result → Wrong Reasoning → Wrong Final Action
```

---

## 17. Strengths and Limitations

### Strengths

| Strength | Detail |
|---|---|
| Diverse function types | Beyond simple examples |
| AST-based evaluation | Structured call comparison |
| Real-world data | Community-contributed Live set |
| Multi-turn evaluation | Sustained interaction, not just isolated requests |
| Agentic evaluation | Web search and memory in V4 |
| Practical metrics | Cost and latency alongside accuracy |

### Limitations / interpretation cautions

| Caution | Explanation |
|---|---|
| **Overall score hides weaknesses** | Models can differ sharply across simple calls, multi-turn, memory, web search, irrelevance — inspect category scores |
| **Version changes matter** | V1–V4 test different capabilities; scores across versions aren't directly comparable |
| **Evaluation mode matters** | FC vs Prompt are different conditions |
| **Accuracy isn't everything** | Latency, cost, memory behavior, tool-selection traits, and long-context performance vary independently |

---

## 18. Recommended Reporting Template

Avoid reporting only *"BFCL Overall Accuracy = XX%"*. Prefer:

```
BFCL Version: V4
Mode: FC / Prompt
Overall Accuracy: XX%

Agentic
  Web Search:            XX%
  Memory:                XX%

Multi-Turn
  Base:                  XX%
  Missing Function:      XX%
  Missing Parameter:     XX%
  Long Context:          XX%

Live
  Simple AST:            XX%
  Multiple AST:          XX%
  Parallel AST:          XX%
  Parallel Multiple AST: XX%

Non-Live
  Python Simple AST:     XX%
  Java Simple AST:       XX%
  JavaScript Simple AST: XX%
  Multiple AST:          XX%
  Parallel AST:          XX%
  Parallel Multiple AST: XX%

Hallucination
  Irrelevance:           XX%
  Relevance:             XX%

Cost:    $XX
Latency: XX seconds
```

---

## 19. BFCL in an LLM/VLM Agent Evaluation Framework

```
                  AI Agent
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
    Reasoning     Tool Use     Perception
        │            │            │
        ▼            ▼            ▼
    Reasoning       BFCL       Vision / VLM
    benchmarks   (function     benchmarks
                  calling)
        └────────────┼────────────┘
                     ▼
            Overall Agent Evaluation
```

BFCL covers the **tool-use / function-calling dimension** only — not general agent intelligence.

---

## 20. Key Takeaways

**Progression of BFCL:**

```
V1: Basic function calling
  ↓
V2: Real-world / Live functions
  ↓
V3: Multi-turn + multi-step interaction
  ↓
V4: Holistic agentic evaluation (web search + memory + tool use)
```

**Central idea:** effective tool use is more than a syntactically valid call. A capable agent must understand intent, pick the right tool, supply correct parameters, coordinate multiple calls, avoid irrelevant tools, maintain context, handle missing information, and increasingly work with external information and memory.

**One-line definition:** BFCL is a comprehensive benchmark of LLM function-calling and tool-use ability, covering function selection, argument generation, parallel/multiple calls, relevance detection, multi-turn interaction, and agentic capabilities (web search, memory), using AST-based, execution-based, and agentic evaluation.

---

## 21. References

1. **BFCL V4 Leaderboard** — https://gorilla.cs.berkeley.edu/leaderboard.html
2. **Patil et al., ICML 2025** — *The Berkeley Function Calling Leaderboard (BFCL): From Tool Use to Agentic Evaluation of Large Language Models*, PMLR vol. 267, pp. 48371–48392 — https://proceedings.mlr.press/v267/patil25a.html
3. **OpenReview paper** — https://openreview.net/forum?id=2GmDdhBdDk
4. **BFCL V2 Live blog** — https://gorilla.cs.berkeley.edu/blogs/12_bfcl_v2_live.html
5. **BFCL V4 Web Search / Agentic documentation** — https://gorilla.cs.berkeley.edu/blogs/15_bfcl_v4_web_search.html
