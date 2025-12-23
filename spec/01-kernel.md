# 01 — The LifeOS Kernel

> **Presence × Memory × Action × Agents**

**Version**: 0.1  
**Status**: Draft  
**Last Updated**: 2025-12-23

---

## 1. Definition

> **The LifeOS Kernel is a governance core that guarantees no memory, no action, no agent operates without stable human presence.**

LifeOS is not a technical OS.  
**It is an OS of responsibility.**

---

## 2. The Four Subsystems

| System | Role | Power |
|--------|------|-------|
| **Presence Loop** | Cognitive authority | ✅ Authorizes / Refuses |
| **MemoryOS** | Structured memory | ❌ Does not decide |
| **ActionOS** | Execution | ❌ Does not trigger |
| **Agent Orchestration** | Distributed compute | ❌ Does not command |

👉 **Only one system has power: the Presence Loop.**

---

## 3. Absolute Hierarchy (Non-Circular)

```
Presence Loop
    ↓ authorizes
MemoryOS
    ↓ feeds
Agent Orchestration
    ↓ proposes
ActionOS
    ↓ executes
Reality
```

⚠️ **No arrow goes back up without passing through Presence Loop.**

This makes LifeOS:
- ✅ Non-dangerous
- ✅ Non-manipulable  
- ✅ Non-autonomous by accident

---

## 4. Presence Loop — The Sovereign Authority

### 4.1 What Presence Loop DOES
- Observes cognitive states
- Detects stability
- Authorizes or blocks

### 4.2 What Presence Loop NEVER does
- ❌ Store
- ❌ Compute
- ❌ Execute
- ❌ Optimize

> *Presence does not work.*  
> *It decides when work is right.*

### 4.3 Cognitive States

```typescript
type CognitiveState = 
  | "STABLE"      // Clear mind, can decide
  | "FOCUSED"     // Deep work, minimize interrupts
  | "SCATTERED"   // Fragmented attention
  | "OVERLOADED"  // Too much input
  | "RECOVERING"  // Coming back from overload
  | "ABSENT"      // Not present (auto-lock)
```

### 4.4 State Transitions

```
           ┌──────────────────────────────────────────┐
           │                                          │
           ▼                                          │
    ┌─────────────┐                           ┌──────┴──────┐
    │   ABSENT    │ ──── presence detected ──►│   STABLE    │
    └─────────────┘                           └──────┬──────┘
           ▲                                         │
           │                                    focus trigger
    no presence                                      │
           │                                         ▼
    ┌──────┴──────┐                           ┌─────────────┐
    │  OVERLOADED │ ◄── too much input ────── │   FOCUSED   │
    └──────┬──────┘                           └─────────────┘
           │                                         ▲
           │ time passes                             │
           │                                    calm restored
           ▼                                         │
    ┌─────────────┐                           ┌──────┴──────┐
    │  RECOVERING │ ─────────────────────────►│  SCATTERED  │
    └─────────────┘                           └─────────────┘
```

---

## 5. MemoryOS — Conditional Memory

### 5.1 Fundamental Rule

> **No memory is written without Condensation + Dissolution.**

MemoryOS:
- Receives **signals**, not raw data
- Structures **after the fact**
- Accepts **forgetting** as central function

### 5.2 Prohibitions
- ❌ No exhaustive logging
- ❌ No "keep everything"
- ❌ No agent-side memory

### 5.3 Memory Types

| Type | Description | TTL |
|------|-------------|-----|
| `EPHEMERAL` | Working context | Minutes-Hours |
| `FACT` | Stable facts | Indefinite |
| `NARRATIVE` | Versioned interpretations | Review-based |
| `RELATIONAL` | Shared decisions | Until revoked |
| `REFLECTIVE` | Lessons learned | Indefinite |

*Full specification: [02-memory-os.md](./02-memory-os.md)*

---

## 6. ActionOS — Controlled Execution

### 6.1 Principle

> **ActionOS is an executor, not a decider.**

It receives authorized actions and executes them. Period.

### 6.2 Action Lifecycle

```
1. Proposal generated (by Agent/Harmonia)
         │
         ▼
2. LOCK 1: User classifies intent
         │
         ▼
3. LOCK 2: User authorizes execution
         │
         ▼
4. ActionOS executes
         │
         ▼
5. Result recorded + feedback to Presence Loop
```

### 6.3 Action Types

```typescript
type ActionType =
  | "NAVIGATE"    // Go somewhere (URL, app, space)
  | "CREATE"      // Create content (file, event, message)
  | "MODIFY"      // Change existing content
  | "DELETE"      // Remove content
  | "COMMUNICATE" // Send message (email, chat)
  | "SEARCH"      // Query external systems
  | "WAIT"        // Scheduled future action
```

---

## 7. Agent Orchestration — Intelligence Under Control

### 7.1 Agent Nature

Agents are:
- **Temporary** — Spawned for tasks, terminated after
- **Specialized** — One domain per agent
- **Bounded** — Limited permissions
- **Visible** — User can see all active agents
- **Revocable** — User can kill any agent instantly

### 7.2 Agent Prohibitions

- ❌ Cannot write to MemoryOS without proposal
- ❌ Cannot execute actions without double lock
- ❌ Cannot spawn other agents without authorization
- ❌ Cannot persist across sessions without consent

### 7.3 Agent Manifest

Every agent must declare:

```typescript
interface AgentManifest {
  id: string
  name: string
  domain: string              // "calendar", "email", "research"
  capabilities: string[]      // What it CAN do
  limitations: string[]       // What it CANNOT do
  dataAccess: {
    read: string[]            // What it can read
    write: string[]           // What it can write
  }
  ttl: number                 // Max lifetime in seconds
  killSwitch: true            // Always true (non-negotiable)
}
```

*Full specification: [07-agents.md](./07-agents.md)*

---

## 8. Inter-System Communication

```
┌───────────────────────────────────────────────────────────────────────────┐
│                                                                           │
│                        KERNEL MESSAGE BUS                                 │
│                                                                           │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐                │
│   │  Presence   │     │  MemoryOS   │     │  ActionOS   │                │
│   │    Loop     │     │             │     │             │                │
│   └──────┬──────┘     └──────┬──────┘     └──────┬──────┘                │
│          │                   │                   │                        │
│          │    state.change   │  memory.propose   │  action.request       │
│          │◄──────────────────│◄──────────────────│◄─────────────────     │
│          │                   │                   │                        │
│          │    state.query    │  memory.retrieve  │  action.authorized    │
│          │──────────────────►│──────────────────►│─────────────────►     │
│          │                   │                   │                        │
│   └──────┴──────┘     └──────┴──────┘     └──────┴──────┘                │
│                                                                           │
│                        Agent Orchestration                                │
│                    (listens to all, proposes only)                        │
│                                                                           │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## 9. Implementation Requirements

An implementation is Kernel-compliant if:

| Requirement | Verification |
|-------------|--------------|
| Presence Loop exists | State detection active |
| Double Lock implemented | Both locks required for any action |
| Memory has governance | Consent + TTL + versioning |
| Agents have manifests | All agents declared |
| Kill switch works | Agents terminable in <1s |
| No circular authority | Audit shows no bypasses |

---

## 10. Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2025-12-23 | Initial draft |

---

*Previous: [00-overview.md](./00-overview.md)*  
*Next: [02-memory-os.md](./02-memory-os.md)*
