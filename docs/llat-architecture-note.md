# Architecture Note: The Intent‑to‑Action Web Stack

> **LLAT and the missing layer between documents and agents**

**Status**: Draft (non‑W3C, TAG‑oriented)  
**Last Updated**: 2025‑12‑24

**Abstract**: This note proposes a layered model for an Intent‑to‑Action Web and introduces **Language‑Level Action Transport (LLAT)** as a thin action semantics layer above existing agent/tool protocols. It is intended to support architectural discussion (for example within the W3C TAG) and is **not** a formal standards‑track document.

The note focuses on three elements:

- A layered model from **L0 to L8** for the Intent‑to‑Action Web
- A distinction between **L6 (Agent / Tool Invocation)**, **L7 (LLAT)** and **L8 (Presence & Governance)**
- A short motivation: **"Why now?"** — why agentic behavior is not well‑captured by a document‑centric Web model

---

## 1. Layered Model (L0 → L8)

The classic Web stack carries us from **IP packets** to **hypertext documents and data**.  
The Intent‑to‑Action Web requires one more step: a way to carry **human‑authorized actions** with the same rigor.

The following conceptual stack extends the familiar model up to **L8**:

```text
┌────────────────────────────────────────────────────────────────────────────┐
│ L8: PRESENCE & GOVERNANCE (Human Level)                                   │
│     • Human consent, norms, responsibilities                               │
│     • Double‑lock execution policies                                      │
├────────────────────────────────────────────────────────────────────────────┤
│ L7: LANGUAGE‑LEVEL ACTION TRANSPORT (LLAT)                                │
│     • Intents, action proposals, traces                                   │
│     • Human‑legible + machine‑checkable representations                   │
│     • Independent of any single vendor protocol                           │
├────────────────────────────────────────────────────────────────────────────┤
│ L6: AGENT / TOOL INVOCATION                                               │
│     • Protocols for calling tools/agents (e.g. MCP, plugins, functions)   │
│     • Capability manifests, tool schemas                                  │
├────────────────────────────────────────────────────────────────────────────┤
│ L5: PERSONAL CONTEXT & MEMORY                                             │
│     • User‑centric knowledge graph                                        │
│     • Patterns, preferences, local reasoning state                        │
├────────────────────────────────────────────────────────────────────────────┤
│ L4: PODS & IDENTITY                                                       │
│     • Solid pods, WebID‑OIDC, ACL/ACP                                     │
│     • Where personal data and profiles live                               │
├────────────────────────────────────────────────────────────────────────────┤
│ L3: LINKED DATA WEB                                                       │
│     • RDF, JSON‑LD, vocabularies                                          │
│     • URIs as global identifiers                                          │
├────────────────────────────────────────────────────────────────────────────┤
│ L2: REPRESENTATION & APPLICATION PROTOCOLS                                │
│     • HTTP(S), WebSocket, gRPC‑Web                                        │
│     • HTML, JSON, binary formats                                          │
├────────────────────────────────────────────────────────────────────────────┤
│ L1: TRANSPORT & SECURITY                                                  │
│     • TCP/UDP, TLS, QUIC                                                 │
├────────────────────────────────────────────────────────────────────────────┤
│ L0: NETWORK & HARDWARE                                                    │
│     • Physical network, devices, sensors                                  │
└────────────────────────────────────────────────────────────────────────────┘
```

A corresponding graphical diagram is available in this repository:

- `assets/llat-layers.svg` — source diagram (vector)
- `assets/llat-layers.png` — export for slide decks and PDFs

This note focuses on the top three layers.

---

## 2. L6 / L7 / L8: Distinct Roles

### 2.1 L6 — Agent / Tool Invocation

**L6** is where concrete **agent and tool protocols** live.

Examples include:

- Model‑centric tool protocols (e.g. **MCP**)
- Application/plugin systems (browser or platform extensions)
- Plain HTTP / RPC APIs wrapped as tools

Typical responsibilities:

- Describing available capabilities ("tools")
- Advertising schemas for arguments and results
- Managing connections, authentication, timeouts

L6 is about **"how to call things"**.

### 2.2 L7 — LLAT: Language‑Level Action Transport

**L7 (LLAT)** sits *above* any particular agent/tool protocol and answers a different question:

> *"What exactly is the human trying to do, and what sequence of actions is being proposed or executed on their behalf?"*

LLAT standardizes the minimal vocabulary needed to represent:

- **Intents** — structured descriptions of what the human wants
- **ActionProposals** — bounded, typed suggestions for changing the world
- **ExecutionTraces** — append‑only records of what actually happened, across tools and vendors

LLAT is explicitly **human‑centred**:

- Representations are intended to be **readable by humans** and **processable by machines**.
- Traces are designed to be **portable** between implementations and auditable by the user (and, where appropriate, regulators).

L7 is about **"what we are doing"**, not **"how we call the tool"**.

### 2.3 L8 — Presence & Governance

**L8** concerns **who is responsible** and **under which norms** actions are allowed to proceed.

At this layer we find:

- Human presence and consent ("double‑lock" style mechanisms)
- Personal norms and values (e.g. reversibility, safety thresholds)
- Legal, organizational, and societal constraints

L8 is not a protocol in the narrow sense, but its policies must still be **capturable and portable**:

- As machine‑checkable policies (e.g. authorization rules, emergency escape hatches)
- As part of the **context** attached to LLAT intents and traces

