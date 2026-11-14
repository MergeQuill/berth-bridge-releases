# Security Policy

## Supported versions

MQ Station is pre-1.0 and ships from `main`. Only the latest released version receives
fixes; there are no backports.

## Reporting a vulnerability

**Please do not open a public issue for a security problem.**

Report it privately through GitHub's
[Security Advisories](https://github.com/MergeQuill/mq-station-releases/security/advisories/new)
form on this repository. Include:

- what the flaw allows an attacker to do,
- the steps or the minimal project that reproduces it,
- the MQ Station version (Settings → About) and your macOS version.

This is a spare-time project: expect a first reply within a week. If a fix is needed,
you will be credited in the release notes unless you ask otherwise.

## What is in scope

MQ Station runs shells, spawns AI CLIs and touches your filesystem and git repositories on
purpose, so "the app can run commands" is not itself a vulnerability. Reports that
*are* in scope include:

- escaping a registered project root through a filesystem, LSP, search or git path,
- a secret (token, licence key, `.env` value) reaching a log, notification, error
  message, the UI or an outbound request that should not carry it,
- executing a command or a destructive git operation without the confirmation gate
  that is supposed to guard it,
- a credential read (Keychain, `~/.config/github-copilot/`, `~/.gemini/oauth_creds.json`)
  or an outbound quota request happening without a stored consent grant — see
  "Credentials" in `PRIVACY.md`,
- the preview webview navigating to a non-`http(s)` scheme, or otherwise bypassing
  its allowlist,
- SQL injection or corruption of the local database through user-controlled input,
- privilege escalation through the bundled Node sidecar or a language server.

## Out of scope

- Anything the AI agents themselves do — they run under their own providers' terms.
- Vulnerabilities in a tool MQ Station merely invokes from your `PATH` (Intelephense,
  phpactor, PHPStan, git, the agent CLIs) — report those upstream.
