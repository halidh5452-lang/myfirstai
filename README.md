# MYAI

MYAI is a terminal AI coding agent powered by [OpenRouter](https://openrouter.ai). It understands a request, inspects your project, plans, and then acts — reading and editing files, running commands, and verifying results — while keeping every operation inside your project root.

## Features

- **Interactive sessions** — chat with the agent in your terminal, with conversation memory (`clear` to reset).
- **One-shot mode** — run a single instruction and exit.
- **Planning first** — complex requests show a step-by-step plan before anything runs.
- **File tools** — list, read, write, edit, delete, create directories, and search files.
- **Project analysis** — auto-detects project type, config files, and entry points; builds a tree summary.
- **Shell tools** — run commands, check syntax, run tests and builds.
- **Git helpers** — status, diff, log, and branch, both inside sessions and standalone.
- **Safety by default** — commands are classified as safe/moderate/dangerous and require approval; `--dry-run` plans without touching files.
- **Secret redaction** — API keys and credentials found in tool output are masked before display.
- **Project rules & memory** — `.myai/rules.md` lets you pin project conventions the agent must follow.

## Requirements

- Node.js **>= 18**
- An [OpenRouter](https://openrouter.ai/keys) API key

## Installation

```bash
git clone <repo-url>
cd myai
npm install
npm run build
```

Link it globally so `myai` works anywhere:

```bash
npm link
```

Or run it locally with `npm start` (equivalent to `node dist/cli/index.js`).

## Quick start

```bash
myai doctor        # verify Node, npm, API key, and connectivity
myai init          # create .myai/ project config (config.json, rules.md, memory.md)
myai               # start an interactive session
```

First time? Run `myai config` to store your OpenRouter API key, or set it via the environment:

```bash
export OPENROUTER_API_KEY=sk-or-...
```

## Usage

```
myai                       Start interactive agent session
myai .                     Start session in current directory
myai "prompt"              Run a single instruction
myai chat                  Start interactive chat session
myai init                  Create .myai/ project config
myai config                Configure API key, model, options
myai doctor                Check environment and connectivity
myai git [status|diff|log|branch]   Git helpers
myai tree                  Show project tree
myai version               Show version
myai help                  Show this help
```

Options:

| Flag | Description |
| --- | --- |
| `--model <id>` | Override the model for this session |
| `--dry-run` | Plan only; no files modified |
| `--yes` | Auto-approve operations (trusted automation) |
| `--verbose` | Show more detail |
| `--debug` | Show debug output |
| `--depth <n>` | Tree depth for `myai tree` (default 4) |

Inside an interactive session:

| Command | Action |
| --- | --- |
| `exit`, `quit`, `:q`, `bye` | Leave the session |
| `clear` | Clear conversation context |
| `memory` | Show history length |

## Configuration

Configuration comes from environment variables, falling back to a persisted file at `~/.myairc.json` (kept readable only by you). Set values interactively with `myai config` or via the environment:

| Variable | Default | Description |
| --- | --- | --- |
| `OPENROUTER_API_KEY` | — | Your OpenRouter API key |
| `OPENROUTER_MODEL` | `nvidia/nemotron-3.5-lightning:free` | Model ID to use |
| `MYAI_TEMPERATURE` | `0.2` | Sampling temperature |
| `MYAI_MAX_TOKENS` | `4096` | Max response tokens |
| `MYAI_AUTO_APPROVE` | `false` | Skip confirmation prompts |
| `MYAI_MAX_RETRIES` | `5` | Retry attempts on failures |

See `.env.example` for a template (copy it to `.env`; never commit `.env`).

## Project config: `.myai/`

Running `myai init` creates:

- `.myai/config.json` — per-project settings marker
- `.myai/rules.md` — project rules the agent must follow (one per line)
- `.myai/memory.md` — persistent project memory

## Safety

- **Path confinement** — file operations are restricted to the project root; escaping paths are rejected.
- **Command classification** — shell commands are rated `safe` / `moderate` / `dangerous`; anything not safe requires explicit approval. Known-destructive commands (fork bombs, `shutdown`, `diskpart`, `mkfs`, force-push, etc.) are blocked outright.
- **Confirmation prompts** — deletes and risky commands always ask first, unless `--yes` / auto-approve is enabled.
- **Dry run** — `--dry-run` shows the plan without modifying anything.
- **Secret redaction** — credentials in agent output are masked.

## Development

```bash
npm run dev         # watch-mode compile
npm run typecheck   # tsc --noEmit
npm run build       # compile to dist/
npm start           # run the CLI
```

## License

MIT — see [LICENSE](LICENSE).
