# 03 — SenseMaking

> **Transform fragmented signals into meaningful structure, with traceable interpretive decisions**

**Version**: 0.1  
**Status**: Draft  
**Last Updated**: 2025-12-23

---

## 1. Definition

> **SenseMaking is the computable process of transforming raw signals into actionable meaning, while preserving the interpretive trace that makes decisions explainable.**

SenseMaking is NOT:
- A black-box AI inference
- Pattern matching without context
- Automatic decision-making

SenseMaking IS:
- A structured cognitive process
- Explicit about uncertainty
- Traceable at every step
- Human-reviewable

---

## 2. The SenseMaking Loop

```
     ┌────────────────────────────────────────────────────┐
     │                                                    │
     │   ┌──────────┐                                     │
     │   │ SCANNING │ ◄─────────────────────────────────┐ │
     │   └────┬─────┘                                   │ │
     │        │ signals detected                        │ │
     │        ▼                                         │ │
     │   ┌──────────┐                                   │ │
     │   │ NOTICING │                                   │ │
     │   └────┬─────┘                                   │ │
     │        │ patterns emerged                        │ │
     │        ▼                                         │ │
     │   ┌──────────────┐                               │ │
     │   │ INTERPRETING │                               │ │
     │   └────┬─────────┘                               │ │
     │        │ meaning assigned                        │ │
     │        ▼                                         │ │
     │   ┌──────────┐                                   │ │
     │   │ DECIDING │                                   │ │
     │   └────┬─────┘                                   │ │
     │        │ action selected                         │ │
     │        ▼                                         │ │
     │   ┌──────────┐                                   │ │
     │   │ ENACTING │ ───────── feedback ───────────────┘ │
     │   └──────────┘                                     │
     │                                                    │
     └────────────────────────────────────────────────────┘
```

---

## 3. The Five Stages

### 3.1 SCANNING — Signal Collection

**Input**: Raw world data (text, voice, sensor, context)  
**Output**: Normalized signal set

```typescript
interface ScanResult {
  signals: Signal[]
  timestamp: Date
  contextWindow: ContextWindow
  confidence: number  // 0-1: scan quality
}

interface Signal {
  id: string
  type: SignalType
  source: string
  raw: unknown
  normalized: string
  intensity: number  // 0-1: signal strength
}

type SignalType = 
  | "linguistic"      // Words, text
  | "temporal"        // Time-based patterns
  | "behavioral"      // User actions
  | "environmental"   // Context (location, device)
  | "relational"      // From other people/systems
```

**Constraints**:
- ❌ No filtering at this stage (preserve all signals)
- ❌ No interpretation (just capture)
- ✅ Normalization to common format
- ✅ Source attribution

---

### 3.2 NOTICING — Pattern Emergence

**Input**: Signal set from SCANNING  
**Output**: Noticed patterns with salience scores

```typescript
interface NoticeResult {
  patterns: Pattern[]
  ignored: Signal[]       // Signals below threshold
  attentionMap: Map<string, number>  // What drew attention
}

interface Pattern {
  id: string
  type: PatternType
  signals: Signal[]       // Contributing signals
  salience: number        // 0-1: importance score
  novelty: number         // 0-1: how new/unexpected
  urgency: number         // 0-1: time sensitivity
}

type PatternType =
  | "recurring"           // Seen before
  | "anomaly"             // Breaks expectations
  | "convergence"         // Multiple signals align
  | "absence"             // Expected signal missing
  | "sequence"            // Temporal pattern
```

**Attention Formula**:

$$
\text{Attention}(p) = w_s \cdot \text{salience} + w_n \cdot \text{novelty} + w_u \cdot \text{urgency}
$$

Where default weights are:
- $w_s = 0.4$ (salience)
- $w_n = 0.3$ (novelty)
- $w_u = 0.3$ (urgency)

**Constraints**:
- ❌ No meaning assignment yet
- ✅ Explicit about what was ignored
- ✅ Salience is explainable

---

### 3.3 INTERPRETING — Meaning Assignment

**Input**: Patterns from NOTICING  
**Output**: Interpreted frames with confidence

