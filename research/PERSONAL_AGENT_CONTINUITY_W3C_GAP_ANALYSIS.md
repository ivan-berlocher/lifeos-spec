# Personal Agent Continuity — W3C Spec Gap Analysis v0.1

**Baseline:** 17 September 2026  
**Status:** Working research artifact — hypothesis to falsify, not a standards claim.

## Thesis

The W3C AI Agent Protocol Community Group already defines a **Personal Agent**: an agent serving an individual, operating under user authorization, accessing user data and interacting with other agents on the user's behalf — effectively the user's digital representative.

Our question starts where that definition currently stops:

> **What makes a Personal Agent remain the same Personal Agent across time, models, runtimes, devices and interaction surfaces?**

Working formulation:

> **Interoperability allows agents to communicate. Continuity allows a Personal Agent to persist.**

Candidate persistent properties: **Identity · Presence · Continuity · Authority · Capabilities**.

The goal is first to prove or disprove a standards gap. If it survives research, outputs could be: (1) a paper/problem statement, (2) a vendor-neutral Personal Agent Continuity model/spec, and (3) LifeOS OSS as reference implementation, with Lya as a product implementation.

## 1. Existing W3C foundation

The AI Agent Protocol CG aims at open protocols for discovery, identity and collaboration across the Web — foundations for an **Agentic Web**. Its scope includes inter-agent communication, identity, capability/interface/goal/state metadata, security/privacy/authorization and interoperability.

Sources:
- https://www.w3.org/groups/cg/agentprotocol/
- https://www.w3.org/community/agentprotocol/
- https://github.com/w3c-cg/ai-agent-protocol
- https://w3c-cg.github.io/ai-agent-protocol/protocol.html
- https://w3c-cg.github.io/ai-agent-protocol/use_case.html
- https://github.com/w3c-cg/ai-agent-protocol/issues
- https://www.w3.org/groups/cg/agentprotocol/calendar/

The current protocol is explicitly **Tentative** and still contains TODO sections. This is Community Group work, not a finished W3C Recommendation.

### Personal Agent

The W3C use-case document says a Personal Agent directly serves the user, acts under authorization, accesses personal data, represents user interests in agent-to-agent interactions, preserves privacy and personalizes service.

It distinguishes:
- **Personal Agent** — digital representative of the individual.
- **Service Agent** — specialized service provider.
- **Search Agent** — discovery/directory/capability matching.

Therefore **Personal Agent is not itself the missing concept**. The candidate gap is its operational continuity.

## 2. What is already covered

**Identity:** DID-based identity/authentication and cross-platform identity consistency. Strong network identity foundation, but not yet a complete answer to whether Personal Agent identity is independent of model/runtime/session/device.

**Description & Capabilities:** Agent Description documents expose information, capabilities and Natural Language / Structured Interfaces.

**Discovery:** active discovery includes `https://{domain}/.well-known/agent-descriptions`; passive discovery supports registration with search agents. This is part of the future “DNS/Google for agents” layer.

**Authorization:** W3C examples already include autonomous low-risk choices, human-in-the-loop decisions, explicit consent and lack of payment authority. Authority is therefore **partial, not absent**.

## 3. Layering hypothesis

```text
HUMAN
  ↓
PERSONAL AGENT — LYA
Identity · Presence · Continuity · Authority · Capabilities
  ↓
LifeOS
Personal-Agent continuity/runtime infrastructure
missions · checkpoints · handoffs · workers · authority · state
  ↓
Claude / GPT-Codex / Mac-local / future runtimes
  ↓
AGENTIC WEB / W3C INTEROP
Identity · Description · Discovery · Authentication · Interaction
  ↓
Search / Service / Other Agents
```

**Lya** = who remains with the user.  
**LifeOS** = what lets that Personal Agent persist and act.  
**Agentic Web standards** = how it discovers and interacts with the external agent world.

## 4. Continuity question

Sequence:

`iPhone → ChatGPT → Claude → Mac worker → Telegram → Lya app`

Interoperability asks: **can these systems communicate?**  
Continuity asks: **is it still the same Lya?**

Chat history alone is insufficient. Safe resumption may require principal identity, mission, plan/checkpoint, completed/failed/pending/in-flight work, decisions, outcomes, authority state, capability bindings, execution ownership, pending human confirmations and next valid action.

## 5. Memory is not Continuity

The W3C **AI Agent Memory Interoperability Community Group** works on portable memory across vendors/models/frameworks/tools:
- https://www.w3.org/groups/cg/ai-agent-memory-interop/
- https://www.w3.org/community/ai-agent-memory-interop/

Its published scope explicitly places **agent runtime semantics out of scope**.

Therefore:

> **Memory is evidence and context for continuity; memory alone is not continuity.**

An agent can recover every memory and still not know which mission is active, what already happened, who owns execution, whether authority survived handoff, or what to do next.

Adjacent assurance work also exists in the **Agent Declaration and Assurance CG**:
- https://www.w3.org/groups/cg/adacg/

A Continuity model should reference adjacent runtime-attestation work rather than redefine it.

## 6. Evidence of nearby gaps in the Agent Protocol repository

These are open proposals, **not adopted W3C decisions**.

- **Issue #36 — Memory, accumulated state, provenance:** https://github.com/w3c-cg/ai-agent-protocol/issues/36  
  Explicitly raises accumulated memory/state/provenance across sessions.

- **Issue #30 — Behavioral consistency across context rotation:** https://github.com/w3c-cg/ai-agent-protocol/issues/30  
  Distinguishes authentication identity from behavioral identity; raises context compaction, session lifecycle and authorization survival.

- **Issue #44 — Verifier obligations/runtime evidence:** https://github.com/w3c-cg/ai-agent-protocol/issues/44  
  Notes that identifier control does not prove runtime identity or authorization scope.

