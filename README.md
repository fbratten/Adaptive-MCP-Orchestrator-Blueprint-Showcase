# Intelligent MCP Orchestrator

> A Cognitive Dispatcher that routes tasks between internal MCP tools and external AI providers (Claude-first), with self-learning capabilities.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status](https://img.shields.io/badge/status-complete-green)]()
[![Tests](https://img.shields.io/badge/tests-960%2B%20passing-brightgreen)]()
[![Modules](https://img.shields.io/badge/modules-16-blue)]()
[![Dashboards](https://img.shields.io/badge/grafana-4%20dashboards-orange)]()

---

## Overview

The **Intelligent MCP Orchestrator** is a platform that intelligently routes tasks to the best available tool or AI provider. Unlike standard AI wrappers, this system:

- Acts as a **"Cognitive Dispatcher"** that selects the optimal tool for each task
- Remembers what worked and improves over time (**Learning Layer**)
- Provides full **observability and control** via dashboard and metrics
- Supports **hot-swappable configuration** without restarts

### Key Capabilities

| Capability | Description |
|------------|-------------|
| **Capability-Based Routing** | Match tasks to tools via capability tags, not exact names |
| **Claude-First Bias** | Prefer Anthropic Claude for external calls (D-M1-003) |
| **Internal Tools First** | Try local MCP tools before external providers (D-M1-004) |
| **Retry & Fallback** | Exponential backoff with automatic fallback chains |
| **Learning Layer** | Vector DB + Graph DB for semantic similarity and relationships |
| **Full Observability** | Prometheus metrics, Grafana dashboards, structured logs |

---

## Architecture

### The 6-Module System

```
┌─────────────────────────────────────────────────────────────┐
│                    MCP Orchestrator Platform                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐       │
│   │   M1    │  │   M2    │  │   M3    │  │   M4    │       │
│   │  Core   │◄─┤ Config  │  │  Logs   │  │Dashboard│       │
│   │Orchestr.│  │ Engine  │  │Observab.│  │   API   │       │
│   └────┬────┘  └─────────┘  └────┬────┘  └────┬────┘       │
│        │                         │            │             │
│        │    ┌─────────────┐      │            │             │
│        └───►│     M5      │◄─────┘            │             │
│             │  Learning   │◄──────────────────┘             │
│             │   Layer     │                                 │
│             └──────┬──────┘                                 │
│                    │                                        │
│             ┌──────▼──────┐                                 │
│             │     M6      │                                 │
│             │Infrastructure│                                │
│             │   (Docker)  │                                 │
│             └─────────────┘                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

| Module | Purpose | Tests | Status |
|--------|---------|-------|--------|
| **M1** | Core Orchestrator - Task routing, tool selection | 89 | ✅ Complete |
| **M2** | Configuration Engine - YAML config, API keys | 106 | ✅ Complete |
| **M3** | Logging & Observability - JSON logs, metrics | 110 | ✅ Complete |
| **M4** | Dashboard API - FastAPI, SSE streaming | 136 | ✅ Complete |
| **M5** | Learning Layer - LanceDB vectors, Neo4j graphs | 107 | ✅ Complete |
| **M6** | Infrastructure - Docker Compose, Prometheus | Docker | ✅ Complete |

### Extended Modules

| Module | Purpose | Tests | Status |
|--------|---------|-------|--------|
| **projects02** | AI Assistant Integration - Multi-provider routing | 189 | ✅ Complete |
| **projects03** | MCP Meta-Router - External MCP server routing | 226 | ✅ Complete |

#### projects02: AI Assistant Integration

5 modules for integrating AI assistants (OpenAI, Gemini, Claude) with the orchestrator:

- **p02-1-openai-adapter** - OpenAI GPT-4 provider adapter
- **p02-2-gemini-adapter** - Google Gemini provider adapter
- **p02-3-assistant-bridge** - Main integration bridge
- **p02-4-provider-monitor** - Health monitoring & feedback
- **p02-5-assistant-config** - Configuration schemas

#### projects03: MCP Meta-Router

5 modules for routing tasks to external MCP servers:

- **p03-1-mcp-client** - MCP client with stdio transport
- **p03-2-mcp-discovery** - Server and tool discovery
- **p03-3-mcp-registry** - Unified tool registry
- **p03-4-mcp-router** - Main meta-router
- **p03-5-mcp-config** - Configuration schemas

Supported MCP Servers: browser-mcp, paaf, filesystem, research-agent, and more.

---

## Dashboard & Observability (New)

Full visibility into system behavior with multiple observation points:

### Grafana Dashboards

| Dashboard | Purpose |
|-----------|---------|
| **MCP Overview** | System health, request rates, latency metrics |
| **M5 Learning Layer** | LanceDB embeddings, Neo4j nodes, relationships |
| **API Endpoints** | Per-endpoint request metrics |
| **Logs Explorer** | Full-text log search via Loki |

### Learning Layer Visibility

Real-time monitoring of the self-learning system:

- **Vector Store (LanceDB):** Embeddings count, by-tool breakdown, by-outcome stats
- **Graph Store (Neo4j):** Task/Tool/Capability nodes, relationships
- **REST API:** `/learning/stats` and `/learning/health` endpoints

### Prometheus Metrics

15+ custom metrics including:
- `mcp_learning_healthy` - Overall health status
- `mcp_learning_vector_store_available` - LanceDB availability
- `mcp_learning_graph_store_available` - Neo4j availability
- `mcp_learning_vector_embeddings_total` - Total embeddings
- `mcp_learning_graph_relationships_total` - Graph relationships

---

## Integration Ready

Connect your systems to the orchestrator:

### SPINE Integration

Works with SPINE multi-agent framework for Plan→Execute→Verify→Commit workflows.

### MCP Server Ecosystem

Routes to any MCP-compatible server: browser-mcp, paaf, research-agent, filesystem, and custom servers.

### REST API Gateway

All capabilities accessible via REST API with OpenAPI documentation at `/docs`.

---

## How It Works

### Task Execution Flow

```
User Request                    Tool Selection                  Execution
    │                               │                              │
    ▼                               ▼                              ▼
┌─────────┐    ┌─────────────────────────────────────┐    ┌─────────────┐
│  Task   │───►│  1. Parse capabilities required     │───►│  Execute    │
│ "Write  │    │  2. Find matching tools             │    │  via best   │
│  code"  │    │  3. Score by capability match       │    │  provider   │
│         │    │  4. Apply learning boost (M5)       │    │             │
│         │    │  5. Select highest scoring tool     │    │             │
└─────────┘    └─────────────────────────────────────┘    └─────────────┘
                                                                 │
                                                                 ▼
                                                          ┌─────────────┐
                                                          │  Response   │
                                                          │  + Metrics  │
                                                          │  + Feedback │
                                                          └─────────────┘
```

### Example: Real API Call

```python
from mcp_core.orchestrator import Orchestrator

orchestrator = Orchestrator(auto_configure=True)
orchestrator.configure_anthropic(api_key="sk-...")

# Ask for code generation - automatically routes to Claude
response = await orchestrator.execute_task(
    task="Write a Python function to calculate Fibonacci numbers",
    capabilities=["code_generation"],
)

print(f"Tool used: {response.tool_used}")      # claude_code_generation
print(f"Provider: {response.provider}")         # anthropic
print(f"Tokens: {response.tokens_used}")        # TokenUsage(in=23, out=287)
print(f"Response: {response.result['response']}")
```

---

## Available Tools

| Tool ID | Provider | Capabilities | Description |
|---------|----------|--------------|-------------|
| `internal_echo` | internal | testing, echo | Echo back input for testing |
| `internal_calculator` | internal | calculation, math | Basic arithmetic |
| `claude_code_analysis` | anthropic | code_analysis, code_review | Analyze code quality |
| `claude_code_generation` | anthropic | code_generation, coding | Generate code from descriptions |
| `claude_text_analysis` | anthropic | text_summarization, text_analysis | Analyze text content |
| `claude_general` | anthropic | general, chat, reasoning | General-purpose assistant |

---

## Tech Stack

- **Language:** Python 3.11+
- **API Framework:** FastAPI
- **Vector DB:** LanceDB (semantic similarity, local)
- **Graph DB:** Neo4j (tool-task relationships)
- **Embeddings:** MiniLM/SBERT (~384 dimensions)
- **Deployment:** Docker & Docker Compose

---

## Quick Start

### 1. Clone and Setup

```bash
git clone https://github.com/fbratten/ai-projects-and-management-example.git
cd "Adaptive MCP Orchestrator Blueprint"
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 2. Configure API Keys

```bash
# Create .env file
cat > .env << EOF
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...
EOF
```

### 3. Run with Docker

```bash
docker-compose up -d
```

### 4. Access Services

| Service | URL | Purpose |
|---------|-----|---------|
| Dashboard | http://localhost:8080 | API docs, health, tools |
| Prometheus | http://localhost:9090 | Metrics queries |
| Grafana | http://localhost:3000 | Dashboards (admin/admin) |
| Loki | http://localhost:3100 | Log aggregation |
| Neo4j | http://localhost:7474 | Graph database browser |
| Learning Stats | http://localhost:8080/learning/stats | Learning layer API |

---

## Key Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| D-M1-003 | Claude-First (1.5x weight) | Tool calling maturity |
| D-M1-004 | Internal Tools First | Lower latency, no costs |
| D-M3-001 | JSON Structured Logging | Machine-parseable |
| D-M5-001 | LanceDB + Neo4j Dual DB | Best tool for each job |
| D-M6-001 | Docker Compose | Simplicity over K8s |

---

## Project Structure

```
Adaptive MCP Orchestrator Blueprint/
├── projects/
│   ├── p1-mcp-core/        # M1: Core Orchestrator
│   ├── p2-config-engine/   # M2: Configuration
│   ├── p3-observability/   # M3: Logging
│   ├── p4-dashboard/       # M4: Dashboard API
│   ├── p5-learning/        # M5: Learning Layer
│   └── p6-infrastructure/  # M6: Docker
├── projects02/             # AI Assistant Integration (5 modules)
├── projects03/             # MCP Meta-Router (5 modules)
├── docs/
│   ├── architecture/       # Design specs
│   └── reviews/            # Code reviews
├── ai-memory/              # AI context files
├── CLAUDE.md               # Project instructions
├── NEXT.md                 # Task tracking
└── docker-compose.yml      # Infrastructure
```

## Interactive Demos

The project includes interactive demos for exploring how the orchestrator works:

- **[Overview](docs/overview.md)** - Architectural summary and key capabilities
- **[Tool Selection Simulator](demos/)** - See how capability-based routing works
- **[Provider Selector Lab](demos/lab/provider-selector.html)** - Interactive multi-provider selection
- **[MCP Router Lab](demos/lab/mcp-router.html)** - MCP server routing simulation
- **[How It Works](demos/explain/)** - Visual explanations of core concepts

---

## License

MIT License - See [LICENSE](LICENSE) for details.

---

## Related

- [SPINE Framework](https://github.com/fbratten/spine-showcase) - Multi-agent orchestration
- [MCP Protocol](https://modelcontextprotocol.io) - Model Context Protocol
