# SentinelAI 

SentinelAI is a full-stack incident-response system. It is intentionally split into layers so you can reason about the product as an operating model instead of a single application:

1. The platform control plane manages identities, clusters, incidents, jobs, and the agent runtime.
2. The dashboard gives operators a stable UI for clusters, incident transcripts, audit trails, and account state.
3. The edge MCP servers expose live infrastructure, logs, metrics, GitHub history, and runbooks to the agent.
4. The Target_Client stack generates the traffic, failures, and observability signals that make the demo meaningful.

The point of the repository is not only to show code; it is to show a complete feedback loop. The target client produces symptoms, the edge layer exposes evidence, the agent reasons about that evidence, the backend persists state, and the dashboard lets a human read and steer the result.

## System Architecture

### Layer Architecture

![System Topology](docs/architecture/images/system-topology.svg)

The dashboard talks to the backend through Next.js rewrites, which keeps the browser origin simple. The agent runtime mounts the versioned SaaS API and the auth router, then uses the MCP layer to gather live evidence from the target side. The backend owns persistence and identity, not the reasoning flow itself.

**Key semantics:**
- Arrows show request and evidence flow, not startup dependency order
- Dashboard is a client of the API; no reverse dependency
- Target_Client produces incidents and observability signals  
- Edge MCP servers expose infrastructure as tools to the agent  
- Agent reasoning and persistence happen in the platform layer

### Request-to-Investigation Flow

![Agent Runtime Flow](docs/architecture/images/agent-runtime-flow.svg)

When an incident arrives or the user sends a follow-up question, the agent runtime orchestrates specialist agents to gather evidence in parallel, aggregates findings through a supervisor step, decides if more investigation is needed, and persists the final summary to the timeline.

### Backend Data Model

![Backend Data Model](docs/architecture/images/backend-data-model.svg)

The backend persists organizations, users, clusters, incidents, timeline events, jobs, audit trails, and SLOs. The incident timeline event table is the core: each event represents a step in the investigation, and `pending_supervisor` marks events that are eligible for follow-up handling.

For implementation details and diagram maintenance, see [docs/architecture/README.md](docs/architecture/README.md).

## Repository Map

| Path | What It Teaches |
| --- | --- |
| [platform/README.md](platform/README.md) | How the control plane is started and what services it depends on |
| [backend/README.md](backend/README.md) | How data, auth, models, and seeding work |
| [sre_agent/README.md](sre_agent/README.md) | How the LangGraph runtime and SaaS API are assembled |
| [dashboard/README.md](dashboard/README.md) | How the operator UI is structured and how it authenticates |
| [edge_mcp_servers/README.md](edge_mcp_servers/README.md) | How evidence is exposed from the edge and why MCP is used |
| [Target_Client/README.md](Target_Client/README.md) | How incidents are generated in the demo environment |
| [tests/README.md](tests/README.md) | Which behaviors are validated in code |
