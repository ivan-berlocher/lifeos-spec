# LifeOS: An Explainable Web Architecture for Human-Centric Systems

**Technical Note v0.2**

**Author:** Ivan Berlocher  
**Date:** December 2025  
**DOI:** 10.5281/zenodo.18028496  
**Repository:** https://github.com/ivan-berlocher/lifeos-spec  
**License:** MIT

---

## Abstract

We present LifeOS, a human-centric system architecture grounded in Web standards and designed for explainability by construction. LifeOS explicitly separates symbolic reasoning from perceptual and interpretative intelligence, enabling decisions that remain inspectable, attributable, and context-aware across layers. Rather than focusing on Explainable AI models, this work frames an **Explainable Web** perspective, where facts, relations, and decisions can be audited in terms of *what* was decided, *why*, *by whom*, and *when*.

**Keywords:** explainable web, human sovereignty, symbolic reasoning, Solid Protocol, cognitive architecture, auditable systems

---

## 1. Motivation: Explainable Web ≠ XAI

### 1.1 The Problem

Current approaches to AI explainability focus on making neural networks interpretable *after the fact*. This creates a fundamental mismatch:

| XAI Approach | Limitation |
|--------------|------------|
| Attention visualization | Shows correlation, not causation |
| Feature importance | Model-specific, not transferable |
| Saliency maps | Fragile to perturbation |
| Post-hoc rationalization | May not reflect actual decision process |

The deeper problem: **these techniques try to explain opaque systems rather than building transparent ones.**

### 1.2 The Explainable Web Alternative

LifeOS takes a different approach: design for explainability from the ground up.

| Principle | Implementation |
|-----------|----------------|
| **Facts are structured** | RDF/Linked Data on Solid pods |
| **Rules are explicit** | Symbolic deduction, deterministic |
| **Interpretation is bounded** | Logged, versioned, confidence-scored |
| **Decisions are attributed** | Presence timestamp + consent record |

This is not post-hoc explanation. It is **structural transparency**.

---

## 2. Architectural Principle

### 2.1 Separation: Symbolic vs. Perceptual

LifeOS explicitly separates two types of intelligence:

| Aspect | Symbolic (Deductive) | Perceptual (Neural) |
|--------|---------------------|---------------------|
| **Nature** | Deterministic | Probabilistic |
| **Compute** | CPU, milliseconds | GPU, variable |
| **Trust** | Verifiable | Attributable |
| **Explainability** | Complete | Partial |
| **Role in LifeOS** | Core reasoning | Augmentation only |

### 2.2 Why This Matters

```
Neural networks excel at perception.
Symbolic systems excel at reasoning.
LifeOS uses each where appropriate.
```

**Key constraint**: Perceptual outputs are never directly actionable. They must pass through symbolic verification before reaching the user.

---

## 3. Explainability by Design

### 3.1 The Four-Question Audit

Every LifeOS decision can answer:

| Question | Layer | Source |
|----------|-------|--------|
| **What** was decided? | L0/L1 | Action log with RDF provenance |
| **Why** this decision? | L1 | Rule trace (premises → conclusion) |
| **Who** decided? | L3 | Presence + consent timestamp |
| **When** was this decided? | L3 | Temporal context + role |

### 3.2 Auditability Across Layers

```
┌─────────────────────────────────────────────────────────────┐
│  AUDIT TRAIL                                                │
├─────────────────────────────────────────────────────────────┤
│  L3: Who confirmed? When? In what role?                     │
│  L2: Which model? What confidence? What inputs?             │
│  L1: Which rules fired? What was the deduction chain?       │
│  L0: What facts were queried? From which pod?               │
└─────────────────────────────────────────────────────────────┘
```

Every layer produces auditable output. No black boxes.

---

## 4. Reasoning Layers (L0–L3)

### 4.1 Architecture Overview

```
┌──────────────────────────────────────────────────────────────┐
│  L3: INTENTIONAL GATING                                      │
│      Human presence, consent, final authority                │
│      → "Is this what I actually want?"                       │
├──────────────────────────────────────────────────────────────┤
│  L2: CONTEXTUAL INTERPRETATION                               │
│      LLM/multimodal, probabilistic, expensive                │
│      → "What does this mean? What pattern is emerging?"      │
├──────────────────────────────────────────────────────────────┤
│  L1: DEDUCTIVE RULES                                         │
│      Symbolic, deterministic, cheap, explainable             │
│      → "Given these facts, what follows?"                    │
├──────────────────────────────────────────────────────────────┤
│  L0: FACTS & GRAPH                                           │
│      Solid pods, RDF triples, structured data                │
│      → "What do I actually know?"                            │
└──────────────────────────────────────────────────────────────┘
```

### 4.2 L0: Facts & Graph (Solid/RDF)

The ground truth layer. Queryable, ownable, portable.

- **Storage**: Solid pod (user-controlled)
- **Format**: RDF triples, JSON-LD
- **Trust**: Verifiable (cryptographic proofs possible)
- **Cost**: Storage only, near-zero compute

### 4.3 L1: Deductive Rules

