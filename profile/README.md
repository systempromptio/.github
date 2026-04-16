<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://systemprompt.io/files/images/logo.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://systemprompt.io/files/images/logo-dark.svg">
  <img src="https://systemprompt.io/files/images/logo-dark.svg" alt="systemprompt.io" width="380">
</picture>

# Own how your organization uses AI.

The narrow waist between your AI and everything it touches.
Self-hosted. Air-gapped. Every interaction governed and provable.

[**Try it now**](https://github.com/systempromptio/systemprompt-template)

[**systemprompt.io**](https://systemprompt.io) · [**Documentation**](https://systemprompt.io/documentation/) · [**Guides**](https://systemprompt.io/guides) · [**Discord**](https://discord.gg/wkAbSuPWpr)

</div>

---

## AI governance at scale

3,308 req/s on a laptop. p50 13.5 ms. <1% overhead on AI response time. One compiled Rust binary. PostgreSQL. Nothing else.

<div align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/int-benchmark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/light/int-benchmark.svg">
  <img alt="Governance benchmark: 3,308 req/s" src="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/int-benchmark.svg" width="820">
</picture>
</div>

---

## Infrastructure

A 50MB Rust binary that authenticates, authorises, rate-limits, logs, and costs every AI interaction. Four in-process services, 144 database tables, zero sidecars. The same artifact deploys to Docker, bare metal, cloud, or an air-gapped network. Config is profile-based YAML checked into version control. JWT validation and rate limiting execute locally per process without distributed infrastructure.

Every layer uses an open standard: MCP for tool communication, OAuth 2.0 and WebAuthn for identity, ChaCha20-Poly1305 for encryption at rest, PostgreSQL for storage, Git for distribution. No proprietary protocols at any layer.

| Self-hosted deployment | Deploy anywhere |
|:---:|:---:|
| <picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/infra-self-hosted.svg"><source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/light/infra-self-hosted.svg"><img alt="Self-hosted deployment" src="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/infra-self-hosted.svg" width="100%"></picture> | <picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/infra-deploy-anywhere.svg"><source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/light/infra-deploy-anywhere.svg"><img alt="Deploy anywhere" src="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/infra-deploy-anywhere.svg" width="100%"></picture> |

[Self-Hosted Deployment](https://systemprompt.io/features/self-hosted-ai-platform) · [Deploy Anywhere](https://systemprompt.io/features/deploy-anywhere) · [Unified Control Plane](https://systemprompt.io/features/unified-control-plane) · [No Vendor Lock-In](https://systemprompt.io/features/no-vendor-lock-in)

---

## Capabilities

### Governance pipeline

Four synchronous enforcement layers evaluate every tool call before execution. Scope check enforces a six-tier RBAC hierarchy (Admin, User, Service, A2A, MCP, Anonymous) where each tier inherits permissions through `Permission::implies`. Secret detection runs 35+ regex patterns against tool inputs to block AWS keys, GitHub PATs, PEM private keys, and database connection strings before they reach a model. Blocklist prevents destructive operations. Rate limiting caps at 300 requests per minute per session with role-based multipliers (Admin 10x, User 1x, Anonymous 0.5x).

Every decision lands in an 18-column audit table with 17 indexes, queryable from the CLI or exportable to your SIEM.

### Secrets management

Credentials are encrypted at rest with ChaCha20-Poly1305 AEAD and injected server-side into subprocess environment variables at tool-call time. Secrets never enter the LLM context window, never appear in logs, and never transit the inference path. The detection layer blocks 12 dangerous file extensions (`.env`, `.sql`, `.bak`, `.config`, etc.) and identifies 20+ scanner tool signatures at the edge. JWT sessions use HS256 signing with a mandatory 32-character minimum secret. Token TTL is 5 minutes.

### Analytics and observability

A single TraceId correlates every event from login through model output. Six correlation columns (UserId, SessionId, TaskId, TraceId, ContextId, ClientId) bind identity at construction time so a row that reaches the database without a trace is a programming error. Cost is tracked in integer microdollars to prevent floating-point drift, with attribution by model, provider, and agent. Anomaly detection flags values exceeding 2x (warning) or 3x (critical) of the rolling average. Nine CLI subcommands cover overview, conversations, agents, tools, requests, sessions, content, traffic, and costs.

### MCP governance

Each MCP server operates as an independent OAuth2 resource server with isolated credentials and per-server scope validation. If a tool is not declared in the plugin manifest, it does not exist for that agent. Deploy-time validation runs four passes (port conflicts, server configs, OAuth requirements, server types) and all four pass or no servers start. The `McpToolHandler` trait enforces type safety at compile time via `DeserializeOwned + JsonSchema` inputs and `Serialize + JsonSchema` outputs.

### Closed-loop agents

Agents query their own error rate, cost, and latency through exposed MCP tools and adjust without a human in the loop. Every logged event carries eight correlation fields enabling complete request lineage reconstruction. The A2A protocol enables multi-provider agent workflows with full governance applied to every hop.

| Governance pipeline | Secrets management | Compliance |
|:---:|:---:|:---:|
| <picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/cap-governance.svg"><source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/light/cap-governance.svg"><img alt="Governance pipeline" src="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/cap-governance.svg" width="100%"></picture> | <picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/cap-secrets.svg"><source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/light/cap-secrets.svg"><img alt="Secrets management" src="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/cap-secrets.svg" width="100%"></picture> | <picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/cap-compliance.svg"><source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/light/cap-compliance.svg"><img alt="Compliance" src="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/cap-compliance.svg" width="100%"></picture> |

[Governance Pipeline](https://systemprompt.io/features/governance-pipeline) · [Secrets Management](https://systemprompt.io/features/secrets-management) · [MCP Governance](https://systemprompt.io/features/mcp-governance) · [Analytics & Observability](https://systemprompt.io/features/analytics-and-observability) · [Closed-Loop Agents](https://systemprompt.io/features/closed-loop-agents) · [Compliance](https://systemprompt.io/features/compliance)

---

## Integrations

Provider-agnostic governance for Anthropic, OpenAI, Gemini, or custom agents. The `AiProvider` trait abstracts 19 methods so switching providers changes config, not code. Cost attribution tracks spend across providers, models, and agents with microdollar precision.

The extension system compiles your code into the binary. The `Extension` trait exposes routes, schemas, migrations, jobs, LLM providers, tool providers, page prerenderers, roles, and config namespaces. 12 extensions ship by default with 71 sqlx-checked schemas and 13 background jobs. Registration happens at link time via `register_extension!`, so there is no runtime reflection and no dynamic loading.

Skills persist across Claude Desktop sessions via OAuth2. Slash commands activate business skills (brand voice, templates, policies) governed by the same four-layer pipeline. The same binary that governs AI agents also serves your website, blog, and documentation with Markdown content, PostgreSQL full-text search, and engagement analytics.

| Any AI agent | Extensible architecture |
|:---:|:---:|
| <picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/int-any-agent.svg"><source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/light/int-any-agent.svg"><img alt="Any AI agent" src="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/int-any-agent.svg" width="100%"></picture> | <picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/int-extensions.svg"><source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/light/int-extensions.svg"><img alt="Extensible architecture" src="https://raw.githubusercontent.com/systempromptio/systemprompt-template/main/demo/recording/svg/output/dark/int-extensions.svg" width="100%"></picture> |

[Any AI Agent](https://systemprompt.io/features/any-ai-agent) · [Claude Desktop & Cowork](https://systemprompt.io/features/cowork) · [Web Server & Publisher](https://systemprompt.io/features/web-publisher) · [Extensible Architecture](https://systemprompt.io/features/extensible-architecture)

---

## Compliance

Identity-bound, replayable audit trails via JWT. Policy-as-code on PreToolUse hooks with 10 lifecycle event variants. Tiered log retention from debug (1 day) through error (90 days). 25+ query methods in `TraceQueryService` for forensic analysis. All data stays on-premises with no outbound telemetry.

Built for SOC 2 Type II, ISO 27001, HIPAA, and OWASP Agentic Top 10.

---

## Repositories

| Project | |
|---|---|
| [systemprompt-core](https://github.com/systempromptio/systemprompt-core) | The Rust library. One binary, every governance primitive. BSL-1.1. |
| [systemprompt-template](https://github.com/systempromptio/systemprompt-template) | Local evaluation. Clone, build, run 40+ demos against your own machine. MIT. |
| [systemprompt-code-orchestrator](https://github.com/systempromptio/systemprompt-code-orchestrator) | MCP server orchestrating Claude Code + Gemini CLI as governed coding agents. |
| [systemprompt-mcp-server](https://github.com/systempromptio/systemprompt-mcp-server) | Production MCP server with OAuth 2.1, task management, and integrations. |
| [systemprompt-marketplace](https://github.com/systempromptio/systemprompt-marketplace) | Plugin marketplace for the systemprompt.io ecosystem. |

---

## Evaluate locally

```bash
just build                                               # compile the workspace
just setup-local <anthropic> <openai> <gemini>           # profile + Postgres + publish
just start                                               # serve on localhost:8080
```

Point Claude Code, Claude Desktop, or any MCP client at it. Permissions follow the user, not the client. Run `systemprompt --help` to explore 8 CLI domains covering governance, analytics, infrastructure, agents, plugins, content, cloud, and build.

---

<div align="center">

[**systemprompt.io**](https://systemprompt.io) · [**Documentation**](https://systemprompt.io/documentation/) · [**Guides**](https://systemprompt.io/guides) · [**Discord**](https://discord.gg/wkAbSuPWpr)

<sub>Own how your organization uses AI. Every interaction governed and provable.</sub>

</div>
