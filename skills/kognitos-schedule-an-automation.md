---
generated: '2026-10-09'
method: generated
name: kognitos-schedule-an-automation
title: Schedule an automation
description: Create, inspect, update, enable/disable or delete the recurring schedule of a Kognitos automation.
api: openapi/kognitos-openapi.yml
operations:
- GetAutomation
- GetAutomationSchedule
- CreateAutomationSchedule
- UpdateAutomationSchedule
- EnableAutomationSchedule
- DisableAutomationSchedule
- DeleteAutomationSchedule
source: Grounded in openapi/kognitos-openapi.yml (every operationId verified verbatim) and https://docs.kognitos.com/guides/mcp/mcp.
  No provider-published skills or AGENTS.md found (docs.kognitos.com/AGENTS.md returns a GitBook not-found page).
---

# Schedule an automation

Create, inspect, update, enable/disable or delete the recurring schedule of a Kognitos automation.

## Steps
1. `GetAutomation` to confirm the target.
2. `GetAutomationSchedule` to see whether one exists.
3. `CreateAutomationSchedule` with body requiring `config` (optional `name`, `enabled`); the response echoes `summary`, `next_run_times` and `remaining_occurrences` — show those to the user to confirm.
4. Change with `UpdateAutomationSchedule` (PATCH); toggle with `EnableAutomationSchedule` / `DisableAutomationSchedule`; remove with `DeleteAutomationSchedule`.

## Rules
- Auth: `Authorization: Bearer <Personal Access Token>` on every call (BearerAuth in the spec; see https://docs.kognitos.com/guides/api-reference/api-keys). Base URL `https://app.us-1.kognitos.com`; every path is scoped by `organization_id` and `workspace_id` — resolve them first with `CurrentUserOrganizations` / `ListWorkspaces`.
- Errors: the spec documents only success codes plus a `default` error response; treat any non-2xx as failure and surface the body.
- Idempotency: the spec defines no idempotency key. Do not blindly retry state-changing POSTs (they may create a second run/schedule); re-read state first.
- MCP equivalent: see ../mcp/kognitos-tool-crosswalk.yml.
