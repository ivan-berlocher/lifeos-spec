# PhD Proposal

## Responsible Cognitive Architectures

**Architectures for Memory, Reasoning, and Responsible Intelligent Systems**

---

**Candidate**: Ivan Berlocher  
**Date**: December 2025  
**Target Institutions**: ETH Zürich, University of Southampton, Max Planck Institute  
**Format**: Position-driven PhD Proposal

---

## Abstract

Current AI systems optimize for immediate task performance while neglecting fundamental architectural requirements for responsible long-term operation: persistent memory, traceable reasoning, and accountability by design. This thesis proposes **Responsible Cognitive Architectures** as a necessary foundation for intelligent systems capable of coherent action across time, explainable decision-making, and alignment with human sovereignty. The research is grounded in LifeOS, an experimental cognitive architecture that demonstrates these principles in a working system.

---

## 1. Problem Statement

### 1.1 The Architectural Gap

Modern AI achieves remarkable performance on isolated tasks, yet fails systematically when deployed as persistent agents operating over extended time horizons. The failures are not algorithmic—they are **architectural**.

Three critical capabilities are missing from current systems:

**Memory as Infrastructure**. Large language models process each interaction without genuine memory. Context windows simulate continuity but provide no persistence, no versioning, no selective forgetting. An AI assistant today cannot recall what it learned from you last year, cannot recognize patterns across your lifetime of interactions, cannot maintain a coherent model of who you are. This is not a parameter count problem. It is an architecture problem.

**Reasoning as Process**. Current systems produce outputs that appear reasoned but lack explicit reasoning traces. When a model recommends an action, there is no inspectable chain from inputs through inference to conclusion. The recommendation may be correct, but its correctness cannot be verified, its logic cannot be audited, its assumptions cannot be challenged. Black-box optimization has replaced accountable deliberation.

**Responsibility as Property**. AI systems today are not designed to be responsible. They cannot explain their decisions over time. They cannot maintain consistent identity across sessions. They cannot be held accountable because accountability requires traceability, and traceability requires architecture—not just capability. When an autonomous agent takes an action that causes harm six months after training, who or what is responsible? Current architectures provide no answer because they were never designed to.

### 1.2 The Stakes

These are not theoretical concerns. As AI systems gain agency—executing code, managing schedules, making purchases, coordinating with other agents—the absence of memory, reasoning, and responsibility becomes dangerous.

An AI financial advisor that cannot remember your risk tolerance.  
An AI healthcare assistant that cannot trace why it recommended a treatment.  
An AI personal assistant that cannot explain why it shared your calendar with a stranger.

The common thread: **systems that act without the architectural capacity for accountability**.

### 1.3 The Research Question

> **How should intelligent systems be architected to maintain coherent memory, explicit reasoning, and verifiable responsibility across extended operation?**

This is not a question about making models larger, training longer, or fine-tuning better. It is a question about **what structures must exist** for responsible AI operation to be possible at all.

---

## 2. Thesis Statement

> **Responsible Cognitive Architectures are necessary to build intelligent systems capable of coherent long-term action and explanation.**

The thesis makes three claims:

1. **Necessity**: Memory, reasoning, and responsibility cannot be retrofitted onto existing architectures. They must be designed in from the foundation.

2. **Sufficiency**: A properly structured cognitive architecture—with memory as a first-class component, reasoning as an explicit process, and closed observe-decide-act-reflect loops—provides the minimal viable substrate for responsible AI.

3. **Demonstrability**: These principles can be implemented and evaluated in real systems, not merely theorized.

The thesis does not claim that Responsible Cognitive Architectures solve AI alignment, guarantee safety, or eliminate all risks. It claims that they are **prerequisite**—necessary conditions without which responsible AI is architecturally impossible.

---

## 3. Architectural Principles

### 3.1 Memory as a First-Class Component

Memory in Responsible Cognitive Architectures is not logging, caching, or context management. It is a **sovereign subsystem** with its own invariants:

