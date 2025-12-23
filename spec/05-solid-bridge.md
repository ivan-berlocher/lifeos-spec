# 05 — Solid Bridge

> **Integration with Solid Protocol for data sovereignty**

**Version**: 0.1  
**Status**: Draft  
**Last Updated**: 2025-12-23

---

## 1. Definition

> **The Solid Bridge is the integration layer that connects LifeOS to the Solid Protocol, ensuring user data sovereignty and true portability.**

Solid Bridge is NOT:
- A replacement for Solid
- A proprietary data format
- A lock-in mechanism

Solid Bridge IS:
- A mapping layer
- A sync mechanism
- A sovereignty guarantee
- Solid-first, LifeOS-second

---

## 2. Why Solid?

### 2.1 Solid Protocol Principles

| Principle | Description | LifeOS Alignment |
|-----------|-------------|------------------|
| **Data Ownership** | Users own their data | ✅ Core principle |
| **Decentralization** | No central authority | ✅ Federation model |
| **Interoperability** | Apps work across pods | ✅ Open specification |
| **Consent** | Explicit data sharing | ✅ Double Lock |

### 2.2 What Solid Provides

```
┌─────────────────────────────────────────────────────────────┐
│                    SOLID ECOSYSTEM                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   👤 PODS (Personal Online Data Stores)                     │
│   • User-controlled storage                                 │
│   • Self-hosted or provider-hosted                          │
│   • RDF-based linked data                                   │
│                                                             │
│   🔐 WEBID (Decentralized Identity)                         │
│   • No central identity provider                            │
│   • User controls their identity                            │
│   • Portable across platforms                               │
│                                                             │
│   📋 ACCESS CONTROL (WAC/ACP)                               │
│   • Fine-grained permissions                                │
│   • User decides who sees what                              │
│   • Revocable at any time                                   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Architecture

### 3.1 LifeOS ↔ Solid Mapping

```
┌─────────────────────────────────────────────────────────────┐
│                        LIFEOS                               │
│                                                             │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│   │   MemoryOS   │  │  SenseMaking │  │   ActionOS   │     │
│   │              │  │    Frames    │  │    Logs      │     │
│   └──────┬───────┘  └──────┬───────┘  └──────┬───────┘     │
│          │                 │                 │              │
└──────────┼─────────────────┼─────────────────┼──────────────┘
           │                 │                 │
           ▼                 ▼                 ▼
┌─────────────────────────────────────────────────────────────┐
│                     SOLID BRIDGE                            │
│                                                             │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│   │   Mapper     │  │   Syncer     │  │   Access     │     │
│   │  (RDF/JSON)  │  │ (Bi-direct.) │  │  (WAC/ACP)   │     │
│   └──────┬───────┘  └──────┬───────┘  └──────┬───────┘     │
│          │                 │                 │              │
└──────────┼─────────────────┼─────────────────┼──────────────┘
           │                 │                 │
           ▼                 ▼                 ▼
┌─────────────────────────────────────────────────────────────┐
│                      SOLID POD                              │
│                                                             │
│   /lifeos/                                                  │
│   ├── memory/          (RDF triples)                        │
│   ├── frames/          (RDF triples)                        │
│   ├── actions/         (Activity Streams)                   │
│   └── .acl             (Access Control)                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 Pod Structure

```
/pod-root/
├── profile/
│   └── card#me                    # WebID profile
├── lifeos/
│   ├── memory/
│   │   ├── stm/                   # Short-term memory
│   │   ├── mtm/                   # Mid-term memory
│   │   ├── ltm/                   # Long-term memory
│   │   └── index.ttl              # Memory index
│   ├── frames/
│   │   └── sensemaking/           # SenseMaking frames
│   ├── actions/
│   │   └── log.ttl                # Action history
│   ├── agents/
│   │   └── manifests/             # Agent declarations
│   └── consent/
│       └── grants.ttl             # Access grants
└── .acl                           # Pod-level ACL
```

---

## 4. Data Mapping

### 4.1 MemoryItem → RDF

```turtle
@prefix lifeos: <https://lifeos.dev/vocab#> .
@prefix schema: <https://schema.org/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<#memory-123>
    a lifeos:MemoryItem ;
    lifeos:type "FACT" ;
    lifeos:essence "Ivan's birthday is June 15"@en ;
    lifeos:conceptualSignature ("birthday" "ivan" "june") ;
    lifeos:createdAt "2025-12-23T10:00:00Z"^^xsd:dateTime ;
    lifeos:ttl "indefinite" ;
    lifeos:version 1 ;
    lifeos:source [
        a lifeos:Source ;
        lifeos:sourceType "user" ;
        lifeos:sourceId "webid:me"
    ] ;
    lifeos:consent [
        a lifeos:Consent ;
        lifeos:consentType "explicit" ;
        lifeos:revocable true
    ] .
```

