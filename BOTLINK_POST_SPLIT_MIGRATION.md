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
| Cross-group shadow participants and messages | conversation, message, routing, permission, bot-gateway, web | Missing | Adapt across all six repositories |
| Requester-scoped approval auto grants | permission | Missing | Add `t_al_bot_approval_auto_grants` and preserve existing approval modes |
| Structured mention routing and punctuation handling | routing, bot-gateway, web | Partial | Merge with AgentLife REQ-013 reply-routing behavior |
| Bot reply stream timing/status | client-gateway, bot-gateway, routing, web | Partial | Preserve AgentLife timestamp normalization and add timing fields |
| Scheduled and interval messages | conversation, bot-gateway, web | Partial | Merge interval and Agent-owned schedule support with AgentLife todos/reminders |
| Agent group/member management | conversation, bot-gateway | Missing | Add internal APIs, OpenAPI exposure, and membership rules |
| Agent provisioning and roles | bot, conversation, bot-gateway, operation, operation-web, web | Partial | Add provisioning tokens, role controls, operations-agent UI and APIs |
| Batch participant presence | routing | Missing | Add internal batch API using AgentLife session stores |
| User status controls | user and administration surfaces | Baseline likely imported | Verify current behavior; port only missing paths |

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
- [ ] Port approval storage and decision behavior.
- [ ] Port routing behavior and batch presence API.
- [ ] Complete remaining capabilities in dependency order.
