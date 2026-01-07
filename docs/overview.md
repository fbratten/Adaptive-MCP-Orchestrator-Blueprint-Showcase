# Adaptive MCP Orchestrator Blueprint: Overview

> A Cognitive Dispatcher that intelligently routes tasks between internal MCP tools and external AI providers, with self-learning capabilities and full observability.

---

## The Vision

The **Adaptive MCP Orchestrator Blueprint** addresses the growing complexity of connecting Large Language Models (LLMs) to multiple external tools and data sources via the Model Context Protocol (MCP).

Rather than creating simple point-to-point connections, this blueprint implements a robust, tripartite architecture that transforms AI systems into **adaptive, secure, and scalable operational frameworks**.

---

## Three-Part Architecture

### 1. The Brain: Intelligent MCP Orchestrator

The central processing unit and intermediary intelligence layer between the LLM and tools.

**Key Capabilities:**
- **Capability-Based Routing** - Match tasks to tools via capability tags, not exact names
- **Intelligent Tool Selection** - Score tools by capability match, provider preference, and historical success
- **Learning Layer** - LanceDB vectors + Neo4j graphs improve selection over time
- **Fallback & Retry** - Exponential backoff with automatic fallback chains

### 2. The Hands: AI Assistant Integration

The client-side interface connecting AI assistants (Claude, GPT, Gemini) to the orchestrator.

**Key Capabilities:**
- **Multi-Provider Support** - OpenAI, Gemini, and Anthropic adapters
- **Health Monitoring** - Real-time provider status and feedback collection
- **Capability Negotiation** - Dynamic feature discovery and negotiation

### 3. The Nervous System: MCP Meta-Router

The infrastructure layer that aggregates and manages connections to external MCP servers.

**Key Capabilities:**
- **Server Multiplexing** - Route to browser-mcp, paaf, research-agent, and more
- **Protocol Translation** - stdio to SSE transport support
- **Dynamic Discovery** - Real-time tool discovery and registration

---

## Dashboard Visibility (New in 2026-01-07)

Full transparency into system behavior with multiple observation points:

### Grafana Dashboards

| Dashboard | Purpose |
|-----------|---------|
| **MCP Overview** | System health, request rates, latency metrics |
| **M5 Learning Layer** | LanceDB embeddings, Neo4j nodes, relationships |
| **API Endpoints** | Per-endpoint request metrics |
| **Logs Explorer** | Full-text log search and analysis |

### Learning Layer Metrics

Real-time visibility into the self-learning system:

- **Vector Store (LanceDB):** Total embeddings, embeddings by tool, embeddings by outcome
- **Graph Store (Neo4j):** Task nodes, tool nodes, capability nodes, relationships
- **Health Status:** Overall health, individual store availability

### REST API Access

Programmatic access to learning statistics:

```bash
# Full statistics
curl http://localhost:8080/learning/stats

# Health check
curl http://localhost:8080/learning/health
```

---

## Integration Points

The orchestrator is designed to connect with external systems:

### SPINE Integration

Works with the SPINE multi-agent orchestration framework for complex workflows:
- Plan → Execute → Verify → Commit automation
- MCPOrchestratorExecutor for SPINE's AgenticLoop

### MCP Server Ecosystem

Routes to any MCP-compatible server:
- `browser-mcp` - Browser automation via Playwright
- `paaf` - Project audit and analysis
- `research-agent` - Research workflows
- `filesystem` - File operations
- Custom MCP servers via configuration

### REST API Gateway

All capabilities accessible via REST:
- `/execute` - Execute tasks through the orchestrator
- `/tools` - List available tools and capabilities
- `/config` - View and reload configuration
- `/learning/stats` - Learning layer statistics
- `/health` - System health checks

---

## By the Numbers

| Metric | Value |
|--------|-------|
| **Total Tests** | 960+ |
| **Core Modules** | 6 (M1-M6) |
| **Extended Modules** | 10 (projects02 + projects03) |
| **Grafana Dashboards** | 4 |
| **Prometheus Metrics** | 15+ custom metrics |
| **API Endpoints** | 20+ |

---

## Conclusion

The Adaptive MCP Orchestrator Blueprint transforms AI tool integration from simple connections into an **intelligent, observable, and adaptive** platform. With full dashboard visibility, self-learning capabilities, and integration-ready APIs, it provides the foundation for scalable AI operations.

**Connect your systems** - whether SPINE, custom applications, or other AI frameworks - to leverage intelligent tool routing with complete transparency into system behavior.