### 4.2 SenseMakingFrame → RDF

```turtle
@prefix lifeos: <https://lifeos.dev/vocab#> .

<#frame-456>
    a lifeos:SenseMakingFrame ;
    lifeos:createdAt "2025-12-23T10:05:00Z"^^xsd:dateTime ;
    lifeos:decision [
        a lifeos:Decision ;
        lifeos:decisionType "propose_action" ;
        lifeos:confidence "0.85"^^xsd:decimal ;
        lifeos:rationale "User mentioned deadline, calendar shows task due tomorrow"
    ] ;
    lifeos:humanReviewed false .
```

---

## 5. Sync Protocol

### 5.1 Sync Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| **Real-time** | Immediate sync | Active session |
| **Batch** | Periodic sync | Background |
| **On-demand** | User-triggered | Manual backup |
| **Offline** | Queue for later | No connection |

### 5.2 Conflict Resolution

```typescript
interface SyncConflict {
  localVersion: MemoryItem
  remoteVersion: MemoryItem
  conflictType: ConflictType
  resolution: ConflictResolution
}

type ConflictType =
  | "concurrent_update"    // Both changed
  | "deleted_remotely"     // Deleted in pod
  | "schema_mismatch"      // Format incompatible

type ConflictResolution =
  | "local_wins"          // Keep local version
  | "remote_wins"         // Keep pod version
  | "merge"               // Combine both
  | "user_decides"        // Ask user
```

---

## 6. Access Control

### 6.1 WAC (Web Access Control)

```turtle
# .acl file for /lifeos/memory/
@prefix acl: <http://www.w3.org/ns/auth/acl#> .

<#owner>
    a acl:Authorization ;
    acl:agent <https://me.pod/profile/card#me> ;
    acl:accessTo </lifeos/memory/> ;
    acl:default </lifeos/memory/> ;
    acl:mode acl:Read, acl:Write, acl:Control .

<#trusted-agent>
    a acl:Authorization ;
    acl:agent <https://lifeos.dev/agents/calendar#agent> ;
    acl:accessTo </lifeos/memory/> ;
    acl:mode acl:Read .
```

### 6.2 Permission Levels

| Level | Read | Write | Delete | Share |
|-------|------|-------|--------|-------|
| Owner | ✅ | ✅ | ✅ | ✅ |
| Trusted Agent | ✅ | Propose | ❌ | ❌ |
| External App | Grant-based | ❌ | ❌ | ❌ |
| Public | ❌ | ❌ | ❌ | ❌ |

---

## 7. Federation

### 7.1 Cross-Pod Communication

```
┌──────────────┐                    ┌──────────────┐
│   POD A      │                    │   POD B      │
│   (User A)   │                    │   (User B)   │
│              │                    │              │
│  ┌────────┐  │    ◄──────────►    │  ┌────────┐  │
│  │LifeOS  │  │   Shared memory    │  │LifeOS  │  │
│  │Instance│  │   (with consent)   │  │Instance│  │
│  └────────┘  │                    │  └────────┘  │
│              │                    │              │
└──────────────┘                    └──────────────┘
```

### 7.2 Shared Memory Protocol

```typescript
interface SharedMemory {
  source: WebId
  target: WebId
  memoryId: string
  permissions: Permission[]
  syncPolicy: SyncPolicy
  expiresAt: Date | null
}

type SyncPolicy =
  | "source_of_truth"     // Source always wins
  | "bidirectional"       // Both can update
  | "read_only"           // Target can only read
```

---

## 8. Portability

### 8.1 Export

```typescript
interface PodExport {
  format: "turtle" | "json-ld" | "nquads"
  scope: string[]
  encryption: boolean
  signature: string
}
```

### 8.2 Import

```typescript
interface PodImport {
  source: URL
  mapping: ImportMapping
  conflictPolicy: ConflictResolution
  validation: boolean
}
```

### 8.3 Migration Path

```
1. Export from old pod     → Standard RDF format
2. Validate data integrity → Checksums
3. Import to new pod       → Mapping rules
4. Verify import           → Compare counts
5. Update WebID pointers   → Update profile
6. Decommission old pod    → After grace period
```

---

## 9. Implementation Requirements

A Solid Bridge implementation is compliant if:

| Requirement | Verification |
|-------------|--------------|
| Pod structure correct | /lifeos/ hierarchy present |
| RDF mapping valid | Validates against ontology |
| Sync works both ways | Changes propagate |
| ACL enforced | Unauthorized access blocked |
| Export works | Full data export possible |
| Import works | Can restore from export |

---

## 10. Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2025-12-23 | Initial draft |

---

*Previous: [04-harmonia.md](./04-harmonia.md)*  
*Next: [06-federation.md](./06-federation.md)*
