# 09 — Layered Architecture

> **Canonical 5-Layer Model for Error-Aware Agent Systems**

---

## 📜 Canonical Statement

> **LifeOS is not a model, but a system in which learning models are embedded within explicit layers of failure memory, knowledge, long-term experience, and agent-level responsibility.**

---

## 🏗️ The Five Layers

```
┌──────────────────────────────────────────────────────────┐
│  L4 — AGENT / OPAR LOOP                                  │
│  Decision · Action · Responsibility                      │
│  "Who is accountable?"                                   │
└─────────────────────────▲────────────────────────────────┘
                          │
┌─────────────────────────┴────────────────────────────────┐
│  L3 — MemoryOS                                           │
│  Episodic · Narrative · Long-term Memory                 │
│  "What have I lived?"                                    │
└─────────────────────────▲────────────────────────────────┘
                          │
┌─────────────────────────┴────────────────────────────────┐
│  L2 — KnowledgeOS                                        │
│  Facts · Concepts · Documents · RAG                      │
│  "What does the world say?"                              │
└─────────────────────────▲────────────────────────────────┘
                          │
┌─────────────────────────┴────────────────────────────────┐
│  L1 — PEM (Persistent Error Memory)                      │
│  Failure Modes · Negative Memory                         │
│  "Where did I fail before?"                              │
└─────────────────────────▲────────────────────────────────┘
                          │
┌─────────────────────────┴────────────────────────────────┐
│  L0 — MODELS                                             │
│  LLMs · Policies · Vision · Speech                       │
│  "Compute, predict, generate" (stateless)                │
└──────────────────────────────────────────────────────────┘
```

---

## 🔍 Layer Specifications

### L0 — Learning Models

**Nature**: Stateless computational engines

**Examples**:
- Large Language Models (GPT-4, LLaMA, Claude)
- Reinforcement Learning policies
- Vision and speech models
- Classification and regression models

**Role**: Compute, predict, generate

**Key Properties**:
- ❌ No explicit memory
- ❌ No responsibility
- ❌ No awareness of past failures
- ✅ Pure computation

**Question answered**: *"What is the output for this input?"*

---

### L1 — Persistent Error Memory (PEM)

**Nature**: Specialized negative memory

**Content**:
- Failure events: `(context, action, outcome, severity)`
- Temporal decay for adaptability
- Severity-weighted sampling

**Role**: Prevent repetition of past failures

**Key Properties**:
- ✅ Explicit, external to model weights
- ✅ Selective (only failures above threshold)
- ✅ Contextual (remembers where, not just what)
- ✅ Decaying (adapts to non-stationary environments)

**Question answered**: *"Have I failed in a similar situation before?"*