```typescript
interface InterpretResult {
  frames: Frame[]
  ambiguities: Ambiguity[]
  interpretiveTrace: InterpretiveStep[]
}

interface Frame {
  id: string
  pattern: Pattern
  meaning: string           // Human-readable interpretation
  confidence: number        // 0-1: certainty
  alternatives: Frame[]     // Other possible interpretations
  assumptions: Assumption[]
}

interface Assumption {
  id: string
  description: string
  source: "model" | "memory" | "context" | "default"
  challengeable: boolean
}

interface Ambiguity {
  patterns: Pattern[]
  possibleMeanings: string[]
  resolutionNeeded: boolean
}

interface InterpretiveStep {
  timestamp: Date
  from: string
  to: string
  reason: string
}
```

**Critical Rule**: Every interpretation must declare its assumptions.

```
FRAME: "User is stressed about deadline"
  └── ASSUMPTION: Calendar shows deadline tomorrow (source: memory)
  └── ASSUMPTION: User speech rate increased (source: model)
  └── ASSUMPTION: "Stressed" is negative (source: default)
        └── CHALLENGEABLE: true
```

**Constraints**:
- ❌ No hidden assumptions
- ❌ No pretending certainty when ambiguous
- ✅ Alternative interpretations preserved
- ✅ Full interpretive trace

---

### 3.4 DECIDING — Action Selection

**Input**: Frames from INTERPRETING  
**Output**: Decision with rationale

```typescript
interface DecisionResult {
  decision: Decision
  consideredOptions: Option[]
  rejectedOptions: RejectedOption[]
  rationale: string
}

interface Decision {
  id: string
  type: DecisionType
  frames: Frame[]           // Supporting frames
  proposedAction: Action | null
  confidence: number
  reversible: boolean
}

type DecisionType =
  | "propose_action"        // Suggest doing something
  | "request_clarification" // Need more information
  | "defer"                 // Not enough confidence
  | "escalate"              // Human must decide
  | "do_nothing"            // Explicit inaction

interface RejectedOption {
  option: Option
  rejectionReason: string
}
```

**Decision Tree**:

```
                    ┌─────────────────────────────┐
                    │  Confidence > threshold?    │
                    └─────────────┬───────────────┘
                                  │
              ┌─────── NO ────────┴──────── YES ──────┐
              │                                       │
              ▼                                       ▼
    ┌─────────────────────┐               ┌───────────────────┐
    │ Ambiguity present?  │               │ Action reversible?│
    └──────────┬──────────┘               └─────────┬─────────┘
               │                                    │
      YES ─────┴───── NO                  YES ──────┴────── NO
       │              │                    │                │
       ▼              ▼                    ▼                ▼
  ┌─────────┐  ┌──────────┐         ┌──────────┐    ┌──────────┐
  │REQUEST_ │  │  DEFER   │         │ PROPOSE_ │    │ ESCALATE │
  │CLARIFY  │  │          │         │ ACTION   │    │          │
  └─────────┘  └──────────┘         └──────────┘    └──────────┘
```

**Constraints**:
- ❌ No action without explicit decision
- ❌ No hiding rejected alternatives
- ✅ Rationale always provided
- ✅ "Do nothing" is a valid decision

---

### 3.5 ENACTING — Execution with Feedback

**Input**: Decision from DECIDING  
**Output**: Execution result + feedback loop signal

```typescript
interface EnactResult {
  execution: Execution | null
  outcome: Outcome
  feedback: FeedbackSignal
}

interface Execution {
  actionId: string
  startedAt: Date
  completedAt: Date | null
  status: ExecutionStatus
}

type ExecutionStatus =
  | "pending_authorization"
  | "executing"
  | "completed"
  | "failed"
  | "cancelled"

interface Outcome {
  success: boolean
  result: unknown
  sideEffects: SideEffect[]
}

interface FeedbackSignal {
  type: FeedbackType
  source: "outcome" | "user" | "system"
  value: unknown
  feedsBackTo: "SCANNING" | "NOTICING" | "INTERPRETING"
}

type FeedbackType =
  | "confirmation"          // Interpretation was correct
  | "correction"            // Interpretation was wrong
  | "refinement"            // Partially correct
  | "new_information"       // Learned something new
```

**Feedback Integration**:

```
ENACTING completes
       │
       ▼
┌──────────────────────────────────────────────────────┐
│ FEEDBACK ROUTING                                     │
├──────────────────────────────────────────────────────┤
│                                                      │
│  "confirmation" ───► INTERPRETING (reinforce model)  │
│                                                      │
│  "correction" ─────► NOTICING (adjust attention)     │
│                      INTERPRETING (update model)     │
│                                                      │
│  "new_information" ► SCANNING (add to signal set)    │
│                                                      │
└──────────────────────────────────────────────────────┘
```

