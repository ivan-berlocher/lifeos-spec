# Intent to Action: A Complete Trace

> **"Show me one complete path through the system."**

This document traces a single human intention from emergence to resolution, through every layer of LifeOS.

---

## The Scenario

**Context**: Sunday evening, 21:47. The human has been working intensely for three days on a project deadline.

**The intention emerges**:
> "I should send that email to Marc about the collaboration"

---

## Phase 1: Presence Detection

The Kernel's Presence module captures the current state:

```yaml
presence_snapshot:
  timestamp: "2024-01-14T21:47:32Z"
  
  physiological:
    estimated_fatigue: 0.73        # High (3 days intense work)
    time_since_last_break: "4h12m"
    circadian_phase: "late_evening"
  
  cognitive:
    attention_fragmentation: 0.61  # Moderate-high
    context_switches_last_hour: 12
    open_loops_detected: 7
  
  emotional:
    stress_indicators: 0.58
    dominant_tone: "driven_but_depleted"
  
  environmental:
    day_type: "weekend"
    social_commitments: []
```

**Presence verdict**: The human is present but compromised — fatigue is high, it's late Sunday, and cognitive resources are depleted.

---

## Phase 2: Memory Retrieval

The intention triggers memory queries across all three tiers:

### Short-Term Memory (STM)
```yaml
recent_context:
  - "Worked on ProjectX presentation (last 6 hours)"
  - "Mentioned Marc in voice note yesterday"
  - "Email draft started Friday, abandoned"
```

### Mid-Term Memory (MTM)
```yaml
working_patterns:
  - "Emails sent after 21h have 2.3x more typos"
  - "Sunday emails to collaborators: 67% require follow-up corrections"
  - "Marc relationship: professional-warm, prefers structured communication"
```

### Long-Term Memory (LTM)
```yaml
historical_wisdom:
  - "Pattern: Late-night 'urgent' sends often regretted"
  - "Marc collaboration history: 3 projects, high-trust relationship"
  - "Self-knowledge: Post-deadline emails often contain emotional leakage"
```

---

## Phase 3: SenseMaking

The SenseMaking module synthesizes a coherent frame:

```yaml
sense_making_frame:
  id: "smf-20240114-214732"
  
  situation_summary: |
    Human wants to send professional email to valued collaborator.
    Current state: cognitively depleted after 3-day sprint.
    Timing: Late Sunday evening.
    
  tensions_detected:
    - type: "urgency_vs_quality"
      description: "Felt urgency may not reflect actual deadline"
      
    - type: "intention_vs_capacity"  
      description: "Desire to act exceeds current capacity for quality"
      
    - type: "relationship_protection"
      description: "Marc deserves considered communication"
  
  interpretive_hypotheses:
    - hypothesis: "Genuine time-sensitive need"
      confidence: 0.15
      
    - hypothesis: "Displacement activity (avoiding rest)"
      confidence: 0.45
      
    - hypothesis: "Anxiety-driven closure-seeking"
      confidence: 0.40
  
  insight: |
    The intention is real, but the timing serves anxiety rather than 
    the relationship or the project. The email can wait. The human cannot
    sustain this pace without cost.
```

---

## Phase 4: Harmonia Orchestration

Harmonia receives the SenseMaking frame and deliberates:

```yaml
harmonia_deliberation:
  input: "smf-20240114-214732"
  
  option_space:
    - action: "SEND_NOW"
      alignment_score: 0.23
      risk: "Quality compromise, relationship noise"
      
    - action: "DRAFT_AND_SCHEDULE"
      alignment_score: 0.71
      benefit: "Captures intention, allows morning review"
      
    - action: "DEFER_ENTIRELY"
      alignment_score: 0.82
      benefit: "Honors fatigue, trusts future self"
      
    - action: "SURFACE_INSIGHT"
      alignment_score: 0.89
      benefit: "Addresses root pattern, not just symptom"
  
  selected_response: "SURFACE_INSIGHT"
  
  reasoning: |
    The highest-alignment action is not about the email at all.
    It's about helping the human see the pattern they're caught in.
```

---

## Phase 5: Double Lock Protocol

Before any external action, the Double Lock engages:

### Lock 1: Technical Gate
```yaml
technical_review:
  proposed_action: "Surface insight to human"
  action_type: "INTERNAL_DIALOGUE"
  external_effects: false
  reversible: true
  
  gate_status: PASS
  reason: "No external action. Information only."
```

### Lock 2: Human Confirmation
```yaml
human_confirmation:
  required: false  # Internal dialogue doesn't require confirmation
  
  # But Harmonia chooses to frame it as a question anyway:
  presentation_mode: "GENTLE_INQUIRY"
```

---

## Phase 6: The Interaction

What the human actually sees:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  You've been working for three days straight.                   │
│  It's almost 10 PM on Sunday.                                   │
│                                                                 │
│  The email to Marc — is it truly urgent,                        │
│  or is it the part of you that can't stop?                      │
│                                                                 │
│  I can:                                                         │
│   → Draft it now and schedule for tomorrow morning              │
│   → Remind you tomorrow when you're fresh                       │
│   → Send it now if you decide it's genuinely time-sensitive     │
│                                                                 │
│  What feels right?                                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Phase 7: Human Response

The human pauses. Reads it again.

> "...schedule it for tomorrow."

---

## Phase 8: Action Execution

Now Harmonia proceeds with the delegated action:

```yaml
action_execution:
  type: "DRAFT_AND_SCHEDULE"
  
  double_lock_check:
    lock_1_technical: PASS  # Scheduling is reversible
    lock_2_human: PASS      # Explicit human choice
  
  execution:
    - draft_email: "Retrieved from Friday's abandoned draft"
    - schedule_time: "2024-01-15T09:00:00Z"
    - notification: "Set reminder to review before send"
  
  status: COMPLETE
```

---

## Phase 9: Memory Consolidation

The interaction becomes memory:

```yaml
memory_consolidation:
  to_stm:
    - "Email to Marc scheduled for tomorrow morning"
    - "Human chose rest over immediate action"
  
  to_mtm:
    - pattern: "Late Sunday + fatigue → defer non-urgent externals"
      confidence_boost: +0.12
  
  to_ltm:
    - insight: "Human developing self-awareness around driven-depleted state"
      significance: 0.67
```

---

## What This Example Demonstrates

| Principle | How it appeared |
|-----------|-----------------|
| **Presence before action** | Fatigue detected before any recommendation |
| **Memory as wisdom** | Past patterns informed interpretation |
| **SenseMaking as synthesis** | Multiple hypotheses held, not rushed to conclusion |
| **Harmonia as orchestrator** | Chose insight over task completion |
| **Double Lock** | Even internal dialogue passed through gates |
| **Human sovereignty** | System offered options, human chose |
| **Action as last resort** | The "best" action was almost no action |

---

## The Counter-Scenario: What a Normal AI Would Do

```
User: "Help me write an email to Marc about the collaboration"

AI: "Sure! Here's a draft:

     Dear Marc,
     
     I wanted to reach out about our collaboration...
     
     [Sends immediately or offers to send]"
```

No presence check. No memory wisdom. No pattern recognition. No protection.

Just execution.

---

## Closing Reflection

The goal of LifeOS is not to do more.

It's to help humans **do what they actually mean** — 
which sometimes means doing nothing at all.

```
"The right action is the one that needs no correction."
```

---

*This trace is fictional but architecturally accurate. Every component referenced exists in the LifeOS specification.*
