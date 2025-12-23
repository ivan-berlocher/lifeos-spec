# 02 — MemoryOS

> **Structured memory with intentional forgetting**

**Version**: 0.1  
**Status**: Draft  
**Last Updated**: 2025-12-23

---

## 1. Definition

> **MemoryOS is a sovereign memory architecture that stores meaningful structure, not raw data, with explicit consent and intentional forgetting.**

MemoryOS is NOT:
- A logging system
- A search index
- "Keep everything"
- AI-accessible without governance

MemoryOS IS:
- Intentional storage
- Consent-gated
- Forgetting-aware
- Structured around meaning

---

## 2. Fundamental Law

> **No memory is written without Condensation + Dissolution.**

Every piece of information must:
1. Pass through **condensation** (what's the meaning?)
2. Have a **dissolution** path (when does it expire?)

---

## 3. Memory Architecture

### 3.1 Three-Layer Model

```
┌────────────────────────────────────────────────────────────────┐
│                                                                │
│   LAYER 3: LONG-TERM MEMORY (LTM)                             │
│   ────────────────────────────────                            │
│   • Stable facts, lessons, relationships                      │
│   • Years to indefinite TTL                                   │
│   • Requires explicit promotion from MTM                      │
│   • Versioned (can be updated, never silently overwritten)    │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│   LAYER 2: MID-TERM MEMORY (MTM)                              │
│   ────────────────────────────────                            │
│   • Working context, ongoing projects, active relations       │
│   • Days to months TTL                                        │
│   • Auto-promoted from STM if reinforced                      │
│   • Auto-demoted to LTM or dissolved if not accessed          │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│   LAYER 1: SHORT-TERM MEMORY (STM)                            │
│   ────────────────────────────────                            │
│   • Current conversation, immediate context                   │
│   • Minutes to hours TTL                                      │
│   • Auto-dissolved unless promoted                            │
│   • Highest volume, lowest persistence                        │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### 3.2 Memory Flow

```
                INPUT (signal, event, decision)
                            │
                            ▼
                ┌───────────────────────┐
                │  CONDENSATION ENGINE  │
                │  "What does this mean?"│
                └───────────┬───────────┘
                            │
                            ▼
                ┌───────────────────────┐
                │      SHORT-TERM       │
                │      MEMORY (STM)     │◄─────────────────────┐
                └───────────┬───────────┘                      │
                            │                                  │
            reinforced?     │                                  │
           YES ─────────────┴───────── NO ──────► DISSOLVED    │
            │                                                  │
            ▼                                                  │
    ┌───────────────────────┐                                  │
    │      MID-TERM         │                                  │
    │      MEMORY (MTM)     │                                  │
    └───────────┬───────────┘                                  │
                │                                              │
    significant?│                                              │
   YES ─────────┴───────── NO ──────► DISSOLVED or ────────────┘
    │                                 RETURN TO STM
    ▼
┌───────────────────────┐
│      LONG-TERM        │
│      MEMORY (LTM)     │
└───────────────────────┘
```

---

## 4. Memory Types

### 4.1 Type Taxonomy

| Type | Description | Default TTL | Promotion Path |
|------|-------------|-------------|----------------|
| `EPHEMERAL` | Working context, conversation | 1-4 hours | → WORKING if important |
| `WORKING` | Active projects, ongoing tasks | 7-30 days | → FACT or NARRATIVE |
| `FACT` | Verified stable information | Indefinite | N/A (terminal) |
| `NARRATIVE` | Interpretations, summaries | Review-based | Updated in place |
| `RELATIONAL` | Connections to people/systems | Until revoked | N/A |
| `REFLECTIVE` | Lessons learned, insights | Indefinite | N/A (terminal) |

### 4.2 Type Definitions

```typescript
type MemoryType = 
  | "EPHEMERAL"
  | "WORKING"
  | "FACT"
  | "NARRATIVE"
  | "RELATIONAL"
  | "REFLECTIVE"

interface MemoryItem {
  // Identity
  id: string
  type: MemoryType
  version: number
  
  // Content
  essence: string           // Core meaning (required)
  details: unknown          // Full content (optional)
  conceptualSignature: string[]  // Key concepts
  
  // Temporality
  createdAt: Date
  updatedAt: Date
  accessedAt: Date
  ttl: Date | null          // null = indefinite
  
  // Governance
  source: MemorySource
  consent: ConsentRecord
  accessControl: AccessControl
  
  // Relations
  relatedTo: string[]       // Other memory IDs
  derivedFrom: string[]     // Source memories
  supersedes: string | null // Previous version
}

interface MemorySource {
  type: "user" | "system" | "agent" | "external"
  id: string
  timestamp: Date
}

interface ConsentRecord {
  givenAt: Date
  type: ConsentType
  scope: string[]
  revocable: boolean
}

type ConsentType =
  | "explicit"      // User explicitly agreed
  | "implicit"      // Derived from user action
  | "inherited"     // From parent memory
  | "system"        // Required for function
```

---

## 5. Memory Operations

### 5.1 WRITE Operations

```typescript
interface WriteOperation {
  type: "CREATE" | "UPDATE" | "PROMOTE" | "DEMOTE"
  memory: MemoryItem
  authorization: Authorization
  condensation: CondensationResult
}

interface CondensationResult {
  originalInput: unknown
  extractedEssence: string
  discardedDetails: string[]
  confidence: number
}
```

**Write Rules**:
- ❌ No raw data storage (must condense first)
- ❌ No silent updates (always version)
- ❌ No agent writes without proposal
- ✅ Essence is required
- ✅ TTL is required (even if null)
- ✅ Source attribution is required

### 5.2 READ Operations

```typescript
interface ReadOperation {
  type: "RETRIEVE" | "SEARCH" | "TRAVERSE"
  query: Query
  accessor: Accessor
  purpose: string
}

interface Query {
  // By identity
  ids?: string[]
  
  // By content
  conceptualMatch?: string[]
  fullTextSearch?: string
  
  // By metadata
  types?: MemoryType[]
  timeRange?: { start: Date; end: Date }
  
  // By relation
  relatedTo?: string[]
  derivedFrom?: string[]
}
```

**Read Rules**:
- ✅ Access logged
- ✅ Purpose stated
- ✅ Access control enforced
- ❌ No bulk exports without consent

### 5.3 DELETE Operations

```typescript
interface DeleteOperation {
  type: "DISSOLVE" | "ARCHIVE" | "PURGE"
  memoryIds: string[]
  reason: DissolutionReason
}

type DissolutionReason =
  | "ttl_expired"           // Natural expiration
  | "user_request"          // User asked to delete
  | "consent_revoked"       // Consent withdrawn
  | "superseded"            // Replaced by new version
  | "irrelevant"            // No longer meaningful
  | "privacy"               // Privacy concern
```

**Dissolution Types**:

| Operation | Effect | Reversible |
|-----------|--------|------------|
| `DISSOLVE` | Remove from active memory, keep audit trail | Yes (30 days) |
| `ARCHIVE` | Move to cold storage, not in active queries | Yes |
| `PURGE` | Complete deletion, no trace | No |

---

## 6. Memory Promotion & Demotion

### 6.1 Promotion Triggers

```
STM → MTM (Reinforcement):
  - Accessed 3+ times in 24 hours
  - Referenced in a decision
  - User explicitly marks important
  - Related to active project

MTM → LTM (Significance):
  - Accessed across 7+ days
  - Used in multiple contexts
  - User explicitly promotes
  - Verified as accurate
```

### 6.2 Demotion Triggers

```
MTM → STM (Decay):
  - Not accessed in TTL period
  - Related project completed
  - Contradicted by newer information

LTM → MTM (Review):
  - User marks for review
  - Confidence score drops
  - Contradictory evidence found
```

### 6.3 Promotion Engine

```typescript
interface PromotionEngine {
  evaluate(memory: MemoryItem): PromotionDecision
  schedule(memory: MemoryItem, targetLayer: MemoryLayer): void
  execute(scheduled: ScheduledPromotion[]): void
}

interface PromotionDecision {
  currentLayer: MemoryLayer
  recommendedAction: "PROMOTE" | "DEMOTE" | "MAINTAIN" | "DISSOLVE"
  targetLayer?: MemoryLayer
  confidence: number
  reasons: string[]
}
```

---

## 7. Forgetting as Feature

### 7.1 Philosophy

> **Forgetting is not a bug. It is the most important feature.**

Without intentional forgetting:
- ❌ Signal-to-noise ratio degrades
- ❌ Old patterns override new learning
- ❌ Storage becomes unmanageable
- ❌ Privacy becomes impossible

### 7.2 Forgetting Strategies

| Strategy | Description | Trigger |
|----------|-------------|---------|
| **TTL Expiration** | Natural death by time | Clock |
| **Displacement** | New memories push out old | Capacity |
| **Consolidation** | Multiple memories → summary | Pattern detection |
| **Pruning** | Remove irrelevant connections | Graph analysis |
| **Redaction** | Remove sensitive details, keep structure | Privacy rules |

### 7.3 Anti-Patterns

What MemoryOS explicitly DOES NOT do:

| Anti-Pattern | Why It's Harmful |
|--------------|------------------|
| "Log everything" | Noise overwhelms signal |
| "Never delete" | Privacy impossible |
| "AI decides importance" | Human sovereignty lost |
| "Optimize for recall" | Forgetting is healthy |

---

## 8. Memory and Solid Integration

### 8.1 Storage Location

```
┌─────────────────────────────────────────────────────────┐
│                    USER'S SOLID POD                     │
├─────────────────────────────────────────────────────────┤
│                                                         │
│   /lifeos/                                              │
│   ├── memory/                                           │
│   │   ├── stm/           # Short-term (encrypted)       │
│   │   ├── mtm/           # Mid-term (encrypted)         │
│   │   ├── ltm/           # Long-term (encrypted)        │
│   │   └── index.ttl      # RDF index (public structure) │
│   │                                                     │
│   ├── consent/                                          │
│   │   └── grants.ttl     # Access control               │
│   │                                                     │
│   └── audit/                                            │
│       └── access.log     # Who accessed what            │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 8.2 Data Portability

```typescript
interface MemoryExport {
  format: "json-ld" | "turtle" | "json"
  memories: MemoryItem[]
  relations: Relation[]
  metadata: ExportMetadata
}

interface ExportMetadata {
  exportedAt: Date
  exportedBy: string
  purpose: string
  scope: string[]
  signature: string
}
```

---

## 9. Access Control

### 9.1 Access Levels

| Level | Can Read | Can Write | Can Delete |
|-------|----------|-----------|------------|
| **Owner** | All | All | All |
| **Trusted Agent** | Permitted | Propose only | Never |
| **External App** | Permitted | Never | Never |
| **Anonymous** | Public only | Never | Never |

### 9.2 Access Control List

```typescript
interface AccessControl {
  owner: string
  grants: AccessGrant[]
  defaultPolicy: "deny" | "allow_read" | "allow_write"
}

interface AccessGrant {
  grantee: string
  permissions: Permission[]
  scope: string[]
  validUntil: Date | null
  conditions: Condition[]
}

type Permission = "read" | "write" | "delete" | "share"

interface Condition {
  type: "time" | "location" | "purpose" | "approval"
  value: unknown
}
```

---

## 10. Implementation Requirements

A MemoryOS implementation is compliant if:

| Requirement | Verification |
|-------------|--------------|
| Three-layer model | STM, MTM, LTM all present |
| Condensation required | No raw data storage |
| TTL enforced | All memories have dissolution path |
| Versioning active | Updates create versions, not overwrites |
| Consent tracked | Every memory has consent record |
| Access logged | All reads/writes auditable |
| Forgetting works | TTL expiration actually deletes |
| Solid-compatible | Can export to Solid Pod |

---

## 11. JSON Schema Reference

See [schemas/memory-item.json](../schemas/memory-item.json) for the complete JSON Schema.

---

## 12. Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2025-12-23 | Initial draft |

---

*Previous: [01-kernel.md](./01-kernel.md)*  
*Next: [03-sense-making.md](./03-sense-making.md)*
