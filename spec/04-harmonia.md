# 04 — Harmonia

> **Cognitive orchestration layer — Intent to Action**

**Version**: 0.1  
**Status**: Draft  
**Last Updated**: 2025-12-23

---

## 1. Definition

> **Harmonia is the orchestration layer that transforms user intent into coordinated agent actions, while preserving human authority through the Double Lock mechanism.**

Harmonia is NOT:
- An autonomous agent
- A decision maker
- A replacement for human judgment

Harmonia IS:
- An intent router
- An agent coordinator
- A composition engine
- A gatekeeper for the Double Lock

---

## 2. Position in Architecture

```
                    ┌─────────────────────────────────┐
                    │           👤 USER               │
                    └───────────────┬─────────────────┘
                                    │ intent
                                    ▼
                    ┌─────────────────────────────────┐
                    │         SENSE-MAKING            │
                    │   (understand what user means)   │
                    └───────────────┬─────────────────┘
                                    │ frames
                                    ▼
          ┌─────────────────────────────────────────────────────┐
          │                                                     │
          │                    HARMONIA                         │
          │                                                     │
          │   ┌──────────┐   ┌──────────┐   ┌──────────┐       │
          │   │  INTENT  │──►│  ROUTE   │──►│ COMPOSE  │       │
          │   │  DETECT  │   │          │   │          │       │
          │   └──────────┘   └──────────┘   └──────────┘       │
          │                                                     │
          └───────────────────────┬─────────────────────────────┘
                                  │ proposals
                                  ▼
                    ┌─────────────────────────────────┐
                    │         DOUBLE LOCK             │
                    │   Lock 1: "What is this?"       │
                    │   Lock 2: "Execute?"            │
                    └───────────────┬─────────────────┘
                                    │ authorized
                                    ▼
                    ┌─────────────────────────────────┐
                    │          ACTION OS              │
                    └─────────────────────────────────┘
```

---

## 3. The Three Stages

### 3.1 INTENT DETECTION

**Input**: SenseMaking frames  
**Output**: Classified intent

```typescript
interface IntentDetection {
  primaryIntent: Intent
  secondaryIntents: Intent[]
  confidence: number
  ambiguities: IntentAmbiguity[]
}

interface Intent {
  id: string
  type: IntentType
  domain: string[]           // Which domains are relevant
  urgency: "low" | "medium" | "high" | "critical"
  complexity: "simple" | "compound" | "complex"
  requiresContext: string[]  // What context is needed
}

type IntentType =
  | "query"           // User wants information
  | "command"         // User wants action
  | "exploration"     // User wants options
  | "clarification"   // User needs understanding
  | "delegation"      // User wants agent to handle
  | "conversation"    // User wants dialogue
```

### 3.2 ROUTING

**Input**: Detected intent  
**Output**: Agent assignments

```typescript
interface RoutingDecision {
  primaryAgent: AgentAssignment
  supportAgents: AgentAssignment[]
  memoryAccess: MemoryAccessGrant[]
  constraints: RoutingConstraint[]
}

interface AgentAssignment {
  agentId: string
  role: "lead" | "support" | "validator"
  scope: string[]
  timeout: number
}

interface RoutingConstraint {
  type: "sequential" | "parallel" | "conditional"
  condition?: string
  dependencies?: string[]
}
```

### 3.3 COMPOSITION

**Input**: Agent outputs  
**Output**: Unified response/proposal

```typescript
interface CompositionResult {
  type: CompositionType
  content: unknown
  proposals: ActionProposal[]
  metadata: CompositionMetadata
}

type CompositionType =
  | "response"        // Direct answer to user
  | "proposal"        // Action requiring authorization
  | "clarification"   // Need more information
  | "options"         // Multiple paths forward
  | "delegation"      // Hand off to specialist

interface ActionProposal {
  id: string
  action: Action
  rationale: string
  confidence: number
  reversible: boolean
  alternatives: ActionProposal[]
}
```

---

## 4. The Double Lock Mechanism

### 4.1 Lock 1 — Cognitive Lock

> "What is this, really?"

The user must understand what is being proposed before any execution.

```typescript
interface CognitiveLock {
  proposal: ActionProposal
  explanation: {
    whatWillHappen: string
    whyItWasProposed: string
    whatItWillAffect: string[]
    alternatives: string[]
  }
  userResponse: "understood" | "clarify" | "reject"
}
```

### 4.2 Lock 2 — Authorization Lock

