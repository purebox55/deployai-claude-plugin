---
name: status
description: Read DeployAI projects and release status without creating or publishing anything.
argument-hint: "[project ID] [release ID]"
---

Read `${CLAUDE_PLUGIN_ROOT}/references/workflow.md` before proceeding.

Use $ARGUMENTS and conversation context to identify the requested project and
release. Call `list_projects` if no project is identified; call `get_project` for
a selected project and `get_release` when a release ID is available. Resolve
ambiguous names before reporting a specific project's state.

Report the server's project/release status, URL, access and safe failure details
when available. An empty list is valid. Distinguish a pending candidate, a
deployment in progress, a failed release and the currently live version.
Never infer that a site works merely from its status.

Use read tools only. Do not create projects, upload, approve, cancel releases,
change access, or repair a failed publication as part of this command. If login
is needed, follow `/deployai:init`'s browser-consent instructions.