**Persistence**: Memory survives system restarts, updates, and migrations. What the system knows must not depend on which instance is running.

**Intentionality**: Memory is curated, not accumulated. The system forgets by design. What remains must be explicitly valued, not merely retained by default.

**Versioning**: Memory changes over time, and these changes must be traceable. The system must be able to answer: "What did I believe about X at time T?"

**Governance**: Memory has access controls. Not all components should access all memories. Not all agents should modify all records.

The architectural pattern: **MemoryOS**—a dedicated layer that mediates all memory operations, enforces invariants, and provides a unified interface to the rest of the system.

**Research contribution**: Formalizing memory invariants and demonstrating their enforcement in a working system. Developing the **Error Memory** extension: a specialized memory subsystem that tracks errors, enabling the system to avoid repeating mistakes (see companion paper: "Error-Aware Learning with Persistent Memory").

### 3.2 Reasoning as an Explicit Process

Reasoning in Responsible Cognitive Architectures is not emergent behavior from large models. It is a **structured process** with visible steps:

**Trace Production**: Every decision produces a trace—an inspectable record of what inputs were considered, what inferences were made, what conclusions were drawn.

**Causality**: Traces encode causal relationships, not just correlations. The system must distinguish "X happened before Y" from "X caused Y."

**Verifiability**: Third parties can audit traces without accessing the underlying model weights or training data.

The architectural pattern: **LLAT (LLM-Aligned Audit Traces)**—a trace format that captures the observe-decide-act-explain loop in a machine-readable, human-interpretable structure.

**Research contribution**: Formalizing trace schemas, developing validation criteria for trace consistency, and demonstrating trace-based debugging and explanation (see companion paper: "LLAT: LLM-Aligned Audit Traces for Cognitive Architectures").

### 3.3 Responsibility as an Architectural Property

Responsibility in Responsible Cognitive Architectures is not an add-on feature. It is a **structural property** that emerges from the combination of memory and reasoning:

**Traceability**: Every action can be traced to its origin—what triggered it, what justified it, what authorized it.

**Identity Persistence**: The system maintains consistent identity over time. Actions taken today can be connected to decisions made months ago.

**Accountability Surfaces**: Clear interfaces exist for auditing system behavior. External observers can verify claims about past actions.

The architectural pattern: **Closed ODAR Loops (Observe-Decide-Act-Reflect)**—a mandatory control structure where every action cycle includes reflection and recording.

```
┌─────────────┐
│   OBSERVE   │ ← Perceive environment, retrieve relevant memories
└─────┬───────┘
      │
      ▼
┌─────────────┐
│   DECIDE    │ ← Reason explicitly, produce trace
└─────┬───────┘
      │
      ▼
┌─────────────┐
│    ACT      │ ← Execute action, record outcome
└─────┬───────┘
      │
      ▼
┌─────────────┐
│  REFLECT    │ ← Evaluate result, update memory, close loop
└─────────────┘
```

**Research contribution**: Formalizing the ODAR loop structure, defining mandatory vs. optional reflections, and demonstrating how closed loops enable post-hoc explanation.

### 3.4 Sovereignty Preservation

Responsible Cognitive Architectures operate in service of human principals, not as autonomous maximizers:

**Presence Verification**: No action without stable human presence. The system verifies cognitive stability before executing consequential actions.

**Double-Lock Authorization**: AI proposes, human decides—twice. Classification of intent, then authorization of action.

**Graceful Degradation**: The system remains useful when components fail, when connectivity is lost, when AI is unavailable.

The architectural pattern: **Presence Loop**—a continuous monitoring subsystem that gates action execution on human readiness.

**Research contribution**: Formalizing presence states, developing presence verification protocols, and demonstrating sovereignty-preserving agent coordination.

---

## 4. System Artefact: LifeOS

### 4.1 Overview

LifeOS is an experimental cognitive architecture that implements Responsible Cognitive Architecture principles. It serves as:

