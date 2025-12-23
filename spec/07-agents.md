# 07 — Agents

> **Bounded intelligence under human control**

**Version**: 0.1  
**Status**: Draft  
**Last Updated**: 2025-12-23

---

## 1. Definition

> **Agents are temporary, specialized, bounded computations that operate under explicit human authorization with mandatory kill switches.**

Agents are NOT:
- Autonomous entities
- Decision makers
- Persistent beings
- Free to act

Agents ARE:
- Task-specific tools
- Human-controlled
- Temporary by design
- Transparent in operation

---

## 2. Agent Principles

### 2.1 The Five Laws

```
┌────────────────────────────────────────────────────────────────┐
│                    THE FIVE LAWS OF AGENTS                     │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│   1. TEMPORARY                                                 │
│      Agents are spawned for tasks, terminated after.           │
│      No agent persists without explicit renewal.               │
│                                                                │
│   2. SPECIALIZED                                               │
│      One agent, one domain.                                    │
│      No "general purpose" agents.                              │
│                                                                │
│   3. BOUNDED                                                   │
│      Explicit permissions, explicit limitations.               │
│      Cannot exceed declared scope.                             │
│                                                                │
│   4. VISIBLE                                                   │
│      User can see all active agents at any time.               │
│      No hidden agents, no background operations.               │
│                                                                │
│   5. REVOCABLE                                                 │
│      User can kill any agent instantly.                        │
│      Kill switch is non-negotiable.                            │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### 2.2 What Agents Cannot Do

| Prohibition | Rationale |
|-------------|-----------|
| ❌ Write to MemoryOS directly | Must propose, user approves |
| ❌ Execute actions without Double Lock | Human must authorize |
| ❌ Spawn other agents | Prevents agent proliferation |
| ❌ Persist across sessions | Temporary by design |
| ❌ Modify their own permissions | No self-elevation |
| ❌ Access undeclared data | Scope is fixed at spawn |

---

## 3. Agent Lifecycle

### 3.1 States

```
                    ┌─────────────────┐
                    │    DORMANT      │
                    │  (not spawned)  │
                    └────────┬────────┘
                             │
                       spawn request
                             │
                             ▼
                    ┌─────────────────┐
                    │   INITIALIZING  │
                    │  (loading...)   │
                    └────────┬────────┘
                             │
                       ready
                             │
                             ▼
         ┌───────────────────────────────────────┐
         │                                       │
         ▼                                       │
┌─────────────────┐                              │
│     ACTIVE      │◄─────────────────────────────┤
│  (working)      │                              │
└────────┬────────┘                              │
         │                                       │
         ├─── task complete ──► TERMINATING      │
         │                                       │
         ├─── timeout ────────► TERMINATING      │
         │                                       │
         ├─── user kill ──────► TERMINATING      │
         │                                       │
         └─── paused ─────────► PAUSED ──────────┘
                                    │
                                    └─── resume
```

### 3.2 Lifecycle Events

```typescript
interface AgentLifecycle {
  // Creation
  spawn(manifest: AgentManifest, context: SpawnContext): Promise<Agent>
  
  // Control
  pause(agentId: string): Promise<void>
  resume(agentId: string): Promise<void>
  kill(agentId: string, reason: string): Promise<void>
  
  // Monitoring
  getStatus(agentId: string): AgentStatus
  listActive(): Agent[]
  
  // Events
  onStateChange(callback: (event: StateChangeEvent) => void): void
}
```

---

## 4. Agent Manifest

### 4.1 Required Declaration

Every agent must declare its full manifest before spawning:

```typescript
interface AgentManifest {
  // Identity
  id: string
  name: string
  version: string
  description: string
  
  // Domain
  domain: AgentDomain
  capabilities: string[]
  limitations: string[]
  
  // Permissions
  dataAccess: {
    read: DataScope[]
    write: DataScope[]      // Propose only
  }
  
