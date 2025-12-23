# 00 — LifeOS Overview

> **High-level architecture and design rationale**

**Version**: 0.1  
**Status**: Draft  
**Last Updated**: 2025-12-23

---

## 1. What is LifeOS?

LifeOS is a **specification** for building AI-native personal operating systems that enhance human cognition while preserving human sovereignty.

### 1.1 It is NOT:
- A product or application
- A company or startup
- A single implementation
- A replacement for human thinking

### 1.2 It IS:
- A formal specification (like HTTP, WebSocket, or Solid)
- A philosophy made computable
- A blueprint for cognitive infrastructure
- An invitation to build

---

## 2. Design Goals

| Goal | Rationale |
|------|-----------|
| **Human Sovereignty** | The human remains in control at all times |
| **Distributed Architecture** | No single point of failure or control |
| **Interoperability** | Works with existing standards (Solid, etc.) |
| **Explainability** | Every decision is traceable |
| **Graceful Degradation** | Works offline, works without AI |

---

## 3. Core Components

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│                              LIFEOS STACK                                   │
│                                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   LAYER 4: FEDERATION                                                       │
│   ─────────────────────                                                     │
│   • Regional clusters                                                       │
│   • P2P protocols                                                           │
│   • Cross-cluster communication                                             │
│   [See: 06-federation.md]                                                   │
│                                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   LAYER 3: ORCHESTRATION (Harmonia)                                         │
│   ───────────────────────────────────                                       │
│   • Intent detection                                                        │
│   • Agent coordination                                                      │
│   • Output composition                                                      │
│   [See: 04-harmonia.md, 07-agents.md]                                       │
│                                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   LAYER 2: COGNITION                                                        │
│   ────────────────────                                                      │
│   • SenseMaking engine                                                      │
│   • Memory retrieval & activation                                           │
│   • Decision tracing                                                        │
│   [See: 03-sense-making.md, 02-memory-os.md]                                │
│                                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   LAYER 1: KERNEL                                                           │
│   ───────────────────                                                       │
│   • Presence Loop (sovereign authority)                                     │
│   • MemoryOS (data layer)                                                   │
│   • ActionOS (execution layer)                                              │
│   [See: 01-kernel.md]                                                       │
│                                                                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   LAYER 0: STORAGE (Solid-compatible Pods)                                  │
│   ──────────────────────────────────────────                                │
│   • User data ownership                                                     │
│   • Access control                                                          │
│   • Portability                                                             │
│   [See: 05-solid-bridge.md]                                                 │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Information Flow

```
                    ┌─────────────────────────────────┐
                    │           👤 USER               │
                    │   (voice, text, gesture, etc.)  │
                    └───────────────┬─────────────────┘
                                    │
                                    ▼
                    ┌─────────────────────────────────┐
                    │         INPUT LAYER             │
                    │   Multimodal signal capture     │
                    └───────────────┬─────────────────┘
                                    │
                                    ▼
                    ┌─────────────────────────────────┐
                    │       PRESENCE LOOP             │
                    │   "Is the human stable?"        │
                    │   ✓ Stable → Continue           │
                    │   ✗ Unstable → Pause/Warn       │
                    └───────────────┬─────────────────┘
                                    │
                                    ▼
                    ┌─────────────────────────────────┐
                    │       SENSE-MAKING              │
                    │   SCAN → NOTICE → INTERPRET     │
                    │   → DECIDE → ENACT              │
                    └───────────────┬─────────────────┘
                                    │
                                    ▼
                    ┌─────────────────────────────────┐
                    │       HARMONIA                  │
                    │   Intent → Route → Compose      │
                    └───────────────┬─────────────────┘
                                    │
                                    ▼
                    ┌─────────────────────────────────┐
                    │       DOUBLE LOCK               │
                    │   Lock 1: "What is this?"       │
                    │   Lock 2: "Execute?"            │
                    └───────────────┬─────────────────┘
                                    │
                                    ▼
                    ┌─────────────────────────────────┐
                    │       ACTION OS                 │
                    │   Execute authorized actions    │
                    └───────────────┬─────────────────┘
                                    │
                                    ▼
                    ┌─────────────────────────────────┐
                    │         REALITY                 │
                    │   (calendar, email, files...)   │
                    └─────────────────────────────────┘
```

---

## 5. Key Differentiators

| Traditional AI | LifeOS |
|----------------|--------|
| AI decides, human reviews | Human decides, AI assists |
| Data centralized | Data in user pods |
| Platform lock-in | Portable across implementations |
| Black box | Explainable decisions |
| Always-on surveillance | Intentional memory |
| Global infrastructure | Regional clusters |

---

## 6. Compatibility Matrix

| Standard | Status | Notes |
|----------|--------|-------|
| Solid Protocol | ✅ Compatible | Primary data layer |
| WebSocket | ✅ Compatible | Real-time communication |
| JSON-LD | ✅ Compatible | Linked data format |
| WebAuthn | 🔄 Planned | Authentication |
| ActivityPub | 🔄 Planned | Federation protocol |

---

## 7. Reading Order

For new readers:

1. **This document** (overview)
2. [PHILOSOPHY.md](../PHILOSOPHY.md) — The 10 commandments
3. [01-kernel.md](./01-kernel.md) — Core architecture law
4. [03-sense-making.md](./03-sense-making.md) — The cognitive model
5. Remaining specs as needed

---

## 8. Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2025-12-23 | Initial draft |

---

*Next: [01-kernel.md](./01-kernel.md)*