- **Proof of concept**: Demonstrating that the principles are implementable
- **Research platform**: Enabling experiments on memory, reasoning, and responsibility
- **Living specification**: Evolving with research findings

### 4.2 Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              LIFEOS STACK                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│   LAYER 4: FEDERATION                                                       │
│   • Regional clusters • P2P protocols • Cross-cluster communication         │
├─────────────────────────────────────────────────────────────────────────────┤
│   LAYER 3: ORCHESTRATION (Harmonia)                                         │
│   • Intent detection • Agent coordination • Output composition              │
├─────────────────────────────────────────────────────────────────────────────┤
│   LAYER 2: COGNITION                                                        │
│   • SenseMaking engine • Memory retrieval • Decision tracing                │
├─────────────────────────────────────────────────────────────────────────────┤
│   LAYER 1: KERNEL                                                           │
│   • Presence Loop • MemoryOS • ActionOS                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│   LAYER 0: STORAGE (Solid-compatible Pods)                                  │
│   • User data ownership • Access control • Portability                      │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 4.3 Key Components

**MemoryOS**: The memory substrate implementing persistence, intentionality, versioning, and governance. Includes the ErrorMemory subsystem for error-aware learning.

**SenseMaking Engine**: The reasoning substrate producing LLAT-format traces for all decisions.

**Presence Loop**: The sovereignty substrate verifying human presence before consequential actions.

**Harmonia**: The orchestration layer coordinating multiple agents while preserving human control.

### 4.4 Current Status

- Specification: Published (v0.2)
- Core implementation: TypeScript/Python
- LLAT trace format: Defined and validated
- ErrorMemory module: Implemented with experimental results
- Deployment: Development instances operational

---

## 5. Evaluation Strategy

### 5.1 Non-Benchmark Evaluation

Responsible Cognitive Architectures cannot be evaluated by standard ML benchmarks. Accuracy on test sets does not measure memory coherence, reasoning traceability, or responsibility.

The evaluation framework uses three primary criteria:

### 5.2 Traceability of Decisions

**Definition**: The system can produce, for any past action, a complete causal trace from environmental input through reasoning to action output.

**Measurement**:
- Trace completeness: % of actions with full traces
- Trace consistency: % of traces that validate against schema
- Trace usefulness: Human evaluator accuracy in predicting system behavior from traces

**Experiment Design**: 
1. Deploy LifeOS instance over extended period (3+ months)
2. Sample random actions from history
3. Request trace reconstruction
4. Evaluate traces for completeness and consistency
5. Present traces to evaluators, measure prediction accuracy

### 5.3 Persistence of Identity

**Definition**: The system maintains consistent self-model over time, recognizing itself across sessions and contexts.

**Measurement**:
- Memory continuity: % of intentionally-retained memories accessible after N restarts
- Self-reference consistency: Agreement between system self-descriptions at different times
- Cross-session coherence: Alignment of decisions across related situations over time

**Experiment Design**:
1. Establish baseline identity markers (goals, preferences, constraints)
2. Operate system through multiple restart cycles
3. Query identity markers at intervals
4. Measure drift, consistency, and recovery

### 5.4 Ability to Explain Actions Over Time

**Definition**: The system can provide coherent explanations for past actions that reference the state of knowledge and reasoning at the time of action.

**Measurement**:
- Temporal explanation accuracy: Do explanations correctly reference past state?
- Explanation completeness: Do explanations cover all relevant factors?
- Explanation utility: Do explanations help humans understand and predict system behavior?

**Experiment Design**:
1. Log all actions with timestamps
2. At random intervals, request explanation of past action
3. Verify explanation against logged state at action time
4. Evaluate with human judges for completeness and utility

### 5.5 Comparative Analysis

Where appropriate, compare LifeOS against:
- Standard LLM assistants (no persistent memory)
- RAG-augmented systems (retrieval without governance)
- Agent frameworks (action without reflection)

