# Botlink post-split feature migration

## Scope

Migrate business capabilities added to the eleven Botlink repositories after the AgentLife split while preserving AgentLife-specific behavior. Do not copy credentials, production URLs, brand text, or environment-only CI changes.

The repositories do not share Git ancestry. AgentLife services were re-imported between 2026-05-20 and 2026-05-27, so changes must be ported as feature patches rather than merged by branch.

All target repositories use the branch `sync/botlink-post-split`.

## Migration rules

- Keep AgentLife table names (`t_al_*`), package scopes, API prefixes, environment variables, authentication, and product copy.
- Preserve AgentLife changes made after the split, especially REQ-013 reply routing, REQ-014 notifications, default-bot onboarding, timestamp normalization, and login/session changes.
- Reimplement or adapt a patch when Botlink and AgentLife changed the same code path.
- Port tests with each capability and add compatibility coverage where both implementations overlap.
- Exclude secrets, OSS keys, production hosts, and mechanical build-only commits unless required for compilation.

## Feature ledger

| Capability | Botlink repositories | AgentLife status | Migration action |
|---|---|---|---|
| Cross-group shadow participants and messages | conversation, message, routing, permission, bot-gateway, web | Complete | Adapted across all six repositories with AgentLife table/API names |
| Requester-scoped approval auto grants | permission | Complete | Added `t_al_bot_approval_auto_grants`; merged ordinary and cross-group grant scopes |
| Structured mention routing and punctuation handling | routing, bot-gateway, web | Complete | Preserved AgentLife REQ-013 and added selected shadow/Agent mention routing |
| Bot reply stream timing/status | client-gateway, bot-gateway, routing, web | Complete | Preserved timestamp normalization and added start/chunk/completion timestamps |
| Scheduled and interval messages | conversation, bot-gateway, web | Complete | Added interval, Agent-owned APIs, run-now, and user controls |
| Agent group/member management | conversation, bot-gateway | Complete | Added internal/OpenAPI APIs while retaining AgentLife member limits |
| Agent provisioning and roles | bot, conversation, bot-gateway, operation, operation-web, web | Complete | Added provision tokens, roles/capabilities, and operations-Agent UI |
| Batch participant presence | routing | Complete | Added internal batch presence API using AgentLife session stores |
| User status controls | user and administration surfaces | Verified | No post-split Botlink business delta required in user-service |

## Delivery order

1. Shared foundations: message sender type, approval storage, routing helpers.
2. Cross-group shadow orchestration in conversation service, then gateway and web integration.
3. Agent provisioning, roles, and managed groups.
4. Schedules and reply timing/status.
5. Operation service/web and final user-facing integration.
6. Cross-service build, test, migration, configuration, and brand/secret scans.

## Progress

- [x] Establish split window and confirm histories are unrelated.
- [x] Create migration branches in all eleven AgentLife repositories.
- [x] Port shadow message sender support to `agent-life-message-service`.
- [x] Port requester-scoped approval storage and decision behavior.
- [x] Port structured mention routing, bot-status timestamps, and batch presence API.
- [x] Port scheduled conversation messages and interval execution.
- [x] Port Agent roles, capabilities, provisioning controls, and Agent-owned schedule storage.
- [x] Add Agent-managed conversation/member APIs.
- [x] Add Agent schedule internal APIs.
- [x] Complete cross-group shadow orchestration and routing dispatch.
- [x] Integrate approved shadow delivery, return metadata, and Bridge CLI self-mentions.
- [x] Add shadow binding management, structured shadow mentions, and sender display in web.
- [x] Complete shadow binding cleanup and real-Agent multi-group membership lifecycle.
- [x] Merge cross-group approval callbacks with requester-scoped auto grants.
- [x] Add operation provision-token backend and administration page.
- [x] Push `sync/botlink-post-split` for all eleven repositories.
- [x] Run repository tests/builds for every changed repository.
- [x] Scan migrated source for Botlink package/table names, production URLs, and credential patterns.

## Final branch heads

| Repository | Head |
|---|---|
| agent-life-bot-gateway | `f891c69` |
| agent-life-bot-service | `a4701e5` |
| agent-life-client-gateway | `8052d30` |
| agent-life-conversation-service | `406c7b1` |
| agent-life-message-service | `6c1012b` |
| agent-life-permission-service | `1d20e48` |
| agent-life-routing-service | `10404c6` |
| agent-life-user-service | `e3b79df` (no migration delta) |
| agent-life-web | `20d4157` |
| agent-life-operation-service | `e9969a6` |
| agent-life-operation-web | `62eb086` |
