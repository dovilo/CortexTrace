# CortexTrace — AI Agent Observability & Debugging Platform

![MiMo Powered](https://img.shields.io/badge/Powered%20by-MiMo%20v2.5--pro-8b5cf6?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Live-brightgreen?style=for-the-badge)

> Real-time observability for AI agents. Flame graphs, token waterfalls, cost attribution, reasoning path trees, and step-by-step replay.

**🌐 Live Demo:** [dovilo.github.io/CortexTrace](https://dovilo.github.io/CortexTrace/)

---

## What is CortexTrace?

CortexTrace is an **AI agent observability and debugging platform** powered by Xiaomi MiMo v2.5-pro. It provides deep visibility into how AI agents think, reason, and execute — solving the "black box" problem of AI agent behavior.

### The Problem

When AI agents fail, hallucinate, or produce unexpected results, developers have no visibility into *why*. Current tools show inputs and outputs but nothing in between — no reasoning chain, no cost breakdown, no decision path.

### The Solution

CortexTrace provides 6 observability tools:
1. **Reasoning Flame Graph** — Visualize time spent in each reasoning step
2. **Token Waterfall** — See token flow through each node in real-time
3. **Cost Attribution** — Track cost per step, per tool call, per agent
4. **Reasoning Path Tree** — Interactive tree of agent decision paths
5. **Error Tracking** — Capture errors with full context and reasoning state
6. **Step-by-Step Replay** — Replay any agent run from start to finish

---

## Architecture

```
┌───────────────────────────────────────────────────────────────┐
│                    CortexTrace Platform                        │
├───────────────┬───────────────┬───────────────────────────────┤
│  Trace        │  Visualize    │  Analyze                      │
│  Collector    │  Engine       │  Engine                       │
│               │               │                               │
│  ┌─────────┐  │  ┌─────────┐  │  ┌─────────────────────────┐  │
│  │OpenTelem│  │  │Flame    │  │  │  Cost Attribution       │  │
│  │etry SDK │──┼──│Graph    │──┼──│  per-step, per-model    │  │
│  │per-agent│  │  │Renderer │  │  │  token breakdown        │  │
│  └─────────┘  │  └─────────┘  │  └─────────────────────────┘  │
│               │               │                               │
│  ┌─────────┐  │  ┌─────────┐  │  ┌─────────────────────────┐  │
│  │Span     │  │  │Token    │  │  │  Error Detection        │  │
│  │Collector│──┼──│Waterfall│──┼──│  anomaly scoring        │  │
│  │auto-time│  │  │Renderer │  │  │  context capture        │  │
│  └─────────┘  │  └─────────┘  │  └─────────────────────────┘  │
├───────────────┴───────────────┴───────────────────────────────┤
│  MiMo v2.5-pro │ 128K Context │ JSON Mode │ Chain-of-Thought  │
├───────────────────────────────────────────────────────────────┤
│  Replay Engine │ Alert System │ API Gateway │ Team Sharing      │
└───────────────────────────────────────────────────────────────┘
```

---

## 6 Observability Tools

| Tool | What It Does |
|------|-------------|
| 🔥 **Flame Graph** | Visualize time spent in each reasoning step — spot bottlenecks instantly |
| 💧 **Token Waterfall** | Real-time token flow: input/output/cache tokens per node |
| 💰 **Cost Attribution** | Cost per step, per model, per tool call — never overspend again |
| 🌳 **Reasoning Tree** | Interactive decision tree — see what the agent considered and rejected |
| 🐛 **Error Tracking** | Full context capture: prompt, model state, tool responses, stack trace |
| 🔄 **Replay** | Scrub through any agent run step-by-step, inspect state at each point |

---

## Quick Start

```bash
# Clone
git clone https://github.com/dovilo/CortexTrace.git
cd CortexTrace

# Open in browser (static site)
open index.html

# Or serve locally
python3 -m http.server 8080
# → http://localhost:8080
```

No build step. No dependencies. Just open and go.

---

## SDK Integration

### Python (3 lines)

```python
from cortextrace import Tracer

tracer = Tracer("my-agent", model="mimo-v2.5-pro")

with tracer.span("reasoning") as span:
    response = mimo.chat(prompt)
    span.log_tokens(response.usage)
    span.log_cost(response.cost)
    span.log_reasoning(response.chain_of_thought)

# View at: https://cortextrace.dev/dashboard
```

### JavaScript

```javascript
import { Tracer } from 'cortextrace';

const tracer = new Tracer('my-agent', { model: 'mimo-v2.5-pro' });

const span = tracer.startSpan('reasoning');
const response = await mimo.chat(prompt);
span.logTokens(response.usage);
span.logCost(response.cost);
span.end();
```

---

## API Reference

### Get Traces

```http
GET /api/v1/traces?agent_id={id}&limit=50
Authorization: Bearer {api_key}
```

### Get Trace Detail

```http
GET /api/v1/traces/{trace_id}
```

### Response

```json
{
  "trace_id": "tr_7x9k2m",
  "agent_id": "af_r3s_7x9k2m",
  "spans": [
    {
      "name": "reasoning",
      "type": "chain_of_thought",
      "tokens": 1234,
      "cost": 0.0045,
      "latency_ms": 340,
      "children": [...]
    }
  ],
  "total_tokens": 4567,
  "total_cost": 0.0123,
  "total_latency_ms": 1820,
  "errors": []
}
```

---

## How CortexTrace Compares

| Feature | CortexTrace | LangSmith | Helicone | W&B |
|---|---|---|---|---|
| Reasoning Flame Graph | ✅ | ❌ | ❌ | ❌ |
| Token Waterfall | ✅ | ❌ | ✅ | ❌ |
| Cost Attribution | ✅ | ✅ | ✅ | ❌ |
| Reasoning Path Tree | ✅ | ❌ | ❌ | ❌ |
| Error Tracking | ✅ | ✅ | ❌ | ❌ |
| Step-by-Step Replay | ✅ | ❌ | ❌ | ❌ |
| MiMo Integration | ✅ | ❌ | ❌ | ❌ |
| Zero-Config Setup | ✅ | ❌ | ✅ | ❌ |

---

## License

MIT License — see [LICENSE](LICENSE)

---

**Built with 📡 by [dovilo](https://github.com/dovilo) · Powered by Xiaomi MiMo v2.5-pro**
