# SentinelAI 

SentinelAI is a full-stack incident-response system. It is intentionally split into layers so you can reason about the product as an operating model instead of a single application:

1. The platform control plane manages identities, clusters, incidents, jobs, and the agent runtime.
2. The dashboard gives operators a stable UI for clusters, incident transcripts, audit trails, and account state.
3. The edge MCP servers expose live infrastructure, logs, metrics, GitHub history, and runbooks to the agent.
4. The Target_Client stack generates the traffic, failures, and observability signals that make the demo meaningful.

The point of the repository is not only to show code; it is to show a complete feedback loop. The target client produces symptoms, the edge layer exposes evidence, the agent reasons about that evidence, the backend persists state, and the dashboard lets a human read and steer the result.
