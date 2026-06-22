# 0001-my-slack-proxy — RESEARCH

Evidence gathered during SPEC→DESIGN. **Collection caveat:** `mgrep --web` was over its monthly quota (429, resets next month), so these are from established Slack-platform / MCP knowledge, **not freshly fetched primary sources**. Items marked `[VERIFY]` need live re-verification when the quota resets.

## Slack ingestion — Socket Mode vs Events API
- Socket Mode = app opens a persistent **outbound WebSocket** to Slack; receives events/interactions over it. No public HTTPS endpoint, no inbound firewall opening.
- Events API = Slack POSTs to a **public HTTPS** Request URL (URL verification + signed-request validation required).
- Socket Mode needs an app-level token `xapp-` with `connections:write`. `[VERIFY]`
- Socket Mode is positioned for internal/single-workspace apps; **not for Marketplace distribution / large scale**; small per-app concurrent-connection cap (historically ~10). `[VERIFY exact cap]`
- Legacy **RTM API is deprecated**; Events API + Socket Mode is the replacement. `[VERIFY exact date]`
- Source: Slack docs "Socket Mode" (docs.slack.dev/apis/events-api/using-socket-mode); RTM deprecation (api.slack.com/rtm). `[VERIFY URLs]`

## Slack read access — bot token vs user token
- A **bot token (`xoxb-`) can only read conversations the bot is a member of** — no DMs, no private channels it isn't in, cannot call `search.messages`.
- To read a specific person's full surface, that person OAuth-grants **user scopes** and the app uses their **user token (`xoxp-`)**.
- Read scopes: `channels:history`, `groups:history`, `im:history`, `mpim:history` (+ `*:read` companions to enumerate conversations) and `search:read` (user-token-only).
- **Token rotation:** when enabled, access tokens are short-lived (~12h) + refresh tokens; refresh via `oauth.v2.access`. Classic tokens (rotation off) are long-lived. `[VERIFY 12h]`
- **2025 rate-limit tightening:** `conversations.history`/`conversations.replies` throttled hard for non-Marketplace apps (~1 req/min, ~15 objects/call) — pushes architecture toward event-driven ingestion over history polling. `[VERIFY exact numbers/date/tiers]`
- Source: Slack "Token rotation" (api.slack.com/authentication/rotation); rate-limit changelog (api.slack.com/changelog, api.slack.com/docs/rate-limits). `[VERIFY]`

## Slack disclosed posting (non-impersonation)
- Posting with the **bot token via `chat.postMessage`** always renders with the app/bot identity and an immutable **APP badge**.
- `username`/`icon_url` overrides require `chat:write.customize` and **still show the APP badge** — cannot impersonate a person.
- Posting *as the user* requires a **user token with `chat:write`** (no badge) — true impersonation; to be avoided for a disclosed agent.
- Disclosed pattern: bot identity + thread reply (`thread_ts`) + explicit "on behalf of @user" copy.
- Source: Slack `chat.postMessage` (api.slack.com/methods/chat.postMessage). `[VERIFY customize behavior]`

## Multi-tenant in one workspace
- Supported shape: **one installed app + one bot token + N per-user user-tokens** (each user runs the user-scope OAuth grant; app stores per-user `xoxp-`).
- OAuth v2 separates bot scopes (once per install) from user scopes (per authorizing user).
- **Admin-gateable:** app install can require admin approval (Enterprise Grid / app-approval); admins can restrict which scopes/OAuth flows are permitted. Per-user authorization is not guaranteed without admin cooperation.
- Source: Slack "Installing with OAuth" (api.slack.com/authentication/oauth-v2). `[VERIFY admin controls]`

## MCP as the context-source adapter
- **Model Context Protocol (MCP)** = current de-facto open standard for pluggable, source-agnostic context/tooling for LLM agents (Anthropic, late 2024; broad cross-vendor adoption since).
- Shape: a **host** runs **clients**, each connecting 1:1 to a **server** fronting one source (Atlassian/Confluence/Jira, GitHub, Google Workspace, Slack), exposing tools/resources/prompts.
- Transports: stdio (local), Streamable HTTP (remote; supersedes older HTTP+SSE). `[VERIFY transport naming]`
- Maturity: fast-moving; server quality varies — pin versions. **Auth is the main caveat**: OAuth 2.1-style, with confused-deputy and prompt-injection-via-tool risks; per-source credential scoping and tenant isolation must be designed, not assumed.
- Source: MCP spec (modelcontextprotocol.io); Anthropic intro (anthropic.com/news/model-context-protocol). `[VERIFY spec version]`

## Go ecosystem note (language = Go, asserted from prior knowledge, not freshly verified)
- Slack: `slack-go/slack` supports Socket Mode. MCP: official `modelcontextprotocol/go-sdk` and community `mark3labs/mcp-go` exist but are less mature than Python/TS SDKs. Model providers: `anthropic-sdk-go`, AWS SDK Go v2 (Bedrock), GCP Vertex Go. `[VERIFY current maturity/versions]`
