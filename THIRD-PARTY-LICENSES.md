# Third-Party Licences

Berth itself is MIT licensed (see [`LICENSE`](LICENSE)). This file lists the
third-party software Berth **redistributes** inside its macOS app bundle, and the
tools it merely *invokes* from your machine without shipping them.

Version numbers below refer to the pins in `package.json`,
`src-tauri/Cargo.toml` / `Cargo.lock` and `scripts/prepare-lsp-bundle.sh`. When
those pins move, update this file.

---

## 1. Redistributed inside the app bundle

### Node.js runtime (sidecar)

| Component | Version | Licence |
|---|---|---|
| [Node.js](https://nodejs.org/) | v22.23.1 | MIT (with the third-party notices in Node's own `LICENSE`) |

Fetched from `nodejs.org` by `scripts/prepare-lsp-bundle.sh` (SHA-256 verified
against the published `SHASUMS256.txt`), lipo'd into a universal binary and shipped
as a Tauri sidecar. It exists solely to run the bundled language servers.

### Language servers

| Component | Version | Licence |
|---|---|---|
| [typescript-language-server](https://github.com/typescript-language-server/typescript-language-server) | 5.3.0 | Apache-2.0 |
| [TypeScript](https://www.typescriptlang.org/) (`tsserver`, also Volar's `tsdk`) | 5.9.3 | Apache-2.0 |
| [@vue/language-server](https://github.com/vuejs/language-tools) (Volar, standalone 1.x) | 1.8.27 | MIT |
| [@tailwindcss/language-server](https://github.com/tailwindlabs/tailwindcss-intellisense) | 0.14.29 | MIT |
| [@github/copilot-language-server](https://www.npmjs.com/package/@github/copilot-language-server) | 1.519.0 | MIT |
| [Pyright](https://github.com/microsoft/pyright) (Python LSP) | 1.1.414 | MIT |

Their transitive dependencies (as installed by the pinned
`src-tauri/resources/lsp-servers/package-lock.json`) are MIT, ISC, Apache-2.0,
BSD-2-Clause and BSD-3-Clause. Each package keeps its own `LICENSE` file inside the
shipped `node_modules` tree.

> **Note — nested `@github/copilot` is removed, not shipped.** The Copilot
> language-server package vendors a copy of GitHub's Copilot CLI (`@github/copilot`)
> under `dist/node_modules/`, whose `package.json` declares
> `SEE LICENSE IN LICENSE.md` — GitHub's own terms, not MIT. Berth never loads it
> (only `dist/language-server.js` runs; the CLI is reached solely from `dist/main.js`),
> so `scripts/prepare-lsp-bundle.sh` deletes that directory during its prune step and
> asserts it stayed deleted. Nothing under it is redistributed.

### Frontend libraries (compiled into the app's JS bundle)

All MIT unless noted:

React, React DOM, Zustand, CodeMirror 6 (`@codemirror/*`, `@lezer/highlight`),
xterm.js (`@xterm/*`), Radix UI (`@radix-ui/*`, `@radix-ui/themes`), Phosphor Icons
(`@phosphor-icons/web`), Prettier, react-markdown, remark-gfm, json5, tauri-pty,
three.js, Tailwind CSS.

Dual-licensed: `@tauri-apps/api`, `@tauri-apps/plugin-dialog`,
`@tauri-apps/plugin-notification`, `@tauri-apps/plugin-opener`,
`@tauri-apps/plugin-process`, `@tauri-apps/plugin-updater` (MIT OR Apache-2.0).
ISC: `yaml`.

The authoritative, versioned list is `package.json` + `package-lock.json`.

### Rust crates (compiled into the native binary)

Tauri and its plugins (`tauri`,
`tauri-plugin-{opener,pty,dialog,notification,updater,process,global-shortcut}`),
`serde`, `serde_json`, `reqwest`, `mail-parser`, `rusqlite` (with a bundled SQLite — SQLite itself is public
domain), `git2` (with vendored libgit2), `notify`, `ignore`, `grep-searcher`,
`grep-regex`, `grep-matcher`, `trash`, `objc2` / `objc2-app-kit` /
`objc2-foundation`, `mac-notification-sys`.

These are MIT / Apache-2.0 / MIT-OR-Apache-2.0 (libgit2 is GPL-2.0-with-linking-
exception, which permits linking from differently licensed programs). The
authoritative list is `src-tauri/Cargo.toml` + `Cargo.lock`; `deny.toml` drives the
`cargo-deny` advisory check in CI.

### Fonts and icons

Phosphor Icons (`@phosphor-icons/web`, MIT) ships with the app — its webfont files
are bundled by Vite.

**Inter is *not* bundled.** `src/styles/tokens.css` imports it from Google Fonts at
runtime (`fonts.googleapis.com`), so the app fetches it over the network on launch
instead of redistributing it. Inter itself is SIL Open Font License 1.1. See
[`PRIVACY.md`](PRIVACY.md) — together with the release update check, this is the
app's only unsolicited outbound request.

---

## 2. NOT redistributed — resolved from your machine

Berth runs these only if you have installed them yourself; none is part of the
download:

| Tool | Why it is not bundled | How Berth finds it |
|---|---|---|
| [Intelephense](https://intelephense.com/) (PHP LSP) | Its licence forbids redistribution ("Reproduce, copy, distribute … except where permitted by INTENDED USE") | `PATH` lookup — `npm i -g intelephense` |
| [phpactor](https://phpactor.readthedocs.io/) (optional PHP refactors) | Opt-in, user-installed | `PATH` lookup |
| PHPStan / Larastan | Belongs to the analysed project | the project's own `vendor/bin/phpstan` |
| Laravel Pint / PHP-CS-Fixer | Belongs to the analysed project | the project's own `vendor/bin/…` |
| Claude Code, Codex, Gemini, OpenCode, Antigravity, GitHub Copilot CLI | These are the agents you already run | `PATH` lookup |
| `git` | System tool | `PATH` (network operations only; local reads use libgit2) |

A premium Intelephense licence key entered in Berth's settings is stored locally and
passed only to your own Intelephense process — see [`PRIVACY.md`](PRIVACY.md).
