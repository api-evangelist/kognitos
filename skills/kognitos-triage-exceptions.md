---
generated: '2026-10-09'
method: generated
name: kognitos-triage-exceptions
title: Triage automation exceptions
description: List and count open exceptions in a workspace, inspect one with its troubleshooting guide, and pause or continue
  the affected run.
api: openapi/kognitos-openapi.yml
operations:
- ListExceptions
- CountExceptionsByGroup
- CountExceptionsByAutomation
- ListExceptionGroups
- GetException
- ListGuideEntries
- GetGuideEntry
- PauseRun
- ContinueRun
source: Grounded in openapi/kognitos-openapi.yml (every operationId verified verbatim) and https://docs.kognitos.com/guides/mcp/mcp.
  No provider-published skills or AGENTS.md found (docs.kognitos.com/AGENTS.md returns a GitBook not-found page).
---

# Triage automation exceptions

List and count open exceptions in a workspace, inspect one with its troubleshooting guide, and pause or continue the affected run.

## Steps
1. `CountExceptionsByGroup` / `CountExceptionsByAutomation` for an overview; `ListExceptionGroups` for the groups.
2. `ListExceptions` (filterable, paged) and `GetException` for the one to triage.
3. `ListGuideEntries` / `GetGuideEntry` on the owning automation to read the troubleshooting guide.
4. Resolve via the guidance, then `ContinueRun` (or `PauseRun` to hold a run while investigating).
Note: archiving, assigning and replying to exceptions are exposed only through the MCP server (`kognitos_manage_exceptions`, `kognitos_reply_to_exception`), not the public REST API.

## Rules
- Auth: `Authorization: Bearer <Personal Access Token>` on every call (BearerAuth in the spec; see https://docs.kognitos.com/guides/api-reference/api-keys). Base URL `https://app.us-1.kognitos.com`; every path is scoped by `organization_id` and `workspace_id` — resolve them first with `CurrentUserOrganizations` / `ListWorkspaces`.
- Errors: the spec documents only success codes plus a `default` error response; treat any non-2xx as failure and surface the body.
- Idempotency: the spec defines no idempotency key. Do not blindly retry state-changing POSTs (they may create a second run/schedule); re-read state first.
- MCP equivalent: see ../mcp/kognitos-tool-crosswalk.yml.