The comparison is not accuracy-based but capability-based: Can the system do X at all? How well? At what cost?

---

## 6. Research Timeline

### Year 1: Foundations
- Formalize memory invariants (Q1-Q2)
- Implement and validate MemoryOS (Q2-Q3)
- Develop LLAT trace schema (Q3-Q4)
- **Publication target**: LLAT paper (workshop or conference)

### Year 2: Integration
- Implement full ODAR loop (Q1-Q2)
- Develop ErrorMemory extension (Q2-Q3)
- Build evaluation framework (Q3-Q4)
- **Publication target**: Error Memory paper (ICML/NeurIPS)

### Year 3: Evaluation
- Long-term deployment experiments (Q1-Q3)
- Comparative analysis (Q2-Q3)
- Thesis writing (Q3-Q4)
- **Publication target**: Architecture paper (journal), thesis

---

## 7. Expected Contributions

### 7.1 Theoretical
- Formal definition of Responsible Cognitive Architecture
- Invariant specifications for memory, reasoning, and responsibility
- Non-benchmark evaluation framework for cognitive architectures

### 7.2 Technical
- LifeOS reference implementation
- LLAT trace format and validation tools
- ErrorMemory module and integration patterns

### 7.3 Practical
- Design guidelines for responsible AI systems
- Evaluation protocols for long-term AI operation
- Open-source components for the research community

---

## 8. Why This Matters

The next generation of AI systems will have memory. They will have agency. They will operate over months and years, not minutes.

If we do not establish architectural foundations for responsibility now, we will build systems that cannot be held accountable, cannot explain themselves, and cannot be trusted.

This is not about making AI safe. Safety is a property of systems in environments. This is about making AI **accountable**—giving it the architectural capacity to answer for its actions.

Responsible Cognitive Architectures are not the only answer. But they are, I argue, a necessary part of any answer.

The question is not whether we will build persistent AI agents. We will. The question is whether those agents will be architecturally capable of responsibility.

This thesis aims to make that possible.

---

## References

1. LifeOS Specification v0.2 (2025). https://github.com/user/lifeos-spec
2. LLAT: LLM-Aligned Audit Traces for Cognitive Architectures (2025). Working paper.
3. Error-Aware Learning with Persistent Memory (2025). Working paper.
4. Solid Protocol Specification. https://solidproject.org
5. Berners-Lee, T. (2009). Socially Aware Cloud Storage.
6. Russell, S. (2019). Human Compatible: AI and the Problem of Control.
7. Amodei, D. et al. (2016). Concrete Problems in AI Safety.
8. Langley, P. (2017). Progress and Challenges in Research on Cognitive Architectures.
9. Laird, J. (2012). The Soar Cognitive Architecture.
10. Anderson, J. R. (2007). How Can the Human Mind Occur in the Physical Universe?

---

## Appendix A: Alignment with Existing Programs

### ETH Zürich — Computer Science
- **Fit**: Systems research, formal methods, responsible AI
- **Potential supervisors**: Security/Privacy group, Systems group
- **Angle**: Formal verification of cognitive architecture properties

### University of Southampton — Web Science
- **Fit**: Solid Protocol origin, distributed systems, human-computer interaction
- **Potential supervisors**: Web and Internet Science group
- **Angle**: Decentralized cognitive architectures, data sovereignty

### Max Planck Institute — Intelligent Systems
- **Fit**: Cognitive systems, machine learning theory
- **Potential supervisors**: Empirical Inference, Autonomous Learning
- **Angle**: Theoretical foundations of memory and learning in cognitive architectures

---

## Appendix B: Candidate Background

- 15+ years software engineering experience
- Expertise in distributed systems, AI infrastructure, cognitive architectures
- Creator of LifeOS specification and implementation
- Publications in progress: LLAT paper, Error Memory paper
- Open-source contributions: LifeOS, LLAT reference implementation

---

*"The specification exists to serve humans, not the reverse."*
— LifeOS Philosophy, Commandment X