**Reference**: [ICML 2026 Paper — Persistent Error Memory](https://github.com/ivan-berlocher/lifeos-error-memory-paper)

---

### L2 — KnowledgeOS

**Nature**: Declarative knowledge store

**Content**:
- Facts and concepts
- Documents and ontologies
- External sources (RAG)
- Structured relationships

**Role**: Provide factual grounding

**Key Properties**:
- ✅ World knowledge
- ✅ Queryable and retrievable
- ❌ No behavioral memory
- ❌ No awareness of system failures

**Question answered**: *"What does the world / the sources say about this?"*

**Integration**: Solid Pods, federated knowledge graphs, RAG pipelines

---

### L3 — MemoryOS

**Nature**: Integrative long-term memory

**Content**:
- Episodic memories (lived experiences)
- Narrative structure (temporal coherence)
- Decisions and their consequences
- Identity-forming traces

**Role**: Maintain continuity and identity

**Key Properties**:
- ✅ Autobiographical
- ✅ Temporal
- ✅ Integrative (can absorb PEM events)
- ✅ Identity-preserving

**Question answered**: *"What have I lived, decided, learned over time?"*

**Reference**: [02-memory-os.md](./02-memory-os.md)

---

### L4 — Agent / OPAR Loop

**Nature**: Decision and responsibility layer

**Components**:
- **O**bserve: Perceive environment (L0 + L2)
- **P**rocess: Reason and plan
- **A**ct: Execute decisions
- **R**eflect: Evaluate outcomes (L1 + L3)
- **G**row: Adapt and improve

**Role**: Be accountable

**Key Properties**:
- ✅ Responsibility exists here
- ✅ Decisions are made here
- ✅ Learning happens at system level
- ❌ Not in the model weights

**Question answered**: *"Who is accountable for this action?"*

**Reference**: [07-agents.md](./07-agents.md), [04-harmonia.md](./04-harmonia.md)

---

## 🔀 Information Flow

### Upward Flow (Perception → Decision)

```
L0 → L1: "This output was a failure"
L1 → L2: "Check knowledge for similar contexts"
L2 → L3: "Record this episode"
L3 → L4: "Inform decision with history"
```

### Downward Flow (Decision → Action)

```
L4 → L3: "What did we do last time?"
L3 → L2: "What facts are relevant?"
L2 → L1: "Any known failure modes here?"
L1 → L0: "Generate, avoiding these patterns"
```

---

## 🚫 Separation Rules (Non-Negotiable)

| ❌ Never Do | ✅ Always Do |
|-------------|--------------|
| Put PEM in model weights | Keep PEM external and explicit |
| Call PEM "knowledge" | PEM = failure memory only |
| Call PEM "MemoryOS" | MemoryOS is integrative, PEM is specialized |
| Say "the model remembers" | Say "the system remembers" |
| Mix layers | Maintain strict boundaries |

---

## 🧭 Mapping to Research Tracks

| Paper/Context | Layers Covered | Focus |
|---------------|----------------|-------|
| **ICML 2026** | L1 only | PEM as learning primitive |
| **NeurIPS 2026** | L1 → L4 | PEM in agent systems |
| **LifeOS** | L0 → L4 | Full orchestrated system |

---

## 🎯 Canonical Phrases

### Technical (for papers)

> *"Layered architecture of an error-aware agent system. Learning models (L0) operate without explicit memory. Persistent Error Memory (L1) introduces failure-specific retention. KnowledgeOS (L2) provides declarative knowledge. MemoryOS (L3) integrates long-term episodic experience. The agent loop (L4) orchestrates decision-making with responsibility."*

### Foundational (for keynotes)

> *"Models compute. Knowledge informs. Memory remembers. Error memory prevents repetition. Agents are responsible."*

### Essence (one line)

> *"Intelligence without memory repeats mistakes; LifeOS is built to remember."*

---

## 🔗 Relationship to Other Specs

- **[01-kernel.md](./01-kernel.md)**: The kernel law `Presence × Memory × Action` operates at L4
- **[02-memory-os.md](./02-memory-os.md)**: Detailed specification of L3
- **[03-sense-making.md](./03-sense-making.md)**: Interpretation happens across L2-L4
- **[04-harmonia.md](./04-harmonia.md)**: Orchestration at L4
- **[07-agents.md](./07-agents.md)**: Agent behavior at L4
- **[08-llat.md](./08-llat.md)**: Tracing operates across all layers

---

## 📊 Comparison: PEM vs RLHF vs RAG

| Dimension | PEM | RLHF | RAG |
|-----------|-----|------|-----|
| **Where it lives** | External memory | Model weights | Database |
| **Type** | Failure / negative | Preference / norm | Fact / info |
| **Temporality** | Persistent, lived | Frozen after training | Instantaneous |
| **Key question** | "Did I fail here before?" | "What do humans prefer?" | "What do sources say?" |
| **Corrects** | Error repetition | Style / alignment | Hallucinations |
| **Amnesic?** | ❌ No | ✅ Yes | ❌ No (but neutral) |

**Key insight**: These three are complementary, not competing.

- **RLHF** aligns the model to norms
- **RAG** connects the model to the world  
- **PEM** gives the system memory of its own failures

---

## 📅 Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2025-12-28 | Initial canonical architecture |

---

*This document is part of the [LifeOS Specification](../README.md).*
