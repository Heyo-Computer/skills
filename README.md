# heyvm — Claude Code Skill

A Claude Code skill for managing sandboxes via the `heyvm` CLI. Create, start, stop, and execute commands in isolated environments backed by multiple runtimes (containers, microVMs, WebAssembly).

## Usage

Invoke the skill from Claude Code with:

```
/heyvm [subcommand] [args...]
```

### Examples

```
/heyvm list                          # list running sandboxes
/heyvm create --name dev --type shell  # create a new sandbox
/heyvm exec dev -- python script.py   # run a command in a sandbox
/heyvm stop dev                       # stop a sandbox
```

## Supported Commands

| Command | Description |
|---------|-------------|
| `create` | Create a new sandbox (`--name`, `--type`, `--image`, `--backend-type`) |
| `start` | Start a stopped sandbox |
| `stop` | Stop a running sandbox |
| `restart` | Restart a sandbox |
| `exec` | Run a command inside a sandbox |
| `list` | List running sandboxes |
| `list-inactive` | List stopped sandboxes |
| `pull` | Pull a container image |
| `bind` | Expose a sandbox port publicly |
| `mount-add` | Add a mount to a sandbox |
| `wt` | Create a git worktree sandbox |
| `archive` | Archive sandbox mounts to S3 |
| `proxy` / `connect` | P2P port forwarding via iroh |

## Backend Types

- **msb** — Microsandbox (default)
- **wasix** — WASIX WebAssembly
- **wasip2** — WASI Preview 2
- **bubblewrap** — Linux namespaces
- **libvirt** — QEMU/KVM via libvirt
- **firecracker** — Firecracker microVMs

## Installation

The skill definition lives in `heyvm/SKILL.md`. The `heyvm` binary must be on your PATH, or built from the `mvm-ctrl` crate:

```bash
cargo build --release -p heyvm
```
