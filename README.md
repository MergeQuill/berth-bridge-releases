# Berth Bridge

**A terminal-first workspace for developers who work with AI agents.** macOS only.

Berth keeps every project you are working on in one window: split terminals that
survive, a file tree, git status and diffs, a real editor with language servers, and
the AI CLIs you already use — each running in its own git worktree, with its usage and
quota visible while it works.

**[Download the latest release](https://github.com/MergeQuill/berth-bridge-releases/releases/latest)**
— open the `.dmg` and drag Berth Bridge to Applications. It updates itself afterwards.

Berth Bridge is in **beta**.

---

## What it does

**Terminals that survive.** A split-pane tree per project, built on xterm.js and a
real PTY. Panes keep running when you switch projects, workspaces or tabs. Drag tabs
between panes; the terminal keeps its scrollback.

**AI agents, isolated.** Launch Claude Code, Codex, Gemini, OpenCode, Antigravity or
GitHub Copilot CLI into a dedicated git worktree, so an agent's changes never touch
your working tree until you have reviewed the diff and chosen to merge. Resume a past
session, chain headless steps into a pipeline, and get a clickable macOS notification
when an agent finishes or asks a question — clicking it focuses the terminal it came
from.

**Usage you can see.** Live token and quota panels for Claude, Codex, Gemini and
Copilot, read from the CLIs' own local logs. Session and weekly windows, cost per
turn, and a history of past conversations — including ones Berth never started.

**The Software Office.** Every open project as a team room on an isometric floor:
each agent sits at a desk with its state over its head, raises a hand when it has a
question, and walks to the lounge when idle. A side panel lists what is waiting on
you — questions, failed runs, work ready for review — and opens the agent's terminal
or its review desk (worktree diff, checks, merge or discard) without leaving the
floor. A menu-bar tray lists the same agents from anywhere on the Mac.

**A real editor.** CodeMirror 6 with bundled language servers for
TypeScript/JavaScript, Vue, Tailwind and Python (PHP needs a one-line install, see
below; Go and Rust use `gopls` / `rust-analyzer` when installed):
completion with auto-import, hover, signature help, go-to-definition and peek,
find-all-references, rename, quick fixes, inlay hints, semantic highlighting, error
lens, breadcrumbs, snippets and an outline. Project-wide search and replace with a
dry-run preview. Format on save.

**Git without leaving.** Status, per-hunk staging, commit, history, blame gutter,
stashes, worktrees, branch management, and a merge-conflict resolver. Diffs open as
editor tabs. A Git status page shows every project's branch and push state at once.

**Servers.** Saved SSH servers in their own workspace, grouped and importable from
`~/.ssh/config`, each opening as a tab with the same split panes. Berth runs your
system `ssh` and never stores a password or key.

**Notes and to-dos.** Per-project markdown notes in a live-preview editor, and a
task list with subtasks that can launch an agent prefilled with the task.

**Local dev helpers.** Laravel, Vue, Python, Go and Rust project detection; editable
quick commands; a dev server restarted when it crashes; readiness checks for
missing dependencies and pending migrations; PHPStan/Larastan diagnostics in the
editor; a loopback SMTP catcher on `127.0.0.1:2525`; a read-only Laravel Herd card;
and a searchable per-project command history (⌃R).

Everything is stored locally in SQLite, with an optional auto-backup to a folder of
your choice. There is no account, no sync and no telemetry — see
[PRIVACY.md](PRIVACY.md).

---

## Install

1. Download `Berth-Bridge_<version>_universal.dmg` from the
   [latest release](https://github.com/MergeQuill/berth-bridge-releases/releases/latest).
2. Open it and drag **Berth Bridge** to Applications.
3. Launch it. macOS asks once to confirm an app "downloaded from the Internet" — that
   is normal Gatekeeper behaviour for anything outside the App Store.

**Requirements:** macOS 11 or later, Apple Silicon or Intel (one universal build).
Nothing else to install: the Node runtime and the language servers ship inside the app.
The AI CLIs (Claude Code, Codex, Gemini, …) are not bundled — Berth launches the ones
already on your `PATH`.

The app is signed with an Apple Developer ID and notarized by Apple. Updates are
checked at launch and hourly against this repository, and every update package is
verified against a signing key built into the app before it is installed.

### Optional: PHP code intelligence

The TypeScript, Vue, Tailwind, Python (Pyright) and Copilot language servers ship inside the app —
nothing to install. **PHP is the exception:**
[Intelephense](https://intelephense.com/) is licensed in a way that forbids
redistribution, so Berth cannot bundle it. Install it once and Berth finds it on your
`PATH`:

```bash
npm i -g intelephense
```

Without it, PHP files still get syntax highlighting, PHPStan/Larastan diagnostics and
Pint formatting — only the LSP surface (completion, hover, go-to-definition,
references, outline) stays off. Settings → Editor → LSP shows whether Berth found it.
`phpactor`, the optional PHP refactor server, is likewise resolved from `PATH` and
never bundled.

---

## Feedback and bugs

Open an [issue](https://github.com/MergeQuill/berth-bridge-releases/issues/new/choose).
Security problems go through a private report instead — see [SECURITY.md](SECURITY.md).

## Privacy

No account, no sync, no telemetry. What the app reads and which network requests it
makes are listed in [PRIVACY.md](PRIVACY.md).

## Licence

MIT — see [LICENSE](LICENSE). Third-party components, and what Berth deliberately
does *not* redistribute, are listed in
[THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md).

This repository holds the release artifacts and the user-facing documents only.