  // Constraints
  ttl: number               // Max lifetime in seconds
  maxMemoryMB: number       // Memory limit
  maxTokens: number         // Token budget
  
  // Governance
  killSwitch: true          // Always true (non-negotiable)
  auditLevel: AuditLevel
  
  // Dependencies
  requires: string[]        // Other agents or services
  conflicts: string[]       // Cannot run alongside
}

type AgentDomain =
  | "calendar"
  | "email"
  | "research"
  | "creative"
  | "memory"
  | "system"
  | "custom"

type DataScope = {
  type: "memory" | "external" | "conversation"
  path: string
  filter?: string
}

type AuditLevel = "minimal" | "standard" | "verbose"
```

### 4.2 Example Manifest

```json
{
  "id": "calendar-agent-v1",
  "name": "Calendar Agent",
  "version": "1.0.0",
  "description": "Manages calendar events and scheduling",
  
  "domain": "calendar",
  "capabilities": [
    "read_calendar_events",
    "propose_new_events",
    "suggest_reschedules",
    "detect_conflicts"
  ],
  "limitations": [
    "cannot_delete_events_directly",
    "cannot_access_email",
    "cannot_book_external_resources"
  ],
  
  "dataAccess": {
    "read": [
      { "type": "memory", "path": "/calendar/**" },
      { "type": "memory", "path": "/contacts/names" }
    ],
    "write": [
      { "type": "memory", "path": "/calendar/proposed/**" }
    ]
  },
  
  "ttl": 3600,
  "maxMemoryMB": 512,
  "maxTokens": 10000,
  
  "killSwitch": true,
  "auditLevel": "standard",
  
  "requires": [],
  "conflicts": ["calendar-agent-v2"]
}
```

---

## 5. Agent Types

### 5.1 Domain Agents

| Agent | Domain | Purpose |
|-------|--------|---------|
| **CalendarAgent** | calendar | Schedule management |
| **EmailAgent** | email | Message handling |
| **ResearchAgent** | research | Information gathering |
| **CreativeAgent** | creative | Content generation |
| **MemoryAgent** | memory | Memory retrieval |
| **SystemAgent** | system | LifeOS management |

### 5.2 Agent Hierarchy

```
┌────────────────────────────────────────────────────────────────┐
│                    AGENT HIERARCHY                             │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│   HARMONIA (Orchestrator)                                      │
│   ├── Routes to appropriate agents                             │
│   ├── Coordinates multi-agent tasks                            │
│   └── Composes outputs                                         │
│                                                                │
│   DOMAIN AGENTS                                                │
│   ├── CalendarAgent ─── calendar operations                    │
│   ├── EmailAgent ────── email operations                       │
│   ├── ResearchAgent ─── information gathering                  │
│   ├── CreativeAgent ─── content generation                     │
│   └── MemoryAgent ───── memory operations                      │
│                                                                │
│   UTILITY AGENTS                                               │
│   ├── ValidatorAgent ── checks outputs                         │
│   ├── FormatterAgent ── formats responses                      │
│   └── TranslatorAgent ─ language conversion                    │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

## 6. Agent Communication

### 6.1 Message Protocol

```typescript
interface AgentMessage {
  id: string
  from: string              // Agent ID or "user" or "harmonia"
  to: string                // Agent ID or "user" or "harmonia"
  type: MessageType
  payload: unknown
  timestamp: Date
  replyTo?: string          // For responses
}

type MessageType =
  | "request"               // Ask agent to do something
  | "response"              // Agent's answer
  | "proposal"              // Agent proposes action
  | "status"                // Agent reports status
  | "error"                 // Something went wrong
  | "terminate"             // Agent is ending
```

### 6.2 Communication Flow

