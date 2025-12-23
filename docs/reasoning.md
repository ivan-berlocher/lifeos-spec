# Reasoning in LifeOS

> **"LifeOS does not conflate logic and intelligence."**

This document defines the three distinct reasoning modalities in LifeOS and their architectural separation.

---

## Overview

LifeOS explicitly separates three types of reasoning, each with different computational properties, trust requirements, and explainability characteristics:

| Modality | Nature | Trust Model | Explainability |
|----------|--------|-------------|----------------|
| **Deductive** | Deterministic | Verifiable | Complete |
| **Interpretive** | Probabilistic | Attributable | Partial |
| **Intentional** | Human-anchored | Sovereign | Self-evident |

---

## 1. Deductive Reasoning

### Definition

Deductive reasoning operates on **explicit facts, relationships, and rules**. Given the same inputs, it always produces the same outputs.

### Characteristics

- **Deterministic**: No randomness, no hallucination
- **Symbolic**: Operates on structured data (RDF, graphs, ontologies)
- **Lightweight**: Runs on any hardware, no GPU required
- **Verifiable**: Every conclusion can be traced to premises
- **W3C/Solid-aligned**: Uses standard web semantics

### Examples in LifeOS

```
IF presence.fatigue > 0.7 AND time.hour > 21
THEN recommendation.defer_external_actions = true
```

```
IF memory.pattern("late_night_emails").confidence > 0.8
AND current_action.type = "send_email"
THEN flag.requires_review = true
```

### Implementation Layer

- Memory queries (STM/MTM/LTM retrieval)
- Rule evaluation in Harmonia
- Access control in Solid pods
- Double Lock gate verification

---

## 2. Interpretive Reasoning

### Definition

Interpretive reasoning generates **meaning, summaries, and perceptual understanding** from unstructured or ambiguous inputs. It is inherently probabilistic.

### Characteristics

- **Probabilistic**: Outputs vary, confidence scores required
- **Multimodal**: Can process text, audio, images, sensor data
- **Compute-intensive**: Benefits from GPU/NPU acceleration
- **Attributable**: Source models and inputs must be logged
- **Selective**: Invoked only when deduction is insufficient

### Examples in LifeOS

```yaml
interpretive_task:
  input: "User's voice note from this morning"
  output:
    summary: "Expressed concern about project deadline"
    emotional_tone: "anxious but determined"
    confidence: 0.78
    model_used: "local-whisper-v3"
```

```yaml
interpretive_task:
  input: "Three days of activity patterns"
  output:
    insight: "Working hours extending, breaks decreasing"
    pattern_type: "burnout_early_warning"
    confidence: 0.65
```

### Implementation Layer

- SenseMaking frame generation
- Presence state inference
- Memory consolidation significance scoring
- Natural language understanding

### Trust Boundary

Interpretive outputs are **never directly actionable**. They must pass through:

1. SenseMaking synthesis (multiple hypotheses)
2. Harmonia deliberation (alignment scoring)
3. Double Lock verification (human confirmation for external actions)

---

## 3. Intentional Reasoning

### Definition

Intentional reasoning concerns **why now, for whom, and toward what end**. It is anchored in human presence and cannot be fully automated.

### Characteristics

- **Human-anchored**: Requires active presence confirmation
- **Contextual**: Same action may be right or wrong depending on intent
- **Sovereign**: Only the human can ultimately validate intent
- **Self-evident**: The human knows their own intention (even if imperfectly)

### Examples in LifeOS

```yaml
intentional_question:
  situation: "User wants to send late-night email"
  deductive_input: "Fatigue high, pattern suggests regret likely"
  interpretive_input: "Emotional tone suggests anxiety-driven"
  
  intentional_probe:
    question: "Is this truly urgent, or is it the part of you that can't stop?"
    options:
      - "Send now (I've decided it's time-sensitive)"
      - "Schedule for tomorrow (let me review when fresh)"
      - "Remind me tomorrow (I'll decide then)"
```

### Implementation Layer

- Presence Loop validation
- Harmonia option presentation
- Double Lock human confirmation
- Final action authorization

### The Irreducible Human Element

Intentional reasoning cannot be replaced by prediction:

- A model can predict what the user *will likely do*
- Only the user can decide what they *should do*

LifeOS preserves this gap by design.

---

## Architectural Separation

### Why Separation Matters

| Problem | Without Separation | With Separation |
|---------|-------------------|-----------------|
| Hallucination | Spreads to decisions | Contained in interpretive layer |
| Unexplainability | Entire system opaque | Deductive core remains transparent |
| Sovereignty loss | AI decides intent | Human retains final authority |
| Compute dependency | Everything needs GPU | Core runs anywhere |

### Data Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                        INTENTIONAL                              │
│                    (Human-anchored)                             │
│         "Is this what I actually want to do?"                   │
│                           │                                     │
│                           ▼                                     │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    INTERPRETIVE                          │   │
│  │                   (Probabilistic)                        │   │
│  │    "What does this mean? What pattern is emerging?"      │   │
│  │                           │                              │   │
│  │                           ▼                              │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │                  DEDUCTIVE                       │    │   │
│  │  │                (Deterministic)                   │    │   │
│  │  │     "Given these facts, what follows?"           │    │   │
│  │  │                                                  │    │   │
│  │  │   Facts → Rules → Conclusions                    │    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### Trust Propagation

- **Deductive outputs**: Fully trusted (verifiable)
- **Interpretive outputs**: Conditionally trusted (attributable, bounded confidence)
- **Intentional validation**: Sovereign (only human can confirm)

---

## Implications for Implementation

### What Runs Where

| Reasoning Type | Compute Requirement | Deployment |
|---------------|---------------------|------------|
| Deductive | Minimal (CPU) | Edge, Pod, anywhere |
| Interpretive | Variable (GPU optional) | Local or selective cloud |
| Intentional | Human attention | User interface |

### Explainability Guarantees

| Layer | Audit Question | Answer Source |
|-------|---------------|---------------|
| Deductive | "Why this conclusion?" | Rule trace |
| Interpretive | "Where did this insight come from?" | Model + input attribution |
| Intentional | "Who decided this?" | Presence + consent log |

---

## Summary

LifeOS reasoning is not a monolithic "AI brain." It is a **layered architecture** where:

1. **Deduction** provides the verifiable foundation
2. **Interpretation** augments with probabilistic meaning
3. **Intention** remains with the human

This separation is not a limitation — it is the **source of sovereignty**.

```
"Logic grounds decisions. Intelligence augments meaning. Intent remains human."
```

---

## References

- [01-kernel.md](../spec/01-kernel.md) — Presence × Memory × Action
- [03-sense-making.md](../spec/03-sense-making.md) — Interpretive synthesis
- [04-harmonia.md](../spec/04-harmonia.md) — Orchestration and Double Lock
- [README.md](../README.md) — Architectural Vision

---

**Document version:** 0.1  
**Last updated:** December 2025
