# DeployAI for Claude Code

Connect Claude Code to [DeployAI](https://atar.biz), prepare your website or
supported web app, approve its publication, and get a live address.

This repository is a Claude Code plugin marketplace containing the `deployai`
plugin. It connects directly to DeployAI's existing MCP service. Account sign-in
and permissions use browser OAuth; installation does not sign you in or publish.

## Install

In an interactive Claude Code session:

```text
/plugin marketplace add purebox55/deployai-claude-plugin
/plugin install deployai@deployai-marketplace
/reload-plugins
/deployai:init
```

Alternatively, after adding the marketplace, run `/plugin`, open **Discover**,
find **deployai**, and install it. If the connection is not loaded after reloading,
start a new Claude Code session.

When prompted, open `/mcp`, select the plugin's DeployAI connection, and complete
sign-in and consent in your browser. Never paste a password, verification code or
token into the conversation. An empty project list is a successful connection.

The equivalent terminal installation is:

```sh
claude plugin marketplace add purebox55/deployai-claude-plugin
claude plugin install deployai@deployai-marketplace
```

Then start Claude Code and run `/deployai:init`.

## Commands

| Command                                     | What it does                                                                                   |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `/deployai:init`                            | Connect and verify access. No upload or publication.                                           |
| `/deployai:prepare`                         | Inspect the intended project and upload selected assets as a candidate. Stops before approval. |
| `/deployai:publish <projectId> <releaseId>` | Show the exact candidate, obtain explicit approval, publish, and verify the live site.         |
| `/deployai:status`                          | Read projects and release status.                                                              |

For a first site, tell Claude which project/folder you want to publish and whether
access should be public or private. Run `/deployai:prepare`. Plain HTML needs no
build; supported frameworks use their existing build. Preparation uploads the
selected files to DeployAI before publication approval.

Review the summary, address, access and release details before confirming
publication. OAuth consent and running the publish command do not approve an
unseen release. If the same exact candidate was already explicitly approved in
the conversation, that approval remains valid.

DeployAI supports static sites, React/Vite frontends and documented bundled
Cloudflare Workers APIs. Check the [compatibility guide](https://docs.atar.biz/text/compatibility.txt)
for limits. A private site requires its appropriate sign-in. Pilot capacity and
account quotas still apply.

## Update or remove

Refresh the marketplace, then update the installed plugin:

```text
/plugin marketplace update deployai-marketplace
/plugin update deployai@deployai-marketplace
/reload-plugins
```

To remove it:

```text
/plugin uninstall deployai@deployai-marketplace
```

Uninstalling removes the plugin; it does not delete your sites. Manage or revoke
account grants separately in [DeployAI connections](https://dash.atar.biz/settings/connections).

## Existing MCP connections

This plugin bundles its own connection. Its tools are named
`mcp__plugin_deployai_deployai__<tool_name>` and the configured server appears as
`plugin:deployai:deployai`. A manually configured DeployAI MCP server is separate
and can have a different account or OAuth grant. The commands use the plugin's
connection; they do not overwrite or silently reuse another server. Missing
publication permission requires renewed browser consent for `releases:publish`.

## Privacy and permissions

The plugin contains Markdown instructions and a native HTTPS MCP configuration.
It declares no hooks, shell helpers, additional agents, embedded credentials or
pre-approved tools. Claude Code's normal permissions remain in effect.

Connection verification reads account/project information. A supplied workspace
challenge sends only that challenge and a workspace name. Preparation sends
selected site files and optional Worker/database configuration to DeployAI.
Inspect files before upload; do not include environment files, credentials or
unrelated workspace content. See [DeployAI's privacy policy](https://atar.biz/privacy).

## Develop and verify

Validated with Claude Code **2.1.304, 2.1.306 and 2.1.308**, Node.js **22.20.0**, macOS arm64. See
[validation and acceptance](docs/validation.md) for observed evidence and pending
installed-client checks.

```sh
claude plugin validate --strict .
claude plugin validate --strict ./plugins/deployai
claude --plugin-dir ./plugins/deployai
```

The repository root is the **marketplace**; `plugins/deployai` is the **plugin**.
Publish only this standalone directory, never the private application repository.
This is a third-party marketplace; listing in Anthropic's official directory is
a separate submission.

Format the Markdown and JSON with Prettier. Change the plugin's version for each
release, update [the changelog](CHANGELOG.md), validate both manifests, and repeat
the fresh-install checks before publishing an update.
