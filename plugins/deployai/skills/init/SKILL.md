---
name: init
description: Connect Claude Code to DeployAI and verify account access without uploading or publishing.
disable-model-invocation: true
---

Read `${CLAUDE_PLUGIN_ROOT}/references/workflow.md` before proceeding.

1. Read DeployAI's live capability discovery and confirm that connections are available.
2. Find the DeployAI tools supplied by this plugin. Their names begin with
   `mcp__plugin_deployai_deployai__`. Use these tools consistently so that a
   separately configured server cannot silently select another account or host.
3. If authentication is needed, direct the user to `/mcp` in Claude Code, select
   this plugin's DeployAI connection, and complete the client-generated browser
   sign-in and consent flow. Installation alone does not authenticate an account.
   Do not register a second server or replace an existing connection.
4. Call `list_projects`. An empty result is successful connectivity. Report only
   account and permission information actually returned by the server.
5. If the user supplied a workspace-verification challenge from DeployAI, call
   `verify_workspace` with that exact challenge and the intended workspace's name.
   Send neither absolute filesystem paths nor file contents. If the challenge
   expired or was consumed, ask the user to start a new check in DeployAI.
6. Stop after verification. Do not create a project, prepare a release, or publish.
   Explain that `/deployai:prepare` is the next step when the user wants to upload.

If tools are missing, ask the user to reload plugins or start a new Claude Code
session, then retry discovery. Do not report a connection based on installation,
configuration, or an open browser alone.
