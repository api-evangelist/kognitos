---
generated: '2026-10-09'
method: generated
name: kognitos-upload-file-input
title: Upload a file as an automation input
description: Upload a document to a workspace (direct multipart or pre-signed URL) and pass it to an automation run.
api: openapi/kognitos-openapi.yml
operations:
- UploadFile
- GenerateUploadUrl
- GetFileMetadata
- InvokeAutomation
- GetRun
- DownloadFile
source: Grounded in openapi/kognitos-openapi.yml (every operationId verified verbatim) and https://docs.kognitos.com/guides/mcp/mcp.
  No provider-published skills or AGENTS.md found (docs.kognitos.com/AGENTS.md returns a GitBook not-found page).
---

# Upload a file as an automation input

Upload a document to a workspace (direct multipart or pre-signed URL) and pass it to an automation run.

## Steps
1. Small file: `UploadFile` (multipart/form-data). Large file: `GenerateUploadUrl` (optional `expire_duration`) returns `upload_uri`; PUT the bytes there.
2. `GetFileMetadata` to confirm the file.
3. `InvokeAutomation` referencing the file in `inputs`, then poll `GetRun`.
4. `DownloadFile` to retrieve file outputs.

## Rules
- Auth: `Authorization: Bearer <Personal Access Token>` on every call (BearerAuth in the spec; see https://docs.kognitos.com/guides/api-reference/api-keys). Base URL `https://app.us-1.kognitos.com`; every path is scoped by `organization_id` and `workspace_id` — resolve them first with `CurrentUserOrganizations` / `ListWorkspaces`.
- Errors: the spec documents only success codes plus a `default` error response; treat any non-2xx as failure and surface the body.
- Idempotency: the spec defines no idempotency key. Do not blindly retry state-changing POSTs (they may create a second run/schedule); re-read state first.
- MCP equivalent: see ../mcp/kognitos-tool-crosswalk.yml.
