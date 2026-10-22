# Validation and acceptance

## Recorded environment

- Date: 20 October 2026 (Asia/Jerusalem).
- Client: native Claude Code 2.1.304, macOS arm64.
- Runtime: Node.js 22.20.0, matching the product's `.nvmrc`.
- Service: `https://dash.atar.biz/mcp`, native HTTP with browser OAuth.
- Package version: 0.1.0.

## Package checks

Run strict validation for both directories. The marketplace validator alone does
not establish that every skill and server loads correctly.

```sh
claude plugin validate --strict .
claude plugin validate --strict ./plugins/deployai
```

From a disposable Claude Code configuration directory, add this marketplace by
absolute local path and install `deployai@deployai-marketplace`. Inspect
`claude plugin list` and `claude plugin details deployai`. Confirm all four skills
and the HTTP server are present in the cached package, with the shared reference
included. Test updates and uninstall there, not against a user's normal settings.

Repeat the installation from the actual GitHub marketplace after publication.
Keep the test configuration private and outside Git. No authenticated API or
publication outcome is implied by a successful installation.

## Fresh-user installed-client acceptance

Use a new account and fresh browser state. Do not inject a session or seed wizard
state. Record actual Claude Code version, OS, commit and observed tool names.

1. Add the GitHub marketplace, install, reload/start a session, and confirm that
   `/deployai:init`, `/deployai:prepare`, `/deployai:publish` and `/deployai:status`
   appear. Check the plugin-provided MCP server in `/mcp`.
2. Run init. Complete email verification and explicit browser consent. Record
   granted scope names without recording tokens/codes. Confirm a successful
   plugin `list_projects` call; an empty list is valid. Verify no project or
   release was created by init. Exercise denied consent and recovery.
3. If the user started a DeployAI workspace check, answer its exact challenge
   through the plugin's `verify_workspace` tool. Test expiry without claiming
   continuous connectivity or local file access.
4. Prepare an explicitly selected small HTML or React/Vite fixture. Record the
   chosen folder, uploaded file paths, destination/access, IDs and hashes. Check
   that files outside the folder and credentials were excluded. Confirm that
   preparation has not published the candidate.
5. Invoke publish. Confirm that the exact release details appear before the user
   approves. Declining must not publish. Approve the intended candidate through
   the conversation and record the actual `approve_release` arguments without
   credentials. A changed candidate requires renewed approval.
6. Wait for the provider-backed release result. Open the actual URL, inspect
   content/assets and use its main action. For a data-writing fixture, verify an
   authorized write survives reload. Check desktop and small screens, browser
   errors, and public/private access boundaries as appropriate.
7. Revoke the grant in DeployAI. Confirm plugin calls fail and browser
   reauthentication recovers. Verify missing `releases:publish` does not trigger
   a fallback to another credential or server.

Preserve failed steps, safe errors, project/release IDs, screenshots of surviving
tabs, browser/network logs and traces in restricted local artifacts. Never commit
session files, tokens, private account data or binary traces. Classify observed
findings as product bug, integration failure, UX friction, or test/environment
defect; leave unknown causes untriaged.

## Evidence boundaries

Strict validation and package installation are recorded separately from OAuth,
authenticated tool calls, preparation, publication and live-site verification.
The product's simulated-assistant publication suite and packaged desktop-bridge
suite do not certify this Claude Code plugin. No unaided participant session has
run; understandability validation remains pending.

## Observed results

- Strict marketplace validation: passed, no warnings.
- Strict plugin validation: passed, no warnings.
- Live unauthenticated discovery: available; expected production endpoint,
  OAuth transport and `releases:publish` scope advertised.
- Fresh local marketplace registration and installation: passed in an isolated
  configuration. Claude Code reported version 0.1.0, all four skills and the
  expected HTTP MCP server. The cached shared reference was inspected separately.
- Local marketplace refresh, plugin update, uninstall and reinstall: passed.
  Local folder installation uses the folder directly, so its update command is
  a no-op; this does not establish remote GitHub update behavior.
- Native `claude mcp get plugin:deployai:deployai`: reached the production server
  and reported `Needs authentication`, as expected for the fresh configuration.
  An initial sandbox attempt failed with DNS `ENOTFOUND`; the network-enabled
  check succeeded. This is an environment restriction, not a product finding.
- Unauthenticated production MCP initialization: returned HTTP 401 and the
  OAuth protected-resource metadata challenge, including `releases:publish`.
- Prettier Markdown/JSON formatting: passed.
- GitHub installation/update: pending. The owner created the public repository
  and invited `buzagloidan`; that invitation was accepted on 22 October. A
  README-only baseline initializes `main`; the plugin is introduced through a
  separate implementation branch and PR.
- Browser OAuth and authenticated plugin tools: pending.
- Installed-client preparation, publication and live-site checks: pending.
- First-time-user usability: pending.

### Revalidation on 21 October 2026

Native Claude Code was updated to 2.1.306. Strict manifest validation, formatting,
fresh isolated marketplace registration/installation and the four-skill/one-MCP
component inventory passed again. The new CLI also emits a separate directory
submission report: licensing and a listing icon remain unresolved. Those are
official-directory submission findings; native marketplace installation passed.
This package has not been submitted to Anthropic's official directory, and no
open-source license has been selected on the owner's behalf.

Only Markdown, JSON and an ignore file are added. No application code, provider
configuration or authentication implementation changed, and no application
release was attempted. The existing product's `npm run check` and simulated
publication/desktop-bridge suites were not run for this standalone package;
native Claude Code validation and the installation lifecycle checks above are
the package evidence. The isolated check did not reuse a user's normal Claude
configuration or authentication grant.
