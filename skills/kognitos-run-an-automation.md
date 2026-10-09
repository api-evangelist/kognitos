---
generated: '2026-10-09'
method: generated
name: kognitos-run-an-automation
title: Run an automation and collect its outputs
description: Find a Kognitos automation, invoke it with inputs, poll the run until it reaches a terminal state, then fetch
  outputs and the run event log.
api: openapi/kognitos-openapi.yml
operations:
- CurrentUserOrganizations
- ListWorkspaces
- ListAutomations
- GetAutomation
- InvokeAutomation
- GetRun
- ListRunEvents
- GetExecutionOutputs
source: Grounded in openapi/kognitos-openapi.yml (every operationId verified verbatim) and https://docs.kognitos.com/guides/mcp/mcp.
  No provider-published skills or AGENTS.md found (docs.kognitos.com/AGENTS.md returns a GitBook not-found page).
---

# Run an automation and collect its outputs

Find a Kognitos automation, invoke it with inputs, poll the run until it reaches a terminal state, then fetch outputs and the run event log.

## Steps
1. `CurrentUserOrganizations` then `ListWorkspaces` to resolve `organization_id` and `workspace_id`.
2. `ListAutomations` (paged with `page_size` / `page_token`) and `GetAutomation` to pick the automation and read its inputs.
3. `InvokeAutomation` with body `{stage, inputs}` (`stage` is required). The response returns `run_id`.
4. Poll `GetRun` until the run state is terminal (the spec models pending, executing, awaiting-guidance, stopping, stopped, failed, completed states). An awaiting-guidance run needs a human or the exceptions flow.
5. `ListRunEvents` for the execution log; `GetExecutionOutputs` for the outputs.

## Rules
- Auth: `Authorization: Bearer <Personal Access Token>` on every call (BearerAuth in the spec; see https://docs.kognitos.com/guides/api-reference/api-keys). Base URL `https://app.us-1.kognitos.com`; every path is scoped by `organization_id` and `workspace_id` — resolve them first with `CurrentUserOrganizations` / `ListWorkspaces`.
- Errors: the spec documents only success codes plus a `default` error response; treat any non-2xx as failure and surface the body.
- Idempotency: the spec defines no idempotency key. Do not blindly retry state-changing POSTs (they may create a second run/schedule); re-read state first.
- MCP equivalent: see ../mcp/kognitos-tool-crosswalk.yml.
