# Decisions

## 2026-10-20 — Package the existing DeployAI MCP service for Claude Code

The Claude Code integration is distributed through a standalone public plugin
repository and its `deployai-marketplace` catalog. This keeps the private product
repository private and gives users one versioned installation for configuration
and workflow instructions.

Use the existing production HTTP MCP endpoint and browser OAuth. Add four skills
for connection, preparation, exact-release approval/publication, and status.
Connection and preparation stop at their own boundaries. No new backend, token
storage, hooks, shell helpers or agents are needed. Host permission prompts and
DeployAI's existing server authorization remain in force.

Authentication, publication and unaided first-user usability need separate
observed validation. A valid manifest or successful read does not certify them.