In short:

- **L6** — *Invocation layer* (tools and agents)
- **L7** — *Action semantics layer* (LLAT)
- **L8** — *Responsibility and governance layer* (humans and norms)

---

## 3. Why Now? (Agents ≠ Documents)

The original Web was built around **documents** and later **data**:

- URIs identify resources
- HTTP retrieves or modifies representations
- HTML and related formats describe hypertext and hypermedia

Modern "AI agents" introduce something qualitatively different:

- They **propose and execute actions** across many systems at once
- They can act with **partial autonomy** based on statistical models
- They operate on top of a growing ecosystem of **tool protocols** (MCP, plugins, APIs)

Treating these agents as if they were just another kind of "document" or "web app" is insufficient:

1. **Authority and responsibility**
   - A web page can misinform; an agent can **move money, delete data, alter records**.
   - We need a shared representation of **who authorized what, under which conditions**.

2. **Traceability and audit**
   - Logs today are mostly implementation‑specific.
   - Regulators, organizations, and end‑users need **portable traces**: the same action history should make sense across implementations and vendors.

3. **Interoperability of agent ecosystems**
   - Multiple vendors will provide agents, tools, and runtimes.
   - Without a shared **action layer**, users are locked into specific stacks, and it is hard to reason about end‑to‑end behavior.

4. **Human‑centred governance**
   - The Web community has deep experience designing **human‑facing protocols**.
   - That experience is needed now at the layer where **human intent meets machine action**.

A small, well‑designed **Language‑Level Action Transport** layer (LLAT) offers:

- A **neutral meeting point** between different agent/tool protocols
- A way to embed **human consent and norms** *into* the technical architecture
- A basis for future W3C/IETF work on **safety, accountability, and interoperability** in agentic systems

---

## Annex A — Minimal LLAT JSON‑LD Example (Non‑normative)

This annex shows an **illustrative** LLAT representation in JSON‑LD for a single, simple flow:

1. One **Intent**
2. One **ActionProposal**
3. One **ExecutionTrace** referencing both

The concrete vocabulary (`llat:Intent`, `llat:ActionProposal`, etc.) is provisional.

```jsonld
{
  "@context": {
    "llat": "https://example.org/ns/llat#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "actor": { "@id": "llat:actor", "@type": "@id" },
    "target": { "@id": "llat:target", "@type": "@id" },
    "proposes": { "@id": "llat:proposes", "@type": "@id" },
    "realizes": { "@id": "llat:realizes", "@type": "@id" },
    "hasStep": { "@id": "llat:hasStep", "@type": "@id" },
    "timestamp": { "@id": "llat:timestamp", "@type": "xsd:dateTime" }
  },
  "@graph": [
    {
      "@id": "intent:send-email-marc-1",
      "@type": "llat:Intent",
      "actor": "https://user.example.org/profile#me",
      "llat:surfaceForm": "I should send that email to Marc about the collaboration.",
      "llat:goal": "Send a concise, professional email to Marc proposing a follow-up call.",
      "llat:urgency": "medium",
      "llat:reversibility": "high"
    },
    {
      "@id": "proposal:send-email-marc-1",
      "@type": "llat:ActionProposal",
      "proposes": "intent:send-email-marc-1",
      "llat:actionType": "llat:SendEmail",
      "actor": "https://agent.example.org/agents/calendar-email#v1",
      "target": "mailto:marc@example.org",
      "llat:subject": "Follow-up on the collaboration",
      "llat:reversible": true,
      "llat:rationale": "User expressed desire to follow up. Timing is deferred to next morning to reduce fatigue-related errors.",
      "llat:status": "llat:AwaitingAuthorization"
    },
    {
      "@id": "trace:send-email-marc-1",
      "@type": "llat:ExecutionTrace",
      "realizes": "proposal:send-email-marc-1",
      "timestamp": "2024-01-15T08:55:00Z",
      "llat:authorization": {
        "@type": "llat:ExplicitAuthorization",
        "actor": "https://user.example.org/profile#me",
        "timestamp": "2024-01-15T08:54:30Z"
      },
      "hasStep": [
        {
          "@id": "step:compose-draft-1",
          "@type": "llat:ExecutionStep",
          "llat:viaTool": "https://agent.example.org/tools/email-draft#v1",
          "timestamp": "2024-01-15T08:53:10Z",
          "llat:result": "Draft prepared"
        },
        {
          "@id": "step:schedule-send-1",
          "@type": "llat:ExecutionStep",
          "llat:viaTool": "https://agent.example.org/tools/email-scheduler#v1",
          "timestamp": "2024-01-15T08:55:00Z",
          "llat:result": "Email scheduled for 2024-01-15T09:00:00Z"
        }
      ]
    }
  ]
}
```

- The **Intent** captures the human’s goal in natural language plus a normalized `llat:goal`.
- The **ActionProposal** specifies a concrete, reversible action (`llat:SendEmail`) scoped to a particular target.
- The **ExecutionTrace** records when authorization was given and which concrete steps (tools) realized the proposal.

This is only one of many possible serializations. The key point is that the **same three concepts** — *Intent*, *ActionProposal*, *ExecutionTrace* — are visible and portable, independently of which agent/tool protocol (MCP, plugins, APIs) executed the steps.

---

This draft is intentionally incomplete. Its purpose is to invite discussion around a missing layer of the Web architecture.
