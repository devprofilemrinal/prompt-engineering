# Prompt Engineering — Learning Notes

Structured notes on Prompt Engineering, built as a stepping stone toward Agentic AI development (tool use, ReAct loops, multi-agent orchestration with LangGraph/CrewAI).

## Why This Repo Exists

Prompt engineering is the skill of designing inputs to LLMs so they produce reliable, useful outputs. Before building AI agents — which are really just LLMs given tools, memory, and loops — it's essential to understand how to *talk to* an LLM effectively. This repo is a topic-by-topic knowledge base for that.

## How to Use This Repo

- Each file is self-contained — read them in order for a structured learning path, or jump to any topic as a reference.
- Notes are written in simple language with diagrams/flowcharts where helpful.
- Every phase builds on the previous one. Phase F (Agent-Specific Prompting) is the bridge into Agent Development.

## Roadmap

### Phase A — Foundations
| # | Topic | File |
|---|-------|------|
| 1 | What is a Prompt? | `01-foundations/01-what-is-a-prompt.md` |
| 2 | Prompt Structure & Components | `01-foundations/02-prompt-structure-components.md` |
| 3 | Zero-shot vs Few-shot Prompting | `01-foundations/03-zero-shot-vs-few-shot.md` |
| 4 | System vs User vs Assistant Messages | `01-foundations/04-system-vs-user-vs-assistant-messages.md` |

### Phase B — Core Techniques
| # | Topic | File |
|---|-------|------|
| 5 | In-Context Learning | `02-core-techniques/05-in-context-learning.md` |
| 6 | Chain-of-Thought Prompting | `02-core-techniques/06-chain-of-thought.md` |
| 7 | Self-Consistency & Tree-of-Thought | `02-core-techniques/07-self-consistency-tree-of-thought.md` |
| 8 | Prompt Templates & Variables | `02-core-techniques/08-prompt-templates-variables.md` |

### Phase C — Structuring Output
| # | Topic | File |
|---|-------|------|
| 9 | Structured Output Prompting | `03-structuring-output/09-structured-output-prompting.md` |
| 10 | Output Formatting Control | `03-structuring-output/10-output-formatting-control.md` |
| 11 | Prompt Chaining | `03-structuring-output/11-prompt-chaining.md` |

### Phase D — Reliability & Safety
| # | Topic | File |
|---|-------|------|
| 12 | Prompt Evaluation & Testing | `04-reliability-safety/12-prompt-evaluation-testing.md` |
| 13 | Guardrails & Prompt Injection Defense | `04-reliability-safety/13-guardrails-prompt-injection-defense.md` |
| 14 | Ambiguity & Hallucination Mitigation | `04-reliability-safety/14-ambiguity-hallucination-mitigation.md` |

### Phase E — Advanced Techniques
| # | Topic | File |
|---|-------|------|
| 15 | Meta-Prompting | `05-advanced-techniques/15-meta-prompting.md` |
| 16 | Automatic Prompt Optimization | `05-advanced-techniques/16-automatic-prompt-optimization.md` |
| 17 | Prompt Compression | `05-advanced-techniques/17-prompt-compression.md` |
| 18 | Retrieval-Augmented Prompting | `05-advanced-techniques/18-retrieval-augmented-prompting.md` |

### Phase F — Agent-Specific Prompting
| # | Topic | File |
|---|-------|------|
| 19 | Tool-Use / Function-Calling Prompts | `06-agent-specific/19-tool-use-function-calling-prompts.md` |
| 20 | ReAct Prompting | `06-agent-specific/20-react-prompting.md` |
| 21 | Multi-Agent Prompt Design | `06-agent-specific/21-multi-agent-prompt-design.md` |
| 22 | Model-Specific Prompting Quirks | `06-agent-specific/22-model-specific-prompting-quirks.md` |
| 23 | Prompt Versioning & Management in Production | `06-agent-specific/23-prompt-versioning-production.md` |

## Status

- [x] Repo scaffolded
- [ ] Topics 1–23 filled in (in progress, one at a time)

## Related Learning Track

This repo is a sub-track of a broader roadmap toward Agentic AI Engineering (Java + Spring AI + LangGraph). Prompt Engineering feeds directly into **Phase 2: Core Agent Patterns** (tool use, memory, ReAct loops, RAG) of that roadmap.