Cheap, deterministic, fully explainable. **Default layer for all decisions.**

```
IF presence.fatigue > 0.7 AND time.hour > 21
THEN flag.defer_external_actions = true
```

- **Compute**: Minimal (any CPU)
- **Explainability**: Complete (rule trace)
- **Latency**: Milliseconds

### 4.4 L2: Contextual Interpretation

Invoked **only when L1 is insufficient**. Probabilistic, attributable, bounded.

- **Compute**: Variable (GPU optional, local preferred)
- **Explainability**: Partial (model + input attribution)
- **Constraint**: Outputs never directly actionable

### 4.5 L3: Intentional Gating

The irreducible human element. Only presence can authorize.

- **Compute**: Human attention (scarce resource)
- **Trust**: Sovereign (only the user can validate)
- **Mechanism**: Double Lock for external actions

---

## 5. Anonymized Examples

### 5.1 Example 1: Family Context

**Actors**: Person A (adult), Person B (adult), Person C (child), Person D (child)

**L0 — Facts**:
```turtle
:personA rel:spouseOf :personB .
:personA rel:parentOf :personC, :personD .
:personB rel:parentOf :personC, :personD .
```

**L1 — Deduction**:
- Rule: `shared parents ⇒ siblingOf`
- Derived: `personC siblingOf personD`

**L2 — Interpretation**:
> "The relationship between Person A and Person C is currently focused on academic support."

**L3 — Decision**:
- Role: Parent
- Context: Upcoming evaluation
- Action: "Surface a supportive memory related to Person C"

**Audit**: What (reminder), Why (parental role + context), Who (Person A), When (time-sensitive)

### 5.2 Example 2: Professional Context

**Actors**: Person E (author), Person F (reviewer), Project X

**L0 — Facts**:
```turtle
:personE schema:worksOn :projectX .
:personF schema:collaboratesWith :personE .
```

**L1 — Deduction**:
- Rule: `collaboratesWith ⇒ professional relationship`

**L2 — Interpretation**:
> "Person F acts as a critical semantic reference for Project X."

**L3 — Decision**:
- Role: Editor
- Context: Pre-publication phase
- Action: "Review this section using Person F's perspective"

**Audit**: What (review suggestion), Why (audience alignment), Who (Person E), When (pre-publication)

---

## 6. Cost & Scalability

### 6.1 Why Reasoning Is Cheap by Default

| Operation | Layer | Cost |
|-----------|-------|------|
| Memory query | L0 | ~0 (storage lookup) |
| Rule evaluation | L1 | ~0 (CPU milliseconds) |
| Pattern detection | L1 | ~0 (graph traversal) |
| Text summarization | L2 | $$ (LLM call) |
| Voice transcription | L2 | $ (Whisper local) |
| Human confirmation | L3 | Attention (priceless) |

### 6.2 Typical Flow Distribution

```
95% of operations:  L0 → L1 → Done        (free)
 4% of operations:  L0 → L1 → L2 → L1     (local compute)  
 1% of operations:  L0 → L1 → L2 → L3     (human attention)
```

**Design principle**: Stay in L0/L1 as long as possible. Escalate only when necessary.

---

## 7. Positioning & Scope

### 7.1 What LifeOS Is

- A **specification** for human-centric cognitive systems
- An **architecture** separating symbolic and perceptual intelligence
- A **framework** for explainability by construction
- **Solid Protocol compatible** (W3C standards)

### 7.2 What LifeOS Is Not

- Not a product or application
- Not a replacement for existing AI assistants
- Not an XAI technique (it's an alternative paradigm)
- Not dependent on specific AI models or vendors

### 7.3 Relation to Standards

| Standard | Relation |
|----------|----------|
| **Solid Protocol** | L0 storage layer |
| **RDF/Linked Data** | Fact representation |
| **W3C PROV** | Provenance tracking |
| **SHACL** | Rule validation |

---

## 8. Status & Next Steps

### 8.1 Current Status

- ✅ Specification complete (8 documents)
- ✅ Conceptual framework validated
- ✅ Examples documented
- 🔄 Reference implementation in progress

### 8.2 Invitation to Discuss

This work is intentionally published early to invite feedback from:

- The Solid community
- XAI researchers interested in alternative approaches
- Practitioners building human-centric systems

**Contact**: via GitHub issues or Solid forum

---

## References

1. Berners-Lee, T. et al. "Solid: A Platform for Decentralized Social Applications"
2. W3C. "RDF 1.1 Concepts and Abstract Syntax"
3. W3C. "Shapes Constraint Language (SHACL)"
4. Bechhofer, S. et al. "OWL Web Ontology Language"

---

## Citation

```bibtex
@misc{berlocher2025lifeos,
  author       = {Berlocher, Ivan},
  title        = {{LifeOS}: An Explainable Web Architecture for Human-Centric Systems},
  year         = {2025},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.18028496},
  url          = {https://doi.org/10.5281/zenodo.18028496}
}
```

---

**Document version:** 0.2  
**Last updated:** December 2025

*"Logic grounds decisions. Intelligence augments meaning. Intent remains human."*