```
User Intent
     │
     ▼
┌──────────────────┐
│    HARMONIA      │
│   (conductor)    │
└────────┬─────────┘
         │
         ├───────────────────────────────┐
         │                               │
         ▼                               ▼
┌──────────────────┐           ┌──────────────────┐
│   LEAD AGENT     │           │  SUPPORT AGENT   │
│                  │◄─────────►│                  │
│   request        │  messages │   context        │
│   response       │           │   data           │
└────────┬─────────┘           └──────────────────┘
         │
         ▼
┌──────────────────┐
│    HARMONIA      │
│   (compose)      │
└────────┬─────────┘
         │
         ▼
    Double Lock
         │
         ▼
    User Response
```

---

## 7. Agent Permissions

### 7.1 Permission Model

```typescript
interface AgentPermissions {
  // Memory access
  memory: {
    read: MemoryScope[]
    propose: MemoryScope[]    // Cannot write directly
  }
  
  // External access
  external: {
    apis: ExternalAPI[]
    network: NetworkScope
  }
  
  // Action capabilities
  actions: {
    canPropose: ActionType[]
    cannotPropose: ActionType[]
  }
}

interface MemoryScope {
  layer: "stm" | "mtm" | "ltm" | "all"
  types: MemoryType[]
  filter?: string
}

interface ExternalAPI {
  service: string
  endpoints: string[]
  rateLimit: number
}
```

### 7.2 Permission Inheritance

```
DEFAULT PERMISSIONS (most restrictive)
         │
         ├─► Domain permissions (based on agent type)
         │
         └─► Custom permissions (user-granted)
                │
                └─► Active permissions (intersection)
```

---

## 8. Kill Switch

### 8.1 Mandatory Implementation

```typescript
interface KillSwitch {
  // Immediate termination
  kill(agentId: string, reason: KillReason): Promise<void>
  
  // Kill all agents
  killAll(reason: KillReason): Promise<void>
  
  // Guaranteed execution time
  maxKillTimeMs: 1000       // Must complete in <1 second
  
  // Cannot be disabled
  readonly enabled: true
}

type KillReason =
  | "user_request"          // User clicked kill
  | "timeout"               // TTL expired
  | "resource_limit"        // Exceeded memory/tokens
  | "error"                 // Unrecoverable error
  | "conflict"              // Conflicting agent spawned
  | "system_shutdown"       // LifeOS shutting down
```

### 8.2 Kill Sequence

```
1. KILL signal received
         │
         ▼
2. Agent enters TERMINATING state (instant)
         │
         ▼
3. All pending operations cancelled
         │
         ▼
4. Resources released
         │
         ▼
5. Audit log written
         │
         ▼
6. Agent removed from registry
         │
         ▼
7. Confirmation sent to user

TOTAL TIME: < 1 second (guaranteed)
```

---

## 9. Audit Trail

### 9.1 What's Logged

| Event | Data Captured |
|-------|---------------|
| Spawn | Manifest, context, timestamp |
| Memory Read | What was accessed |
| Proposal | What was proposed |
| Message | All inter-agent messages |
| State Change | Every state transition |
| Kill | Reason, duration, state at kill |

### 9.2 Audit Format

```typescript
interface AgentAuditEntry {
  id: string
  agentId: string
  timestamp: Date
  eventType: AuditEventType
  details: Record<string, unknown>
  userVisible: boolean
}

type AuditEventType =
  | "spawn"
  | "memory_read"
  | "memory_propose"
  | "external_call"
  | "message_sent"
  | "message_received"
  | "state_change"
  | "error"
  | "kill"
```

---

## 10. Implementation Requirements

An Agent implementation is compliant if:

| Requirement | Verification |
|-------------|--------------|
| Manifest required | No spawn without full manifest |
| TTL enforced | Auto-kill at timeout |
| Kill switch works | Terminates in <1 second |
| Permissions honored | Cannot exceed declared scope |
| Audit complete | All events logged |
| No persistence | Clean slate each session |
| Visible to user | User can list all agents |

---

## 11. Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2025-12-23 | Initial draft |

---

*Previous: [06-federation.md](./06-federation.md)*  
*Index: [00-overview.md](./00-overview.md)*
