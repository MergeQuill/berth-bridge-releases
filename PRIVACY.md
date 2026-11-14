# Privacy

MQ Station is a local desktop app. **It has no analytics, no telemetry, no crash
reporting and no account.** Nothing about you or your code is sent to the author or
to any service operated by this project — there is no such service.

What follows is the complete list of what MQ Station reads on your machine and every
request it can make over the network. If you find something on your system that this
document does not describe, please open an issue — that is a bug.

---

## What MQ Station stores

Everything lives on your Mac:

- **`~/Library/Application Support/com.mergequill.station/mq-station.db`** — an SQLite
  database holding your projects, project groups, pane layouts, terminal sessions
  and scrollback, agent cards, saved commands, prompt templates, notes, to-dos,
  command history, saved SSH servers (host, user, port and the *path* of a key —
  never a password or key), settings and token-usage history.
- **`~/Library/Application Support/com.mergequill.station/intelephense/`** — the PHP
  language server's index cache (kept out of your repositories on purpose).

There is no sync, no account and no server holding your data — MQ Station has none to
sign in to. If you turn on **Settings → Backup → auto-backup folder**, MQ Station writes
a copy of the database above into the folder you choose, on your own disk. Putting
that folder inside iCloud Drive, Dropbox or Drive means your operating system
uploads it to *your* cloud account under its own rules; MQ Station never sees that
account, its credentials, or the copy once it is written.

---

## What MQ Station reads

### Your projects

The directories you add. MQ Station walks them (respecting `.gitignore`), reads and writes
files you edit, and runs git operations on them. Every filesystem and LSP path is
constrained to a registered project root.

### AI CLI transcripts and usage logs — read-only

To draw the usage/quota panels and the Sessions/History lists, MQ Station reads the local
log files the AI CLIs write for themselves:

- `~/.claude/**/*.jsonl`, `~/.claude/projects/**`, `~/.claude/state/plan-runs/**`
- `~/.codex/sessions/**/rollout-*.jsonl`
- `~/.copilot/session-state/**`
- `~/.claude.json` (the installed-model cache)
- `~/.codex/auth.json` — only the e-mail claim of its ID token, to label which
  account the Codex usage belongs to; the token itself is never kept or shown

These are read on your machine and rendered in the app. They are never uploaded
anywhere by MQ Station.

### Files read only when you ask

- **`~/.ssh/config`** — when you import servers into the Servers sidebar. MQ Station
  writes to `~/.ssh` only if you turn on the export switch there (off by default,
  confirmed first): one `Include` line at the top of `~/.ssh/config`, a one-time
  `~/.ssh/config.mq-station-backup`, and an MQ Station-owned `~/.ssh/mq_station_config`. Turning
  the switch off removes exactly those.
- **`~/.local/share/atuin/history.db`** or **`~/.zsh_history`** — when you press
  import in Settings → Terminal → command history. Both are opened read-only.

Mail caught by the local SMTP listener is held in memory only (the last 200
messages) and is gone when MQ Station quits.

### Credentials — read, never displayed, never stored by MQ Station

MQ Station reads none of these credentials **for quota verification** before you say so. On
first run — or any time you have not yet decided — the quota surfaces touch no Keychain
item, no `~/.config/github-copilot/` file and no `~/.gemini/` file, and make no quota
request.

One credential read sits outside that decision, behind its own switch: turning on
**Copilot inline suggestions** (Settings → Editor, off by default) starts the Copilot
language server, which is handed the Copilot token from your Keychain or
`~/.config/github-copilot/`, and its sign-in status is checked against GitHub while
that section is open. With the toggle off, neither happens.

Once you grant quota verification, MQ Station reads tokens that the corresponding CLI
already put on your system (macOS Keychain, `~/.config/github-copilot/`, `~/.gemini/`,
environment variables) to show provider quotas. They are passed straight to the
matching provider API and are never written to MQ Station's database, logs or UI. MQ Station's
own settings never echo a secret back to the interface.

Your decision is stored locally, as the `usage.networkConsent` key in the `settings`
table inside `mq-station.db` (see "What MQ Station stores" above) — it never leaves your
machine. Revoke it any time from Settings, with the "Verify quota data online"
toggle; turning it off stops further credential reads and requests immediately and
clears the cached quota data.

---

## What leaves your machine

| Request | When | What is sent |
|---|---|---|
| `fonts.googleapis.com` | every launch | the Inter webfont request (no app data). Together with the update check below, this is the only request MQ Station makes without you configuring anything. |
| `github.com/MergeQuill/mq-station-releases/releases/latest/download/latest.json` | release builds only: at launch, then hourly | nothing from the app: a plain static GET (GitHub sees your IP, as with any download). The update bundle itself is fetched only after you confirm "Update". |
| `api.anthropic.com/api/oauth/usage` | only after you grant quota verification, then while a usage surface is refreshing | your existing Claude OAuth token, to read *your* quota |
| `api.github.com/copilot_internal/user` | only after you grant quota verification, then while a usage surface is refreshing | your existing Copilot token, to read *your* quota |
| `oauth2.googleapis.com` + `cloudcode-pa.googleapis.com` | only after you grant quota verification, then while a usage surface is refreshing, **and** `MQ_STATION_GEMINI_OAUTH_CLIENT_SECRET` is set in your environment | your existing Gemini OAuth refresh token from `~/.gemini/oauth_creds.json`, to read *your* quota. Without that variable the request is never made. |
| GitHub Copilot | only if you turn on Copilot inline suggestions: a sign-in status check while Settings → Editor is open, then the completions themselves as you type | your Copilot sign-in state, and the code context Copilot needs — exactly as in any other editor |
| Intelephense licence check | only if you enter a premium licence key | the key, validated once by *your* Intelephense process |
| `git fetch/pull/push` | when you press those buttons | whatever your git remote receives, using your own credentials |
| SSH | when you open a saved server | your system's own `ssh` binary connects with your own keys and agent; MQ Station implements no SSH and holds no credential |

MQ Station also **listens** on `127.0.0.1:2525` (loopback only, unreachable from other
machines) so a project can send its mail there instead of out to the internet.

Everything else — the terminal, the editor, the language servers, PHPStan, formatting,
git status/diff/commit — runs entirely offline.

**Agents run in your terminal.** When you launch Claude Code, Codex, Gemini, OpenCode,
Antigravity or Copilot CLI from MQ Station, that agent talks to its own provider under its
own privacy policy. MQ Station only starts the process and shows its output.

---

## What MQ Station never does

- No usage analytics, no feature flags, no A/B testing.
- No code, prompt, transcript or file path is sent to the author.
- No secret value is written to MQ Station's database, logs, notifications or error
  messages — secrets are referenced by location, never repeated.
- Release builds ship without the WebKit inspector surface.

---

## Removing everything

Delete the app and `~/Library/Application Support/com.mergequill.station/`. Nothing else
is left behind, and nothing exists off your machine to delete.
