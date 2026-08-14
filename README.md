# casmux

[![CI](https://github.com/casapps/casmux/actions/workflows/ci.yml/badge.svg)](https://github.com/casapps/casmux/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/casapps/casmux)](https://github.com/casapps/casmux/releases)
[![License](https://img.shields.io/github/license/casapps/casmux)](LICENSE.md)

## About

casmux is a single-binary Rust terminal multiplexer — "tmux evolved for
today's developers." It rebuilds the top tmux plugins as native, built-in
features (no plugin/extension model to install or manage), adds a one-way
`tmux.conf` migration path, and layers on agent-orchestration capabilities
inspired by cmux and herdr so multiple AI coding agents (Claude Code, Codex,
OpenCode, Gemini, etc.) can run as first-class, status-aware panes alongside
ordinary shells. The core philosophy is "It Just Works": zero required
configuration, intelligent defaults, mobile-first responsive UI (30-column
phone terminal to 8K desktop), and a single self-contained binary with no
runtime dependencies.

> **Status:** casmux is currently spec-only (see `IDEA.md` / `AI.md`) — Rust
> implementation has not started yet. This README describes the shipped
> product as designed; commands below will work once the first release lands.

## Features

- Daemon/global-socket architecture: one always-on per-user daemon is the
  single source of truth for every session, window, and pane, with instant
  reattach instead of stale state
- Crash & reboot recovery: periodic autosave, last-N versioned snapshots per
  session, and a restore picker on next launch
- Unified session/window/pane switcher (`prefix+Tab`) with live, per-layer
  `1-9`/`0` numbering — no tmux-style flat overflow into letters
- Full notification pipeline: activity/silence monitoring, visual + audible
  bell, desktop notifications, and an in-app history — not just for agents
- Native built-in equivalents of the most popular tmux plugins — no
  plugin/extension model to install or manage
- Broadcast mode for SSH fleets and multi-agent groups, plus lightweight
  pane-sync and send-to-all-panes companions
- Agent orchestration: bind panes to Claude Code, Codex, OpenCode, Gemini, or
  any CLI agent, with live working/waiting/blocked/done/error status
- One-way `tmux.conf` import for migrating an existing tmux setup
- GUI + TUI + CLI, auto-detected at launch, with first-class X11 and Wayland
  support on the GUI surface
- Mobile-first responsive layout, from a 30-column phone terminal up to 8K

See `IDEA.md` for the full feature and design specification.

## Installation

```bash
# universal install script (curl/wget, OS+arch detection, sudo-aware)
curl -fsSL https://raw.githubusercontent.com/casapps/casmux/main/install.sh | sh

# Homebrew (macOS/Linux)
brew install casapps/casmux/casmux

# Debian/Ubuntu
curl -fsSL https://github.com/casapps/casmux/releases/latest/download/casmux.deb -o casmux.deb
sudo dpkg -i casmux.deb
```

Or download a prebuilt binary for your platform from the
[releases page](https://github.com/casapps/casmux/releases) — every release
ships linux/darwin/windows/freebsd/netbsd/openbsd across amd64/arm64.

## Usage

```bash
casmux new -n work            # start a new named session, detached
casmux attach work            # attach to it (never creates a new socket —
                               # reattaches to the live one, or triggers
                               # crash recovery if it's gone)
casmux list                   # list sessions (simple/detailed/JSON)
casmux import ~/.tmux.conf    # one-way migrate an existing tmux config
casmux kill work               # kill a session
casmux doctor                  # environment/self-diagnostics
```

Run `casmux --help` for the full command and flag reference.

## GUI/TUI/CLI mode behavior

Mode is auto-detected at launch: no `DISPLAY`/`WAYLAND_DISPLAY` or an active
SSH session selects TUI; launching from a desktop environment (file manager,
dock, `GIO_LAUNCHED_DESKTOP_FILE`, Finder, `explorer.exe`) selects GUI.
`--gui` / `--tui` override detection explicitly. CLI subcommands are always
available for scripting regardless of interactive mode.

The GUI surface treats **X11 and Wayland as equally first-class backends** —
neither is a fallback for the other. Display scale factor (HiDPI) is read
from the OS automatically. A single OS-level window is the default, with
sessions/windows navigated inside it; an individual session or window can be
popped out into its own window on request.

## Configuration

casmux works fully out of the box with zero configuration. When a config
file is used, casmux resolves it in the standard XDG-first search order and
supports `-f`/`--file <config>` and `-c`/`--config [name]` to select a
specific one explicitly. See `IDEA.md` → "Configuration" for the full
resolution order and supported keys.

## Development

All builds and tooling run inside Docker — there is no supported host-cargo
workflow.

```bash
# format check
docker compose run --rm dev cargo fmt --all --check

# lint
docker compose run --rm dev cargo clippy --workspace --all-targets --all-features -- -D warnings

# debug build
docker compose run --rm dev cargo build

# release build (artifacts land in a mounted target/ inside the container)
docker compose run --rm dev cargo build --release

# run the GUI with display forwarding (mounts X11/Wayland sockets)
docker compose run --rm gui cargo run -- --ui gui
```

See `docker/README.md` for image build details and `AI.md` → PART 5 for the
full toolchain and packaging spec.

## Testing

```bash
# full test suite
docker compose run --rm dev cargo test --workspace --all-features

# license/advisory/bans/sources compliance
docker compose run --rm dev cargo deny check licenses advisories bans sources

# GUI tests under X11 forwarding
docker compose run --rm gui cargo test --workspace --features gui-tests -- --test-threads=1

# GUI tests under Wayland forwarding
WAYLAND_DISPLAY=wayland-0 docker compose run --rm gui-wayland cargo test --workspace --features gui-tests -- --test-threads=1
```

CI runs the same containerized commands — see `.github/workflows/ci.yml`.

## License

MIT — see [LICENSE.md](LICENSE.md) for the full license text and third-party
attributions.

## Author

🤖 casjay: [GitHub](https://github.com/casjay) 🤖
