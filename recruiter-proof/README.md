# Adaptive MCP Orchestrator recruiter-proof evidence manifest

Verified: 2026-08-02

## Purpose

This directory is the current recruiter-facing evidence entry for the Adaptive MCP Orchestrator Blueprint.
It supplements the older public showcase without rewriting its historical demonstrations or volatile provider examples.

## Pins

- private source repository: `fbratten/ai-projects-and-management-example`
- inspected source pin: `e86fefcac3f3b06d5afd01caeb5cdce8660a19e4`
- public showcase repository: `fbratten/Adaptive-MCP-Orchestrator-Blueprint-Showcase`
- pre-change showcase pin: `417fa071575e546bd3ef48e2e07d3da7491546d4`
- current proof route: `recruiter-proof/index.html`

## Current framing

The project is a complete teaching and reference implementation of:

- capability-based tool and provider routing;
- retry and fallback chains;
- FastAPI dashboard and SSE surfaces;
- structured logging and Prometheus/Grafana/Loki observability;
- LanceDB and Neo4j learning signals;
- an MCP stdio server and extended provider/router modules.

It is not presented as an active production service or as the estate's current universal MCP router.

## Test boundary

The source README records `951 tests passing`.
A separate deep inspection counted approximately 1,038 test functions across 52 files. That count describes discovered test definitions, not a passing test-run receipt.
No suite was re-run during this static publication change.

## SPINE boundary

SPINE contains a dedicated MCPOrchestratorExecutor and fallback path. That establishes an implemented integration seam on the SPINE side. It does not prove that the standalone backend is currently running or that this project is SPINE's adopted universal router.

## Security follow-up

Private source issue #21 tracks rotation and removal of provider credentials previously found committed in MCP configuration history.

- no credential value is repeated here;
- this showcase refresh does not close the issue;
- the source repository remains private;
- public visitors should use the synthetic showcase rather than clone instructions.

## Non-claims

This proof does not claim:

- current production deployment;
- multi-tenant authentication;
- current provider-model rankings or prices;
- that the source repository is public;
- that the original Meta-Router is the estate's current router product;
- that tests were re-run during publication.

## Public-data policy

Only architecture summaries and synthetic routing inputs are published. No credentials, private configuration, source code, logs, prompts, provider accounts, learning-store records or operational data are included.
