---
name: heyvm
description: Interact with the heyvm CLI to manage sandboxes — create, start, stop, restart, list, exec commands, and more. Use when the user wants to manage sandboxes, run code in isolated environments, or work with micro-VMs.
argument-hint: "[subcommand] [args...]"
allowed-tools: Bash, Read, Grep
---

# heyvm CLI — Sandbox Manager

You are interacting with the `heyvm` CLI, the sandbox management tool for the Heyo platform. Use it to create and manage isolated sandbox environments backed by multiple runtimes.

## Binary Location

The binary is `heyvm`. If it is not on PATH, build it from `mvm-ctrl/`:

```bash
cargo build --release -p heyvm
```

## Available Subcommands

### Sandbox Lifecycle

| Command | Description | Example |
|---------|-------------|---------|
| `heyvm create` | Create a new sandbox | `heyvm create --name my-sandbox --type shell` |
| `heyvm start <id>` | Start an inactive sandbox | `heyvm start my-sandbox` |
| `heyvm stop <id>` | Stop a running sandbox | `heyvm stop my-sandbox` |
| `heyvm restart <id>` | Restart a sandbox | `heyvm restart my-sandbox` |

### Execution

| Command | Description | Example |
|---------|-------------|---------|
| `heyvm exec <id> <cmd...>` | Run a command in a sandbox | `heyvm exec my-sandbox -- python -c "print('hello')"` |
| `heyvm sh <id>` | Open interactive shell | `heyvm sh my-sandbox` |

### Listing

| Command | Description | Example |
|---------|-------------|---------|
| `heyvm list` | List running sandboxes | `heyvm list` |
| `heyvm list-inactive` | List stopped sandboxes | `heyvm list-inactive --count 20` |
| `heyvm images-list` | List pulled docker images | `heyvm images-list` |

### Other

| Command | Description | Example |
|---------|-------------|---------|
| `heyvm mount-add` | Add mount to a sandbox | `heyvm mount-add -i my-sandbox --host-path /tmp/data --sandbox-path /data` |
| `heyvm bind <id> <port>` | Proxy a sandbox port publicly | `heyvm bind my-sandbox 8080` |
| `heyvm pull <image>` | Pull a docker image | `heyvm pull ubuntu:24.04` |
| `heyvm wt <branch>` | Create git worktree sandbox | `heyvm wt feat/cool-feature` |
| `heyvm archive <id>` | Archive sandbox mounts to S3 | `heyvm archive my-sandbox --name backup` |
| `heyvm proxy <port>` | Expose local port over iroh P2P | `heyvm proxy 3000` |
| `heyvm connect <ticket>` | Connect to remote proxy | `heyvm connect heyo://...` |

## Create Options

```
--name <NAME>           Sandbox name (required)
--image <IMAGE>         Container image (default from settings)
--slug <SLUG>           URL-safe slug (default: slugified name)
--type <TYPE>           shell | python | node
--mount <HOST:SANDBOX>  Mount path (repeatable)
--ttl-seconds <SECS>    Time-to-live
--start-command <CMD>   Custom start command
--backend-type <TYPE>   msb | wasix | wasip2 | bubblewrap | libvirt | firecracker
```

## Backend Types

- **msb** — Microsandbox (default)
- **wasix** — WASIX WebAssembly
- **wasip2** — WASI Preview 2
- **bubblewrap** — Linux namespaces (Linux only)
- **libvirt** — QEMU/KVM via libvirt (Linux only)
- **firecracker** — Firecracker microVMs (Linux only)

## Workflow

When the user asks to interact with sandboxes:

1. **First, check what's running**: `heyvm list` to see active sandboxes.
2. **Create if needed**: Use `heyvm create` with appropriate `--type` and `--backend-type`.
3. **Execute commands**: Use `heyvm exec <id-or-slug> -- <command>` for non-interactive work.
4. **Use slug or ID**: All commands that take `<id>` also accept the sandbox slug.

When the user provides `$ARGUMENTS`, interpret them as a subcommand and arguments to pass directly to `heyvm`. For example, `/heyvm list` should run `heyvm list`.

If `$ARGUMENTS` is empty, ask the user what they want to do with their sandboxes.
