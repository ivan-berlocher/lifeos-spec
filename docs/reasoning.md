# Reasoning in LifeOS

> **"LifeOS does not conflate logic and intelligence."**

---

## Scope

### What LifeOS Reasons About

- **Structured facts**: Your calendar, contacts, notes, patterns
- **Temporal context**: What's happening now vs. what happened before
- **Behavioral patterns**: Recurring cycles, anomalies, trends
- **Alignment**: Does this action match your stated values/goals?

### What LifeOS Does NOT Reason About

- **Your true desires** (it can probe, not decide)
- **Moral judgments** (it surfaces tensions, doesn't resolve them)
- **External world modeling** (no autonomous web scraping, no speculation)
- **Other people's intentions** (respects epistemic boundaries)

---

## Reasoning Layers

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

### L0: Facts & Graph (Solid/RDF)

The ground truth layer. Queryable, ownable, portable.

| Property | Value |
|----------|-------|
| Storage | Solid pod (user-controlled) |
| Format | RDF triples, JSON-LD |
| Trust | Verifiable (cryptographic proofs possible) |
| Cost | Storage only, near-zero compute |

### L1: Deductive Rules

Cheap, deterministic, fully explainable. **Default layer for all decisions.**

```
IF presence.fatigue > 0.7 AND time.hour > 21
THEN flag.defer_external_actions = true
```

| Property | Value |
|----------|-------|
| Compute | Minimal (any CPU) |
| Explainability | Complete (rule trace) |
| Latency | Milliseconds |
| Trust | Full (same inputs → same outputs) |

### L2: Contextual Interpretation

Invoked **only when L1 is insufficient**. Probabilistic, attributable, bounded.

| Property | Value |
|----------|-------|
| Compute | Variable (GPU optional, local preferred) |
| Explainability | Partial (model + input attribution) |
| Latency | Seconds to minutes |
| Trust | Conditional (confidence scores required) |

**Key constraint**: L2 outputs are **never directly actionable**. They feed back into L1 for rule evaluation or escalate to L3 for human validation.

### L3: Intentional Gating

The irreducible human element. Only presence can authorize.

| Property | Value |
|----------|-------|
| Compute | Human attention (scarce resource) |
| Explainability | Self-evident (the human knows their intent) |
| Latency | Variable (depends on human availability) |
| Trust | Sovereign (only the user can validate) |

**Double Lock**: External actions MUST pass through L3.

---

## Cost Model

### Why LifeOS Is Cheap by Default

| Operation | Layer | Cost |
|-----------|-------|------|
| Memory query | L0 | ~0 (storage lookup) |
| Rule evaluation | L1 | ~0 (CPU milliseconds) |
| Pattern detection | L1 | ~0 (graph traversal) |
| Text summarization | L2 | $$ (LLM call) |
| Voice transcription | L2 | $ (Whisper local) |
| Human confirmation | L3 | Attention (priceless) |

**Design principle**: Stay in L0/L1 as long as possible. Escalate to L2 only when necessary. Reserve L3 for external actions and ambiguous situations.

### Typical Flow

```
95% of operations:  L0 → L1 → Done        (free)
4% of operations:   L0 → L1 → L2 → L1     (local compute)  
1% of operations:   L0 → L1 → L2 → L3     (human attention)
```

---

## Explainability Guarantees

Every LifeOS decision can answer these questions:

| Question | Layer | Answer Source |
|----------|-------|---------------|
| **What** facts were used? | L0 | RDF graph query log |
| **Why** this conclusion? | L1 | Rule trace (premises → conclusion) |
| **Who** interpreted this? | L2 | Model ID + input hash |
| **When** was this decided? | L3 | Presence timestamp + consent log |

### The Explainable Web (Not XAI)

LifeOS does not try to "explain" neural networks. Instead:

- L0/L1 are **natively explainable** (symbolic, deterministic)
- L2 is **attributable** (logged inputs, model versions, confidence)
- L3 is **self-evident** (the human was present and consented)

This is **structural transparency**, not post-hoc explanation.

---

## Summary

| Layer | Nature | Cost | Trust |
|-------|--------|------|-------|
| L0 | Facts | Free | Verifiable |
| L1 | Rules | Free | Deterministic |
| L2 | Interpretation | Variable | Attributable |
| L3 | Intention | Attention | Sovereign |

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

**Document version:** 0.2  
**Last updated:** December 2025
