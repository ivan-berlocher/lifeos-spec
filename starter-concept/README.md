# 🚀 lifeos-starter

> Minimal reference implementation of the LifeOS specification

**Status**: Concept / RFC  
**Spec Version**: v0.1  
**License**: MIT

---

## 🎯 Purpose

`lifeos-starter` is a **minimal, runnable implementation** of the core LifeOS concepts:

1. **Kernel Loop** — Presence × Memory × Action
2. **SenseMaking** — 5-stage cognitive process
3. **MemoryOS** — STM/MTM/LTM with Solid storage
4. **Harmonia** — Intent detection and routing

It is NOT a full LifeOS implementation. It's a starting point for:
- Developers learning the spec
- Teams building custom implementations
- Testing the specification in practice

---

## 📦 What's Included

```
lifeos-starter/
├── src/
│   ├── kernel/
│   │   ├── presence.ts      # Current context detection
│   │   ├── memory.ts        # Memory layer abstraction
│   │   └── action.ts        # Action execution
│   │
│   ├── sense-making/
│   │   ├── scanner.ts       # Signal detection
│   │   ├── noticer.ts       # Pattern recognition
│   │   ├── interpreter.ts   # Frame application
│   │   ├── decider.ts       # Decision generation
│   │   └── enactor.ts       # Action proposal
│   │
│   ├── memory-os/
│   │   ├── stm.ts           # Short-term memory
│   │   ├── mtm.ts           # Medium-term memory
│   │   ├── ltm.ts           # Long-term memory
│   │   └── consolidator.ts  # Layer promotion
│   │
│   ├── solid-bridge/
│   │   ├── pod-client.ts    # Solid Pod operations
│   │   ├── rdf-mapper.ts    # LifeOS ↔ RDF conversion
│   │   └── wac-manager.ts   # Access control
│   │
│   └── index.ts             # Main entry point
│
├── examples/
│   ├── basic-loop.ts        # Minimal kernel loop
│   ├── calendar-sense.ts    # SenseMaking for calendar
│   └── memory-persist.ts    # Solid Pod persistence
│
├── tests/
│   ├── sense-making.test.ts
│   ├── memory-os.test.ts
│   └── solid-bridge.test.ts
│
├── package.json
├── tsconfig.json
└── README.md
```

---

## 🏃 Quick Start

```bash
# Clone the starter
git clone https://github.com/ivan-berlocher/lifeos-starter.git
cd lifeos-starter

# Install dependencies
pnpm install

# Run the basic example
pnpm run example:basic

# Run with a Solid Pod (requires WebID)
SOLID_WEBID=https://yourpod.example/profile/card#me \
pnpm run example:solid
```

---

## 📖 Core Concepts in Code

### The Kernel Loop

```typescript
// src/kernel/index.ts
import { Presence } from './presence';
import { Memory } from './memory';
import { Action } from './action';

export class LifeOSKernel {
  private presence: Presence;
  private memory: Memory;
  private action: Action;

  async tick(): Promise<void> {
    // P × M × A
    const currentPresence = await this.presence.sense();
    const relevantMemory = await this.memory.recall(currentPresence);
    const decision = await this.action.decide(currentPresence, relevantMemory);
    
    if (decision.requiresHumanApproval) {
      await this.action.requestApproval(decision);
    } else {
      await this.action.execute(decision);
    }
    
    // Update memory
    await this.memory.store({
      presence: currentPresence,
      decision,
      timestamp: Date.now()
    });
  }
}
```

### SenseMaking Pipeline

```typescript
// src/sense-making/pipeline.ts
export class SenseMakingPipeline {
  async process(input: RawInput): Promise<SenseMakingFrame> {
    // Stage 1: SCANNING
    const signals = await this.scanner.detectSignals(input);
    
    // Stage 2: NOTICING
    const patterns = await this.noticer.recognizePatterns(signals);
    
    // Stage 3: INTERPRETING
    const frame = await this.interpreter.applyFrame(patterns);
    
    // Stage 4: DECIDING
    const decision = await this.decider.generateOptions(frame);
    
    // Stage 5: ENACTING
    const proposal = await this.enactor.proposeAction(decision);
    
    return {
      signals,
      patterns,
      frame,
      decision,
      proposal,
      confidence: this.calculateConfidence([signals, patterns, frame])
    };
  }
}
```

### Memory Persistence with Solid

```typescript
// src/solid-bridge/pod-client.ts
import { getSolidDataset, saveSolidDatasetAt } from '@inrupt/solid-client';

export class SolidPodClient {
  async storeMemory(item: MemoryItem): Promise<void> {
    const podUrl = `${this.webId}/lifeos/memory/${item.layer.toLowerCase()}/`;
    const rdfData = this.mapper.toRDF(item);
    
    await saveSolidDatasetAt(
      `${podUrl}${item.id}.ttl`,
      rdfData,
      { fetch: this.authenticatedFetch }
    );
  }
  
  async recallMemory(query: MemoryQuery): Promise<MemoryItem[]> {
    const dataset = await getSolidDataset(
      `${this.webId}/lifeos/memory/`,
      { fetch: this.authenticatedFetch }
    );
    
    return this.mapper.fromRDF(dataset, query);
  }
}
```

---

## 🧪 Testing

```bash
# Run all tests
pnpm test

# Run specific suite
pnpm test:sense-making
pnpm test:memory
pnpm test:solid

# Test with coverage
pnpm test:coverage
```

---

## 🔧 Configuration

```typescript
// lifeos.config.ts
export default {
  kernel: {
    tickInterval: 1000,  // ms between kernel ticks
    maxConcurrentActions: 3
  },
  
  senseMaking: {
    confidenceThreshold: 0.7,
    humanInterventionThreshold: 0.5
  },
  
  memory: {
    stm: {
      maxItems: 100,
      ttlMs: 24 * 60 * 60 * 1000  // 24 hours
    },
    mtm: {
      maxItems: 1000,
      ttlMs: 30 * 24 * 60 * 60 * 1000  // 30 days
    },
    ltm: {
      consolidationInterval: 7 * 24 * 60 * 60 * 1000  // weekly
    }
  },
  
  solid: {
    webId: process.env.SOLID_WEBID,
    podProvider: 'https://solidcommunity.net'
  }
};
```

---

## 📊 Roadmap

### v0.1 (Current)
- [x] Basic kernel loop
- [x] In-memory storage
- [x] Console-based SenseMaking

### v0.2
- [ ] Solid Pod integration
- [ ] Basic WAC permissions
- [ ] Simple agent delegation

### v0.3
- [ ] Multi-agent support
- [ ] Federated memory sync
- [ ] Kill switch implementation

---

## 🤝 Contributing

See [CONTRIBUTING.md](https://github.com/ivan-berlocher/lifeos-spec/blob/main/CONTRIBUTING.md) in the spec repository.

---

## �� Related

- [LifeOS Specification](https://github.com/ivan-berlocher/lifeos-spec) — Full specification
- [Solid Project](https://solidproject.org/) — Data sovereignty protocol
- [@inrupt/solid-client](https://docs.inrupt.com/developer-tools/javascript/client-libraries/) — Solid SDK

---

## 📜 License

MIT — See [LICENSE](LICENSE)

---

*"Your AI, your data, your code."* 🌍