> "Do you want me to execute this?"

Only after cognitive lock passes, user can authorize execution.

```typescript
interface AuthorizationLock {
  proposal: ActionProposal
  cognitiveApproved: boolean
  authorizationType: AuthorizationType
  userResponse: "execute" | "modify" | "reject" | "defer"
}

type AuthorizationType =
  | "explicit"        // User says yes
  | "standing"        // Pre-authorized pattern
  | "delegated"       // Trusted agent authorized
  | "emergency"       // Critical situation bypass (logged)
```

### 4.3 Lock Bypass Rules

| Condition | Lock 1 | Lock 2 | Audit |
|-----------|--------|--------|-------|
| Normal operation | Required | Required | Standard |
| Standing authorization | Skipped | Auto-approved | Logged |
| Emergency | Skipped | Auto-approved | Full trace |
| Learning mode | Required | Show only | No execution |

---

## 5. Agent Coordination

### 5.1 Agent Roles

```
┌────────────────────────────────────────────────────────────────┐
│                       HARMONIA CONDUCTOR                       │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│   ┌──────────────┐                                             │
│   │ LEAD AGENT   │ ◄── Primary responsibility                  │
│   │              │     Produces main output                    │
│   └──────┬───────┘                                             │
│          │                                                     │
│          ├──────────┬──────────┐                               │
│          ▼          ▼          ▼                               │
│   ┌──────────┐ ┌──────────┐ ┌──────────┐                       │
│   │ SUPPORT  │ │ SUPPORT  │ │VALIDATOR │                       │
│   │ AGENT 1  │ │ AGENT 2  │ │ AGENT    │                       │
│   │          │ │          │ │          │                       │
│   │ Provides │ │ Provides │ │ Checks   │                       │
│   │ context  │ │ data     │ │ output   │                       │
│   └──────────┘ └──────────┘ └──────────┘                       │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### 5.2 Coordination Patterns

| Pattern | Description | Use Case |
|---------|-------------|----------|
| **Sequential** | Agents execute in order | Research → Summarize → Present |
| **Parallel** | Agents execute simultaneously | Multi-source search |
| **Conditional** | Agent executes based on condition | Fallback if primary fails |
| **Competitive** | Multiple agents, best output wins | Quality optimization |

---

## 6. Domain Routing

### 6.1 Domain Registry

```typescript
interface DomainRegistry {
  domains: Domain[]
  routingRules: RoutingRule[]
}

interface Domain {
  id: string
  name: string
  capabilities: string[]
  agents: string[]
  memoryScopes: string[]
}
```

### 6.2 Standard Domains

| Domain | Purpose | Example Intents |
|--------|---------|-----------------|
| `calendar` | Time management | "Schedule meeting", "What's next?" |
| `memory` | Knowledge retrieval | "Remind me about...", "What did we discuss?" |
| `communication` | Messaging | "Send email to...", "Draft reply" |
| `research` | Information gathering | "Find out about...", "Compare options" |
| `creative` | Content generation | "Write a...", "Help me brainstorm" |
| `system` | LifeOS management | "Change settings", "Show status" |

---

## 7. Error Handling

### 7.1 Failure Modes

```typescript
type HarmoniaFailure =
  | "intent_unclear"       // Can't understand what user wants
  | "no_agent_available"   // No agent can handle this
  | "agent_timeout"        // Agent took too long
  | "agent_failure"        // Agent returned error
  | "composition_failed"   // Couldn't combine outputs
  | "lock_rejected"        // User rejected at lock
```

### 7.2 Recovery Strategies

| Failure | Strategy |
|---------|----------|
| `intent_unclear` | Request clarification |
| `no_agent_available` | Escalate to user |
| `agent_timeout` | Retry with simpler scope |
| `agent_failure` | Try alternative agent |
| `composition_failed` | Present partial results |
| `lock_rejected` | Offer alternatives |

---

## 8. Implementation Requirements

A Harmonia implementation is compliant if:

| Requirement | Verification |
|-------------|--------------|
| Three stages present | Intent → Route → Compose |
| Double Lock enforced | No execution without both locks |
| Agent coordination works | Multiple agents can collaborate |
| Failures handled gracefully | All failure modes have recovery |
| Audit trail exists | All decisions are logged |

---

## 9. Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2025-12-23 | Initial draft |

---

*Previous: [03-sense-making.md](./03-sense-making.md)*  
*Next: [05-solid-bridge.md](./05-solid-bridge.md)*