- **Issue #34 — Evidentiary provenance:** https://github.com/w3c-cg/ai-agent-protocol/issues/34  
  Addresses trustworthy records of what an agent received, its authority and conclusions. But audit history is not current operational state.

Useful distinction: `Agent identifier ≠ Executing runtime ≠ Authorized operation`.

## 7. Initial gap matrix

| Property | Current coverage | Hypothesis to test |
|---|---|---|
| Identity | Partial/strong | Persistent Personal Agent identity independent of runtime? |
| Presence | Limited | Which incarnation is active/reachable/responsible now? |
| Continuity | Partial discussions | How does ongoing work survive runtime/model/device changes? |
| Authority | Partial | Does mandate survive handoff; how narrowed/revoked/delegated? |
| Capabilities | Strong declaration | Which capabilities are bound now under which authority/runtime? |
| Memory | Adjacent W3C CG | How referenced without equating memory and operational state? |
| Runtime assurance | Adjacent | How bind new executor to persistent Personal Agent? |
| Mission/checkpoint | Gap hypothesis | Portable Goal → Plan → Work → Checkpoint → Outcome? |
| Handoff | Gap hypothesis | How does B prove legitimate continuation from A? |
| Execution ownership | Gap hypothesis | Who owns work now; how prevent duplicate external actions? |

“Gap hypothesis” means research target, not proof that no existing solution exists.

## 8. Proposed invariants and primitives

```text
Personal Agent ≠ model
Personal Agent ≠ chat/session
Personal Agent ≠ process/runtime
Personal Agent ≠ device/application
Personal Agent ≠ worker
Personal Agent ≠ memory database
```

Candidate model:

```text
PersonalAgent
├── persistent_identity
├── principal
├── presence
├── continuity_state
├── authority_state
├── capability_bindings
└── runtime_incarnations[]
```

Candidate primitives to investigate: **ContinuityContext, Mission, Checkpoint, Handoff, Mandate/AuthorityGrant, CapabilityBinding, Presence, RuntimeInstance, WorkOutcome, ExecutionLease**.

Decisive test:

> **If this information is lost when the Personal Agent changes runtime, can it still meaningfully and safely be considered the same Personal Agent continuing the same work?**

Second test:

> **Must two independent implementations exchange it to interoperate safely?**

Only if both tests justify it should something enter a standard.

## 9. Falsification scenarios

1. **Model switch:** Claude → GPT during a multi-day mission.
2. **Device handoff:** iPhone delegates local work to Mac.
3. **Context compaction:** runtime loses conversational context but must preserve authority/constraints.
4. **Runtime failure:** worker dies after an external action but before acknowledgement; replacement must avoid duplication.
5. **Revocation:** user revokes authority while workers are active.
6. **Multi-surface presence:** ChatGPT, Telegram, native app and Mac simultaneously represent one Personal Agent.

These scenarios should become conformance tests before a spec is proposed.

## 10. Open source / spec / commercial split

### Open source

**LifeOS OSS** as reference Personal-Agent runtime: identity binding, presence registry, continuity store, mission/checkpoint model, handoff, mandate engine, capability bindings, runtime adapters and Agentic-Web adapters.

### Standardizable

Do **not** standardize “LifeOS”. A neutral work item could be **Personal Agent Continuity**, limited to the minimum interoperable contract between persistent Personal Agent and temporary runtime incarnation.

Likely non-goals: memory storage, generic discovery, tool routing, DID methods, runtime attestation, UI and orchestration algorithms.

### Commercial

The standard can be open and LifeOS can be OSS while commercial value lives in managed continuity sync, secure state, authority/secrets, persistent workers, recovery, multi-device presence, observability, enterprise controls/SLA, connectors — and **Lya as the finished Personal Agent experience**.

```text
Personal Agent Continuity — open model/spec
        ↓
LifeOS OSS — reference implementation
        ↓
LifeOS managed infrastructure
        ↓
Lya — commercial Personal Agent
```

## 11. Paper framing

Working title:

**Personal Agent Continuity: Identity, Presence and Authority Across Models, Runtimes and Devices**

Alternative:

**When Is It Still the Same Personal Agent? A Continuity Model for Cross-Runtime AI Agents**

Core claim to test:

> Existing agent interoperability work increasingly specifies how agents identify, discover, authenticate and communicate. Memory interoperability addresses portable stored memory. A complementary problem remains for Personal Agents that must preserve identity, operational state, authority and capability bindings while execution migrates across model sessions, runtime processes, devices and surfaces.

## 12. Research plan

1. Build a clause-by-clause matrix of current W3C Agent Protocol material against Identity / Presence / Continuity / Authority / Capabilities.
2. Map the Memory Interoperability CG and ADACG to avoid duplication.
3. Search adjacent standards only after the W3C-only gap is explicit.
4. Turn the six falsification scenarios into interoperability requirements.
5. Remove every primitive that is merely a LifeOS implementation detail.
6. Draft a minimal **Continuity Contract v0.1**.
7. Implement two genuinely independent runtime adapters and demonstrate handoff.
8. Only then decide whether the result is paper-only, OSS convention, Community Group contribution, or candidate specification.

## 13. Current conclusion

The strongest opportunity is **not** “another agent protocol” and not “another memory format”.

It is the possible missing layer between:

```text
Agentic Web interoperability
        ↓
Personal Agent definition
        ↓
        ?
        ↓
Persistent cross-runtime Personal Agent
```

The working name for that layer is **Personal Agent Continuity**.

The project should proceed skeptically: prove the gap first, standardize only the interoperable minimum, open-source the reference implementation, and commercialize the managed infrastructure and user experience around it.