---

## 4. The SenseMaking Frame Object

Every complete SenseMaking cycle produces a **Frame Object**:

```typescript
interface SenseMakingFrame {
  // Identity
  id: string
  version: number
  createdAt: Date
  updatedAt: Date

  // Input
  triggerSignals: Signal[]
  contextWindow: ContextWindow

  // Processing
  noticedPatterns: Pattern[]
  interpretations: Frame[]
  decision: Decision

  // Output
  proposedAction: Action | null
  outcome: Outcome | null

  // Traceability
  interpretiveTrace: InterpretiveStep[]
  assumptions: Assumption[]
  confidenceHistory: number[]

  // Governance
  humanReviewed: boolean
  humanOverride: boolean
  feedbackReceived: FeedbackSignal[]
}
```

---

## 5. Confidence Calibration

### 5.1 Confidence Levels

| Level | Range | Meaning | Action Permission |
|-------|-------|---------|-------------------|
| **Very Low** | 0.0 - 0.3 | Highly uncertain | Request clarification only |
| **Low** | 0.3 - 0.5 | Uncertain | Propose with caveats |
| **Medium** | 0.5 - 0.7 | Reasonably confident | Propose, explain alternatives |
| **High** | 0.7 - 0.9 | Confident | Propose directly |
| **Very High** | 0.9 - 1.0 | Near certain | Propose, may auto-execute reversible |

### 5.2 Confidence Decay

Confidence decays over time without reinforcement:

$$
C(t) = C_0 \cdot e^{-\lambda t}
$$

Where:
- $C_0$ = initial confidence
- $\lambda$ = decay rate (default: 0.1 per hour for ephemeral, 0.001 for stable)
- $t$ = time elapsed

---

## 6. SenseMaking and Memory Integration

```
                    SENSEMAKING                      MEMORYOS
                    ───────────                      ────────
                         │
   SCANNING ────────────►│◄────── Retrieve context
                         │
   NOTICING ────────────►│◄────── Pattern history
                         │
   INTERPRETING ────────►│◄────── Past interpretations
                         │        Similar frames
                         │
   DECIDING ────────────►│◄────── Decision outcomes history
                         │
   ENACTING ────────────►│───────► Store frame (if significant)
                         │
                         │
                         └───────► Feedback updates memories
```

**Memory Access Rules**:
- SCANNING: Read-only (context retrieval)
- NOTICING: Read-only (pattern matching)
- INTERPRETING: Read-only (similar frames)
- DECIDING: Read-only (outcome history)
- ENACTING: Read-write (store results, update)

---

## 7. Human Intervention Points

| Stage | Intervention Type | Trigger |
|-------|-------------------|---------|
| SCANNING | Signal injection | User provides explicit input |
| NOTICING | Attention override | "Focus on X, ignore Y" |
| INTERPRETING | Frame correction | "That's not what I meant" |
| DECIDING | Decision override | "Don't do that, do this" |
| ENACTING | Execution halt | "Stop" |

```typescript
interface HumanIntervention {
  id: string
  timestamp: Date
  stage: SenseMakingStage
  type: InterventionType
  content: unknown
  appliedTo: string[]       // Frame/pattern/decision IDs
}

type InterventionType =
  | "inject"                // Add something
  | "override"              // Replace
  | "halt"                  // Stop
  | "confirm"               // Approve
  | "reject"                // Deny
```

---

## 8. Implementation Requirements

A SenseMaking implementation is compliant if:

| Requirement | Verification |
|-------------|--------------|
| Five stages implemented | All stages present and sequential |
| Interpretive trace exists | Every frame has full trace |
| Assumptions explicit | No hidden assumptions |
| Confidence calibrated | Confidence matches outcomes over time |
| Feedback loop works | Outcomes feed back to earlier stages |
| Human intervention possible | All intervention points functional |

---

## 9. JSON Schema Reference

See [schemas/sense-making-frame.json](../schemas/sense-making-frame.json) for the complete JSON Schema.

---

## 10. Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2025-12-23 | Initial draft |

---

*Previous: [02-memory-os.md](./02-memory-os.md)*  
*Next: [04-harmonia.md](./04-harmonia.md)*
