## Project description

casmux is a single-binary Rust terminal multiplexer — "tmux evolved for today's developers."
It rebuilds the top tmux plugins as native, built-in features (no plugin/extension model to
install or manage), adds a one-way `tmux.conf` migration path, and layers on
agent-orchestration capabilities inspired by cmux and herdr so multiple AI coding agents
(Claude Code, Codex, OpenCode, Gemini, etc.) can run as first-class, status-aware panes
alongside ordinary shells. The core philosophy is "It Just Works": zero required
configuration, intelligent defaults, mobile-first responsive UI (30-column phone terminal
to 8K desktop), and a single self-contained binary with no runtime dependencies.

## Project variables

    project_name:  casmux
    project_org:   casapps
    # FROZEN — set once at first-time setup, never edit
    internal_name: casmux
    # FROZEN — set once at first-time setup, never edit
    internal_org:  casapps
    app_name:      casmux

## Business logic

### App surfaces
- **GUI + TUI + CLI**, all in scope. Mode is auto-detected at launch: no `DISPLAY`/`WAYLAND_DISPLAY` or an SSH session → TUI; launched from a desktop environment (file manager, dock, `GIO_LAUNCHED_DESKTOP_FILE`, Finder, `explorer.exe`) → GUI. `--gui` / `--tui` explicitly override detection. CLI is always available for automation and scripting regardless of interactive mode. Per AI.md, the GUI surface must support both X11 and Wayland as first-class backends — neither alone is acceptable.
- CLI recognizes 20+ terminal emulators to help infer the right mode when launched ambiguously (e.g. from a terminal that was itself opened from a GUI launcher).
- **GUI windowing**: a single OS-level window is the default — sessions/windows/panes are navigated inside it, matching cmux and most modern terminal apps — but an individual session or window may be popped out into its own separate OS-level window on request. Display scale factor (HiDPI) is read from the OS and applied automatically, and further adjustable via the font-size keybindings (see "Keybindings").

### Core philosophy / non-negotiables
- **Zero-configuration**: full functionality out of the box; users are never required to read docs, write config, or install anything to get a working multiplexer.
- **No plugin/extension model.** Every capability — including everything drawn from tmux plugins, Zellij, cmux, and herdr — ships as a native built-in feature compiled into the single binary. There is no WASM plugin runtime, no plugin marketplace, and no user-loadable/`dlopen`-style extension mechanism. This is a deliberate product decision, not just the AI.md static-binary constraint.
- **Single self-contained binary**: no external runtime dependencies beyond the kernel and, for the GUI surface, display server sockets. All fonts, themes, and templates are embedded at build time.
- **Mobile-first responsive UI**: four breakpoints — Tiny (≤30 cols, mobile portrait), Small (31–50), Medium (51–80), Large (≥81) — with status-bar density scaling Hidden → Minimal → Compact → Standard → Full as width shrinks. Touch/gesture input (swipe, tap-to-focus, floating buttons, on-screen-keyboard awareness) is supported wherever the GUI runs on a touch-capable display. "Mobile-first" means responsive layout only — there is no separate native iOS/Android app (see "Non-goals").
- **Accessibility (GUI surface)**: fully operable via keyboard only — no action requires a mouse. At least one built-in theme is high-contrast/colorblind-safe by design. Full OS screen-reader support is a stretch goal, not a hard requirement, since the GUI renders its own glyphs rather than using native platform widgets, which limits what a screen reader can see.
- **Internationalization**: i18n scaffolding is built in from day one — every user-facing string routes through a localization-catalog system embedded in the binary at build time (per AI.md's self-contained-assets rule) — even though only an English catalog ships at first release. This avoids a costly string-extraction retrofit later.
- **Performance targets**: cold start <50ms, warm start <20ms, keystroke latency <10ms, 60fps rendering, <10MB base memory per session, <2MB per pane, <200B per scrollback line; must scale to 100+ concurrent sessions, 100+ windows per session, 50+ panes per window, 10,000 lines of scrollback per pane. Adaptive degradation under load: reduce animations and lazy-render when busy, trim scrollback past a memory ceiling, reduce update rate above 80% CPU, and offer a battery-saving mode (disable GPU rendering/animations, cap to 30fps) on battery power.

### CLI
- `attach` / `a`, `list` / `ls` (simple, detailed, and JSON output), `new` (`-n` name, `-d` detached, `-t` template, `-c` start dir), `kill <session|all> [--force]`, `import <tmux.conf> [-o]`, `update [--check-only] [--version]`, `doctor [-v]`, `completions <shell>`.
- Global flags: `-d`/`--debug`, `-f`/`--file <config>`, `-c`/`--config [name]`, `-v`/`--version`.

### Configuration
- Config resolution order: project-local → user → system → built-in defaults. No config file is required to run.
- Named, switchable config profiles (`-c <name>`).
- Live, searchable in-app settings UI — changes apply without restart.
- Auto-configuration by detected environment: truecolor support, SSH session, battery presence, git repository, and narrow-terminal compact mode.

### Session management
- First launch shows a welcome/tour flow.
- Attaches automatically if exactly one session exists; shows a picker when multiple exist.
- New sessions are project-type-aware by default (see "Project detection & templates").
- Full session state persists across detach/reattach and process restart: windows, panes, working directories, environment, command/search/clipboard history, layout, broadcast/zen-mode flags, git branch, and usage stats.
- Session groups, sharing, and locking; auto-detach on idle/disconnect; session preview, quick-switch, search, archive, export/import, and clone/merge; per-session activity/idle monitoring; session notes and tags; session history.
- Session namespaces: multiple independently addressable named sessions/servers on one host.
- Optional multi-client attach to the same session for shared/collaborative viewing — governed by the sharing model in "Roles & access control" below.
- Optional built-in web server exposing a browser-based terminal client for remote attach without a native client installed — governed by the web-UI auth model in "Roles & access control" below.
- casmux runs a per-user background daemon, auto-started on the first `casmux` invocation (analogous to tmux's server), that tracks every session on the host and each session's own scoped control socket. The optional web UI and any admin tooling talk to the daemon over a separate global control socket, not to individual session sockets directly. The global socket is scoped to the invoking OS user only (matching its file permissions) — it is never a system-wide/multi-user socket.
- The global socket is the single source of truth for live application state (which sessions/windows/panes exist, their PIDs, attach status) — per-session sockets handle that session's own control traffic, but tracking-and-restoration logic lives centrally at the daemon/global-socket level rather than being duplicated per session. This is what makes crash/reboot restoration (see "Crash & reboot recovery") and the web UI/admin view of "everything running" both driven off one consistent state source.
- Socket files and session-snapshot files live under a persistent per-user `{data_dir}` (`{data_dir}/sockets`, `{data_dir}/sessions`), never under a temp directory — unlike classic tmux's `/tmp`-based socket, `{data_dir}` is not cleared by a reboot or a distro's tmpfiles/tmp-cleaner policy, which is required for session state to actually survive a reboot (see "Crash & reboot recovery").
- A session's socket file and snapshot are removed from `{data_dir}` only when that session actually ends — killed by the user, or every pane/window in it has exited — never on a plain detach, since a detached session is still running and must remain restorable/reattachable. An unclean shutdown (crash/reboot) also leaves the socket/snapshot in place, by design, so "Crash & reboot recovery" has something to restore from; those are only cleaned up once the recovered session is later completed or explicitly discarded.
- The daemon behaves like a long-running server (tmux-server model): once started, it keeps running in the background for as long as at least one session exists, independent of any single `casmux` client attaching/detaching. When the last remaining session actually ends (not detaches), the daemon performs full cleanup of its own state — removing the global socket file and any leftover per-session entries under `{data_dir}/sockets` and `{data_dir}/sessions` — and then exits itself, so a fully-idle user is left with zero background processes and zero stale files. If the daemon itself dies uncleanly (crashes) instead of exiting normally, the next `casmux` invocation restarts it, and that restart sweeps `{data_dir}` for entries whose PID/socket no longer answers, reaping anything genuinely dead while leaving live/crash-recoverable sessions (per "Crash & reboot recovery") intact.
- A session's own socket is created once, eagerly, at session-creation time, and registered with the daemon immediately — not lazily on first external use. Attaching to an existing session never creates a new socket: it always reattaches to that session's existing socket if one is live, or triggers "Crash & reboot recovery" if the session exists in daemon state but its socket is gone.

### Crash & reboot recovery
- Session state (windows, panes, layout, working directories, environment, running command lines, broadcast/zen-mode flags, git branch) is snapshotted to local per-user storage on a short periodic autosave interval (default ~60s) plus immediately on every structural change (new/closed window or pane, split, layout change) — not only on a clean detach/exit. This bounds data loss from an unclean shutdown (crash, power loss, forced reboot) to roughly the autosave interval, unlike tmux-resurrect/continuum's much longer default.
- On the first `casmux` launch after an unclean shutdown is detected (no clean-exit marker from the last run, or a snapshot exists with no matching live daemon), casmux prompts with a restore picker rather than a blind yes/no — the user chooses **restore all**, **restore selected** (a checklist of the recovered sessions, sourced from the last daemon-known state; selection is whole-session granularity, not per-window/per-pane), or **restore none**. It never silently re-executes commands or re-spawns agent processes without this confirmation step, since a silently-relaunched agent could resume consuming API credits or a silently-rerun command could be destructive. Restoring none (or deselecting an item) discards that item's crash snapshot. This prompt fires identically for an actual crash and for a normal planned reboot — casmux has no reliable way to distinguish the two without an OS shutdown-hook dependency, and even a planned reboot silently re-executing agent processes/commands is the same surprise the prompt exists to avoid.
- `{data_dir}` (and the sockets/sessions under it) is assumed local to a single host, matching how tmux and other terminal multiplexers already behave; a shared/network home directory used from two different hosts at once is an unhandled edge case, not a designed-for scenario, since PIDs from one host are meaningless on another.
- Session snapshots are versioned, not a single continuously-overwritten file: casmux retains the last N autosaves per session (a fixed count, not a time window), so the restore picker can offer "go back to an earlier point" rather than only "the last state before death" — useful when the most recent autosave itself captured an already-broken state (e.g. right before a bad command). Older snapshots beyond the retention count are pruned automatically as new ones are taken.
- On confirmed restore, each selected window/pane is recreated with its original layout and working directory. Per pane: the daemon first checks its last-known PID against the live process table — if the original process actually survived the event (e.g. only casmux/the daemon crashed while the child process was reparented to init), casmux reattaches to that live process; otherwise it re-launches the same command line that was running, in the same directory. Agent panes relaunch the agent CLI, which then resumes its own conversation/session state via the existing agent-resume behavior (see "Agent orchestration"), not just terminal scrollback.
- SSH-backed panes reconnect automatically as part of restore, using the same connection-pooling/exponential-backoff behavior as a normal reconnect (see "SSH & remote access").
- This is the same local-storage trust boundary already declared under "Trust boundaries and constraints" — nothing about crash/reboot recovery changes what is persisted or where, only how promptly and how completely it is captured and replayed.

### Roles & access control
- **Session owner** — the OS user who created a session; full control over it, including granting/revoking shares.
- **Shared/invited viewer or collaborator** — a second client (local or via the web UI) attached to an existing session via an explicit share token issued by the owner; read-only by default, promotable to read-write by the owner. Share tokens are short-lived and independently revocable.
- **Agent process** — a coding agent bound to a pane; holds a socket credential minted per-session, valid only for spawning/reading/writing panes and waiting on completions within that one session (see "Agent socket isolation" below).
- **Remote/admin operator (global token holder)** — whoever holds the daemon's global web-UI token; the highest-privilege non-owner role, since it can reach any session on the host through the daemon. Generated once, stored with owner-only file permissions, rotatable/regenerable on demand.
- **Session sharing model**: sessions are private to their owner by default (Unix socket file permissions, matching tmux/herdr precedent) — there is no "any local user with socket access can attach" default. Sharing requires the owner to explicitly issue a per-session share token.
- **Web UI auth**: disabled by default. When enabled, binds to `127.0.0.1` on a random port unless the user overrides the bind address/port; a global access token is always required to attach (embedded in the attach URL), valid for every session on the host since the web UI talks to the daemon rather than one session.
- **Agent socket isolation**: each agent-bound pane's socket credential is scoped to its own session by the daemon; the daemon rejects it for any other session on the host.
- Every access-control default above (share tokens, web-UI token, per-session agent scoping) is a sane default, not a hard requirement — each is explicitly disable-able by the user (e.g. a `--no-auth`/config opt-out) for anyone who wants the simpler classic-tmux shared-socket trust model. Disabling always requires an explicit, deliberate opt-out; it is never the default.

### Abuse cases & threat model
casmux must ship a sane, on-by-default (but user-disable-able) mitigation for each of the following:
- **Malicious/compromised agent process** — an agent-bound pane's process (compromised, or steered via prompt injection) attempts to use its socket credential to reach panes/sessions outside its own scope. Mitigated by per-session socket-token scoping.
- **Broadcast-mode mis-target** — a user fat-fingers broadcast mode and sends a destructive command or a secret to the wrong host/agent set. Mitigated by the confirm dialog, exclude-local-by-default, and the homogeneous-target-type grouping described in "Broadcast mode".
- **Malicious remote SSH peer** — a remote host casmux connects to (or accepts SSH-forwarded control from) is compromised or hostile. It must not be able to pivot into controlling the local daemon, other sessions, or other panes beyond the single pane/session it is attached to.
- **Web UI token leakage/replay** — the global or per-session share token leaks (browser history, shoulder-surfing, a shared clipboard/log) and is replayed by an unauthorized party. Mitigated by tokens being independently revocable/regenerable and by per-share tokens (viewer role) being shorter-lived than the daemon's global token.
- Every mitigation above is a default, not a lock: the user may disable auth/scoping/confirmation entirely if they deliberately choose the simpler classic-tmux trust model instead.

### Keybindings
- Prefix-key model by default: **`Ctrl-Space` is the primary default prefix, `C-b` is the built-in secondary/fallback prefix** (both active simultaneously out of the box, not an either/or choice) — fully rebindable, and either one may be disabled individually. Every action below has a defined default so nothing ships unbound. Defaults are grounded in real, battle-tested convention rather than invented arbitrarily: first tmux's own defaults, then vim/emacs inside copy mode, and where a genuinely popular personal/community convention diverges from stock tmux in a way that's clearly deliberate (not a stock tmux install), that convention wins — recognizing that most tmux users run a customized config, not the stock one.
- **`Ctrl-Space` as primary prefix — known tradeoff, accepted deliberately:** on some Linux/Windows desktops with an IME installed (ibus, fcitx, and similar), `Ctrl-Space` is a common system-wide input-method-toggle grab that can be intercepted before it ever reaches a terminal at all, which would silently swallow the entire prefix on those setups. This is accepted as a conscious tradeoff for matching the muscle memory of Ctrl-Space-based tmux/Zellij configs — the built-in `C-b` secondary prefix exists specifically as the always-available fallback for exactly that case, so a user on an IME-affected desktop is never actually locked out, just needs to reach for the secondary instead of rebinding anything.
- **System/desktop-binding avoidance rule:** a default is only assigned to a raw (non-prefixed) key combo when casmux is guaranteed to receive that keystroke before any OS, window manager, desktop environment, IME, or host terminal emulator can intercept it. Prefix-chord bindings (`prefix` + anything) and copy-mode bindings always qualify — once a pane is running in raw terminal mode, casmux owns every keystroke and no kernel-level signal or line-discipline processing intervenes (this is the same guarantee tmux/screen have relied on for decades, including for chords like `C-b`/`C-a`/`C-z` that also happen to be readline or job-control defaults outside a multiplexer). Bare, non-prefixed combos are used only where no popular WM/DE/IME grabs them by default (e.g. plain `F1`–`F12`, `Ctrl-P`, `Ctrl-F`, `Shift+Left`/`Shift+Right`). Combos with a well-known conflicting system/DE claim (`Alt+<number>` — common tiling-WM/taskbar workspace switch; bare `Ctrl-Space` used as a standalone, non-prefix action binding — common IME toggle on Linux/Windows) are deliberately avoided as *bare, standalone action* bindings even though they're popular in other tools, and prefix-scoped alternatives are used instead; this rule governs individual action bindings, not the prefix key itself, which is addressed by the dual-prefix fallback above.
- Vim-style `Ctrl-hjkl` pane navigation that passes through to vim inside the pane when appropriate (no prefix needed).
- Always-visible on-screen key/mode hint bar so available actions are discoverable without memorizing bindings.

**Default prefix-chord table** (`prefix` = `Ctrl-Space`, secondary `C-b`):

| Chord | Action |
|-------|--------|
| `c` | New window (retains current pane's working directory) |
| `n` / `p` | Next / previous window |
| `` ` `` | Jump to last-active window (toggle) |
| `1`–`9`, `0` | Select the 1st–10th visible window (display position, always `1-9` then `0` for the 10th — independent of the window's own configured/imported `window_base_index`, which still governs numbering shown in the status bar, `casmux list`, etc.) |
| `,` | Rename window |
| `@` | Kill window (confirm) |
| `x` | Kill pane (confirm) |
| `\` | Split pane left/right (new pane on the right) |
| `/` | Split pane top/bottom (new pane below) |
| `h`/`j`/`k`/`l` (repeatable) | Resize focused pane |
| `Ctrl+←↑↓→` | Resize focused pane by 1 unit |
| arrow keys | Move focus between panes |
| `z` / `+` | Toggle pane zoom |
| `!` | Break pane into its own window |
| `{` / `}` | Swap pane left / right |
| `J` | Join every other window in the session into the current window, as panes |
| `B` | Break every pane in the current window back out into its own window (inverse of `J`) |
| `f` / `F` | Toggle a floating pane / toggle the focused pane between floating and tiled (see "Window & pane management") |
| `o` | Enter move-pane mode — `h`/`j`/`k`/`l` then repositions the focused pane |
| `d` | Detach from session |
| `N` | Prompt for a name and create a new session |
| `r` | Prompt for a target session and relocate every window of the current session into it |
| `R` | Reload config |
| `t` | Toggle the file-tree sidebar (see "File-tree sidebar") |
| `T` | Focus the file-tree sidebar |
| `w` | Prompt for a layout by name: `tiled`, `even-horizontal`, `even-vertical`, `main-horizontal`, `main-vertical` |
| `Space` | In a nested casmux (or tmux) session, forward one literal prefix keystroke to the inner session — the standard nested-multiplexer passthrough (see "Nested sessions & zen mode") |
| `Tab` | Open the unified session/window/pane switcher, rooted at the sessions layer (see "Unified switcher") |
| `Shift+Tab` | Open the same unified switcher, rooted directly at the current session's windows/panes (skips the sessions layer) |
| `$` | Rename session |
| `[` | Enter copy mode |
| `]` | Paste from clipboard buffer |
| `Ctrl-v` | Paste from the OS system clipboard directly (distinct from the internal buffer paste above) |
| `s` | Toggle pane-sync (broadcast keystrokes to every pane in the current window only) |
| `Ctrl-p` | Prompt once for a command and send it to every pane in every window of every session (global, not just the current one) |
| `b` | Open the broadcast target picker (cross-session/SSH/agent-aware — see "Broadcast mode") |
| `m` | Toggle mouse support |
| `Ctrl-n` | Toggle do-not-disturb (mute audible/visual/desktop alerts — see "Notifications & alerts"). Not plain `N`, since `N` is already used above for new-session. |
| `Ctrl-s` / `Ctrl-r` | Manually save a session snapshot now / manually restore a session from its saved snapshot (explicit trigger, in addition to the automatic autosave and crash-prompt flows in "Crash & reboot recovery") |
| `?` | Open help (alternate to `F1`) |
| `a` | **Toggle agent panel** — show/hide the sidebar listing agent-bound panes and their status (working/waiting/blocked/done/error) |

**Default global (non-prefixed) bindings:**

| Key | Action |
|-----|--------|
| `F1` | Open help |
| `F2` | **Toggle agent panel** — global equivalent of `prefix+a`, for reaching it without the prefix chord |
| `Ctrl-P` | Command palette |
| `Ctrl-F` | Global search |
| `Ctrl-Z` | Toggle zen mode |
| `Shift+Left` / `Shift+Right` | Previous / next window, without needing the prefix |
| `Ctrl-=` / `Ctrl-+` / `Ctrl--` / `Ctrl-0` | Increase / increase / decrease / reset font size — **GUI surface only.** In TUI mode casmux is a guest inside the host terminal emulator, which almost always intercepts these combos itself for its own zoom before casmux ever sees them; casmux does not claim a TUI font-size binding and instead defers to the host terminal's own zoom controls. |

**Copy-mode bindings** (entered via `prefix+[`; default style is `vi`, switchable to `emacs` via the `copy_mode` setting):
- vi style: `h`/`j`/`k`/`l` move, `v` or `Space` start selection, `y` or `Enter` yank/copy selection, `Ctrl-c` copy selection straight to the OS system clipboard, `/` search forward, `?` search backward, `n`/`N` repeat search forward/backward, `Ctrl-d`/`Ctrl-u` half-page down/up, `Ctrl-f`/`Ctrl-b` full-page down/up, `g`/`G` jump to top/bottom, `e` edit the scrollback in `$EDITOR`, `Esc` or `q` exit copy mode. Mouse drag also selects and copies on release (see "Mouse support"). `Ctrl-\` (outside copy mode too) jumps to the last-focused pane.
- emacs style: arrow keys move, `Space` start selection, `Alt-w` copy selection, `Ctrl-s` search, `Esc` or `q` exit copy mode. (Real GNU Emacs uses `Ctrl-Space` for set-mark, but that combo is a common IME-toggle shortcut on Linux and Windows desktops and gets consumed before reaching any app in that case, so `Space` is used instead.)

### Unified switcher (sessions/windows/panes)
- `prefix+Tab` opens a single tree-style switcher spanning all three levels of the hierarchy — sessions, windows, panes — modeled on tmux's `choose-tree`, but always sourced live from the daemon/global socket (the single source of truth for what's actually running), so the tree is never stale relative to what another client, an agent, or a crash-restore just changed. `prefix+Shift+Tab` opens the identical UI already rooted at the current session (mirrors tmux's `choose-tree -Zw`).
- **Numbering restarts per layer instead of overflowing into a single continuing sequence, and always runs `1–9` then `0` for the 10th item.** Classic tmux `choose-tree` numbers sessions `0`–`9` and then keeps counting into letters (`M-a`, `M-b`, ...) as it flattens windows into the same list — casmux instead gives each layer its own independent `1–9`-then-`0` numbering (matching the physical top-row key order, not tmux's/each window's own configured `window_base_index`, which is deliberately ignored here for UI simplicity): the sessions layer is numbered `1–9`/`0`, and drilling into a session's windows restarts that session's own windows back at `1–9`/`0`, and drilling into a window's panes restarts again at `1–9`/`0` for that window's own panes.
- Selection works two ways: typing a number (optionally followed by Enter) jumps straight to that entry at the current layer, or `Up`/`Down` moves a highlight through the current layer's list and `Enter` opens/selects the highlighted entry — both are always available, not an either/or mode.
- `Left`/`Right` collapse/expand the highlighted node where applicable (a session or window with children) — `Left` collapses it back to its own summary line, `Right` expands it to show its children in place; `Space` also toggles collapse/expand as an alternate binding for the same action. On a leaf entry (a pane, or a collapsed-with-no-children node) `Left`/`Right` are no-ops.
- The same switcher UI, keybind, and per-layer numbering rule are reused when opened from inside an already-attached session, but scoped to just that session's own windows and panes (talking to that session's own per-session socket) instead of the whole daemon-known tree of every session — i.e. the identical "per-socket vs. global" view the daemon's socket architecture (see "Session management") already supports, exposed through one consistent picker.
- This unified switcher is additive to, not a replacement for, the other session/window navigation bindings above (`` prefix+` ``, `prefix+n`/`p`, `prefix+1`-`9`/`0`).

### File-tree sidebar
- `prefix+t` toggles a directory-tree sidebar panel showing the focused pane's working-directory tree; `prefix+T` focuses it for keyboard navigation. A second built-in sidebar alongside the agent panel (`prefix+a`/`F2`), using the same collapsible-panel styling.
- Selecting a directory in the tree changes the focused pane's working directory to it; selecting a file opens it in `$EDITOR` (or the platform-appropriate default opener) in the focused pane.

### Status bar
- Single-line bottom bar, fully configurable left/center/right component slots: mode indicator, session name, `user@host`, git branch/status, uptime, load/mem/CPU, opt-in weather, battery, network, date/time, and continuum/zoom/broadcast indicators.
- Mode indicator changes color per mode (prefix, copy, broadcast — blinking, zen).
- Per-window flag icons in the tab/status strip (bell, activity, silence, zoomed) — see "Notifications & alerts" for what sets each flag.

### Notifications & alerts
Every alert-worthy event in casmux — not just agent events — funnels through one consistent pipeline: a per-window status-bar flag, an optional visual flash, an optional audible bell, an optional OS desktop notification, and always an entry in the consolidated in-app notification panel/history (see "Agent orchestration"). Each stage below is individually configurable; none are hidden or silently forced.
- **Activity monitoring**: a window/pane not currently focused is flagged when it produces new output — on by default (unlike vanilla tmux's `monitor-activity`, which defaults off), since surfacing background activity without hunting for it fits casmux's zero-config philosophy; disable per-window or globally.
- **Silence monitoring**: the inverse — flag a window/pane that has produced *no* output for a configurable idle threshold (mirrors tmux's `monitor-silence`), useful for noticing a long-running build/deploy has gone quiet or finished. Off by default (requires a meaningful threshold to be useful), one config value to enable.
- **Bell handling**: a terminal bell (`BEL`) from any pane's process is caught and routed rather than passed straight to the host terminal. Scope is configurable to any window / current window only / other windows only / none (mirrors tmux's `bell-action` states).
- **Visual bell**: a bell event flashes the affected pane's border (TUI) or a brief on-screen flash animation (GUI) instead of, or in addition to, the audible bell — configurable independently of it.
- **Audible bell**: passes through as the host terminal's own bell sound in TUI mode; in GUI mode plays the OS's standard notification sound. Can be silenced independently of visual bell and desktop notifications.
- **Desktop notifications**: OS-level notification popups, individually toggleable per event source — bell, activity flag, silence flag, a command finishing (especially a long-running one, or one that exited non-zero), an SSH pane disconnecting/reconnecting, a broadcast target dropping out — in addition to the existing agent-attention/agent-done events.
- **Do-not-disturb**: `prefix+Ctrl-n` toggles a single mute switch (status-bar indicator shown) that suppresses audible bell, visual flash, and desktop notifications all at once without disabling the underlying monitoring — events still land in the notification history, just silently, so nothing is lost while focused/presenting/in a meeting.

### Copy & paste
- vi and emacs copy-mode key styles; smart selection; mouse drag-select and double-click word select; keyboard copy-mode navigation (h/j/k/l).
- Built-in clipboard manager with history UI.
- OSC 52 for remote/SSH clipboard integration.

### Broadcast mode
- Type once, send to many panes simultaneously. Targets may be SSH panes, agent-bound panes, or both — but the target set is always homogeneous-by-declaration: casmux groups auto-detected candidates by type (SSH vs. agent) and requires the user to explicitly opt into a mixed-type broadcast rather than silently merging them, since an SSH pane expects shell commands and an agent pane expects natural-language prompts.
- Auto-detects SSH panes as broadcast candidates; separately, agent panes sharing the same project/role context are suggested as an agent-broadcast group.
- Confirmation dialog before enabling always lists the exact target types and counts (e.g. "3 SSH panes" vs. "3 SSH panes + 2 agent panes"); excludes local, non-broadcast-eligible panes by default as a safety measure.
- Visual pane-border severity indicator scaled to the number of broadcast targets.
- Auto-suggests broadcast mode when 3+ similar SSH hosts are detected (e.g. `web-01`/`web-02`/`web-03`), or when 2+ agent panes share the same project/role context.
- Two lighter-weight, always-available companions to full broadcast mode (see "Keybindings"): `prefix+s` toggles synchronize-panes for just the current window's own panes (no target picker, no confirmation dialog — matches tmux's built-in `synchronize-panes`), and `prefix+Ctrl-p` prompts once for a single command and fires it at every pane in every window of every session server-wide, for one-off fleet-style commands that don't warrant setting up a persistent broadcast group.

### Search
- Global fuzzy search (`Ctrl-F`) across sessions/windows/panes.
- Per-pane scrollback search.

### Project detection & templates
- Auto-detects project type from manifest files or VCS root: Rust, Node.js, Python, Go, Ruby, Java, .NET, PHP, Docker, Kubernetes, Terraform, Ansible.
- Built-in session templates per detected language, plus a "servers" multi-host template (grid/tall layouts for fleet management).
- Custom user-defined templates with lifecycle hooks (`on_create`, `on_attach`, `on_detach`).

### Built-in feature registry (native tmux-plugin parity — no install step)
- Session-level: resurrect, continuum (auto-save/restore), session templates, session sharing, session locking, auto-detach, session groups/preview/quick-switch/search/archive/export/import/clone/merge, activity monitor, idle detection, session notes/tags/history.
- Window/pane-level: smart splits, auto-balance, styled pane borders, enhanced zoom (with saved-layout restore), layout save/templates, window/pane swap, break/join, window groups, tab bar, window preview, auto-rename from running process (e.g. `vim:file`, `ssh:host`, `cargo:cmd`), window search, quick jump, window/pane tags, focus events, window/pane lock, layout cycling.
- Layout presets: Even, Tall, Wide, Main, Grid, plus fully Custom layouts.
- These are representative of a larger native registry spanning Core, Navigation, Status Bar, Clipboard, Development, File Handling, SSH, Productivity, Visual, and Automation categories — the point being every one of them ships built-in, none require a plugin install.

### Themes & fonts
- 10 built-in color themes (Dracula default dark, Gruvbox, Nord, Solarized, Monokai — each with a light variant); `auto` theme detects terminal/OS background.
- 5 embedded Nerd Fonts: Cascadia Code (default), JetBrains Mono, Hack, FiraCode, Iosevka. No external font installation required.

### Mouse support
Full mouse support is a first-class, always-available surface (GUI natively; TUI via standard terminal SGR mouse-reporting), toggled as a whole via `prefix+m` (see "Keybindings") — when off, casmux stops intercepting the mouse entirely and the host terminal's own native text selection/copy takes back over.
- **Focus & selection**: single-click a pane focuses it without disturbing its content; click-drag selects text (extends the built-in clipboard manager, see "Copy & paste") and copies to it on release; double-click selects a word, triple-click selects a line; shift-click extends the current selection.
- **Scrolling**: wheel-scroll over a pane that isn't already in copy mode enters copy-mode scrollback automatically (no `prefix+[` needed) and scrolls it; scrolling back down to the live bottom exits copy mode automatically. Smooth/natural (content-follows-finger) scrolling direction, matching the host OS's configured scroll direction.
- **Resizing**: click-drag a pane border resizes the adjacent panes; drag-resize snaps to the nearest sensible split ratio near round percentages.
- **Drag & drop**: drag a pane's title/border onto another pane to swap their positions; drag a pane out to empty space to break it into its own window (mirrors `prefix+!`); drag a window's tab in the status bar/tab strip to reorder it, or onto another window to merge/move the pane there.
- **Status bar / tab strip clicks**: click a window's name to switch to it directly; click the empty area after the last tab to create a new window (mirrors `prefix+c`); middle-click a tab closes that window (confirm, mirrors `prefix+&`); right-click a tab opens a context menu (rename, kill, move to new session).
- **Pane right-click context menu**: copy, paste, copy-path, open-url, split (vertical/horizontal), zoom, kill pane, swap pane, add/remove from current broadcast group.
- **Modifier-click**: `Ctrl+Click` (`Cmd+Click` on macOS) on a detected URL or file path opens it directly, without disturbing normal click-to-position-cursor and double-click-select-word behavior on the same text. `Ctrl+Click` on additional panes while broadcast mode is armed adds/removes them from an ad-hoc broadcast target set, as an alternative to the automatic homogeneous grouping (see "Broadcast mode").
- **Middle-click paste**: pastes the platform primary selection on Linux/X11 (matching the platform convention); on platforms without a primary selection (macOS, Windows) middle-click is unbound rather than repurposed, since no popular convention exists there to follow.
- **Agent panel**: click an entry in the agent-status sidebar (`prefix+a`/`F2`) to jump focus to that pane; drag its edge to resize the panel.
- **Unified switcher** (see that section): click a row to highlight it, click again (or double-click) to select/open it — the mouse equivalent of `Enter`; click a session/window row's expand/collapse chevron to toggle it — the mouse equivalent of `Left`/`Right`/`Space`.
- **GUI-only extras**: `Ctrl+Scroll` zooms font size (mouse equivalent of the `Ctrl-=`/`Ctrl--` keybindings — GUI surface only, same rationale as the keybind).
- Touch-input equivalents of the above (tap-to-focus, swipe-scroll, floating action buttons) are covered under "Core philosophy / non-negotiables → Mobile-first responsive UI".

### Command palette
- `Ctrl-P` fuzzy command launcher covering every user-facing action.

### Help system
- Interactive, context-aware, searchable, with visual ASCII examples and related-topic links — no external docs required to discover a feature.

### Error handling & recovery
- Typed error conditions (terminal too small, corrupted session file, connection lost, config error, command failed), each with a defined recovery strategy (auto-recover, prompt user, fallback, log-only, fatal).
- Crash handler saves state on unexpected exit and offers restore on next launch.

### Update system
- Self-update from GitHub releases; `--check-only` mode; pin to a specific version; atomic binary replace with automatic backup of the previous binary.
- A downloaded release artifact must pass both a SHA-256 checksum check and a signature verification against the published release before the atomic replace proceeds; any mismatch aborts the update and leaves the current binary running untouched.

### tmux migration (compatibility requirement)
- `casmux import <path/to/tmux.conf> [-o]` performs a one-way import of an existing tmux configuration.
- Direct setting mapping: `prefix` → `prefix_key`, `mouse`, `mode-keys` → `copy_mode`, `escape-time`, `repeat-time`, `base-index` → `window_base_index`, `pane-base-index`, `renumber-windows`, `set-titles`, `default-terminal`.
- The following tmux plugins are recognized and converted to their casmux built-in equivalents on import: tmux-resurrect, tmux-continuum, tmux-yank, tmux-copycat, tmux-open, tmux-battery, tmux-cpu, tmux-prefix-highlight, tmux-sidebar, tmux-sessionist, vim-tmux-navigator, tmux-fzf, tmux-jump, tmux-thumbs.

### Invocation-based compatibility modes (argv[0])
- casmux must be usable as a **drop-in replacement binary** for tmux, GNU screen, or Zellij — installed as (or symlinked to) `tmux`, `screen`, or `zellij` on a machine, with no other change to that machine's existing scripts, dotfiles, or muscle memory. This is a separate requirement from "tmux migration" above (one-way `.conf` import into casmux's own format) — this is casmux **becoming** that program for scripting and muscle-memory purposes, not converting away from it.
- **Detection**: at startup, casmux inspects the invoked command name (`argv[0]`) — the name it was actually run as, whether that's the real binary name or a symlink pointing at the casmux binary. If it resolves to `tmux`, `screen`, or `zellij`, casmux enters that program's compatibility mode for the entire process lifetime; if it resolves to anything else (including `casmux` itself), casmux runs in its own native mode with its own defaults.
- **Compatibility is "1:1+", not a stripped-down clone — it's a compatible shell around the same underlying casmux, not a different, lesser program.** Underneath every compat mode, the session is still a real casmux session: the same daemon/global-socket architecture, the same crash/reboot recovery, the same unified switcher, notifications pipeline, and agent orchestration all remain fully live and fully available, exactly as in native mode — nothing about the actual engine is disabled, hidden, or feature-gated just because casmux was invoked under a compat name. Compat mode narrows what's needed for **fidelity** (scripting surface, config parsing, default keybindings), it does not narrow the product.
- **What compat mode guarantees, per target:**
  - **CLI compatibility**: accepts that program's actual command-line syntax, subcommands, and flags as documented by the upstream project, so existing shell scripts, aliases, and automation invoking `tmux ...`/`screen ...`/`zellij ...` keep working unmodified. casmux's own CLI syntax remains available too (see "CLI") — this is additive, not a replacement of one syntax by the other.
  - **Config file compatibility**: reads that program's native config format directly at its native default path (`tmux.conf`, `.screenrc`, `zellij` `config.kdl`/layout `.kdl` files) — parsed natively, not through the one-way import path — in addition to, not instead of, casmux's own config format.
  - **Keybinding compatibility**: default keybindings match that program's own stock defaults (not casmux's own defaults from "Keybindings" above), so a user's existing muscle memory and existing config's keybinds resolve the way they expect. casmux-native actions that have no equivalent in the target program are simply additional bindings layered on top, not a source of conflict.
  - **On-disk/socket path compatibility**: session/socket paths and naming follow that program's own conventions closely enough that other tooling expecting to find/signal/attach to a real `tmux`/`screen`/`zellij` process (scripts, status-bar integrations, IDE terminal panels) continues to work.
  - **`$TERM` compatibility**: panes see `TERM` set to that target's own convention (`tmux-256color`, `screen-256color`, whatever Zellij's own passthrough sets) rather than casmux's native `TERM` value, since scripts and nested programs commonly branch on `$TERM`.
- Because the underlying engine never changes, a user can freely reach for any casmux-native feature (unified switcher, agent panel, broadcast mode, etc.) at any time while running under a compat-mode invocation — compat mode guarantees the target program's behavior still works, it never removes casmux's own.
- This compatibility requirement applies independently per target — casmux supporting `tmux` compat mode does not imply anything about `screen` or `zellij` compat mode's completeness or vice versa; each is its own 1:1 fidelity bar against its own upstream project.

### Terminfo
- casmux ships its own terminfo entries (`casmux`, `casmux-256color`) describing its actual escape-sequence capability set, and sets `TERM` to one of them inside panes in native mode — matching the same reason tmux ships `tmux`/`tmux-256color` and screen ships `screen`/`screen-256color`: nested programs (vim, less, htop, etc.) negotiate capabilities off `$TERM`, and lying about it (e.g. claiming `xterm-256color`) risks a nested program using sequences casmux doesn't actually pass through. In an argv[0] compat mode, `TERM` instead follows that target's own convention (see "Invocation-based compatibility modes" → "`$TERM` compatibility"), not casmux's native entry.
- Per AI.md's self-contained-assets rule, the terminfo source is embedded in the binary at build time (not a separate file the user must obtain). On first run with root/admin privileges (installer, package post-install step, or first sudo/elevated invocation), casmux compiles and installs the entry **system-wide** to the platform's standard terminfo location (`/usr/share/terminfo` on Linux, the equivalent on macOS/BSD/Windows) — a one-time step covering every user and every shell on that machine, the same way tmux's and screen's own packages install their entries system-wide rather than per-user. If casmux is ever run without the privilege to write there (e.g. an unprivileged user on a machine where no elevated install step has happened yet), it falls back to installing into that user's own per-user terminfo directory (`$TERMINFO`/`~/.terminfo`) so native mode still works immediately — no separate manual `tic` step or external package required from the user either way.
- **Zero-config `TERM` handling**: the user never manually sets `TERM=casmux*` themselves — casmux exports the correct value automatically for every pane it spawns (native entry, or the active compat target's convention), consistent with the project's zero-configuration philosophy. Nothing about picking or exporting `TERM` is a setup step.
- **The entry declares the full modern capability set, not a conservative lowest-common-denominator one**: 24-bit truecolor, UTF-8 and wide-glyph rendering (emoji, Nerd Font icons), italics, bracketed paste, focus events, synchronized-output, and full mouse reporting are all advertised, so nested programs get full functionality by default with no extra per-app flags or user tuning required.

### Nested sessions & zen mode
- casmux sessions may run nested inside another casmux (or tmux) session with correct prefix-key disambiguation.
- Zen mode (`Ctrl-Z`) hides chrome (status bar, borders) for a distraction-free single-pane view; toggled state is part of persisted session state.

### SSH & remote access
- Connection pooling and auto-reconnect with exponential backoff for SSH-backed panes.
- Per-connection tracking of port/agent/X11 forwarding, latency, and bytes sent/received.
- Detects groups of similar hosts (e.g. `web-01`/`02`/`03`) and suggests broadcast mode for batch operations.
- Attaches to a remote host's casmux session the same way as a local one (`casmux ssh user@host`), including remote session/workspace creation and remote command execution.
- Remote-to-local port/browser routing so agent-driven browser automation on a remote host can reach services listening on the operator's local machine.
- Files/images can be pushed into a remote session (e.g. via scp) for an agent pane to consume.
- **SSH panes must never fail with an "unknown terminal type" error, on any remote host.** casmux gets there with two layered mitigations, not one:
  1. **Auto-propagate first**: on connecting an SSH pane, casmux checks whether the remote host's terminfo database already has the `casmux`/`casmux-256color` entry and, if not, silently pushes it (equivalent to `infocmp | ssh host tic -`) before the pane's shell starts — mirroring the same trick tmux and mosh use for their own custom entries.
  2. **Safe fallback if propagation isn't possible**: when the remote host can't be written to (read-only/restricted account, propagation fails, or the connection doesn't allow it), the SSH pane falls back to a widely-pre-installed `TERM` value (`screen-256color`, then `xterm-256color`) instead of casmux's own entry, trading some capability fidelity for guaranteed compatibility on a host casmux doesn't control. This fallback is silent and automatic — never a user-facing error or a manual step.
  - Either way, the user never sees a broken/garbled remote session or a terminal-type error; whichever mitigation applied is visible only as connection metadata (see the per-connection tracking above), not as a failure.

### Directory navigation
- Frecency-based smart-cd (frequency × recency scoring), bookmarks, and navigation history.

### Window & pane management
- Panes: horizontal/vertical splits, floating panes, stacked panes.
- Windows (tabs) grouped under a session; a session is the project-level container (unifies tmux/casmux "session" with cmux's/herdr's "workspace" terminology).
- See "Built-in feature registry" and "Layout presets" above for swap/break/join/groups/tags/lock/zoom/auto-rename behavior.

### Platform support & distribution
- Clipboard integration per platform: Linux (`wl-copy`/`xclip`/OSC 52), macOS (`pbcopy`, `open` for URLs), Windows (Win32 clipboard API), generic OSC 52 fallback elsewhere.
- Distributed as: a universal `install.sh` (curl/wget, OS+arch detection, sudo-aware), a Homebrew formula, and a Debian package, in addition to the direct binary downloads AI.md already requires for the full linux/darwin/windows/freebsd/netbsd/openbsd × amd64/arm64 target matrix.

### Agent orchestration (adds cmux + herdr capabilities on top of the above)
- Panes may be bound to a running coding-agent process (Claude Code, Codex, OpenCode, Gemini, or any CLI agent) instead of a plain shell.
- Real-time agent status surfaced per pane and per tab: working / waiting-for-input / blocked / done / error.
- Agent-attention/agent-done events are one of the event sources feeding the general "Notifications & alerts" pipeline (status flag, visual/audible bell, desktop notification, in-app history) — not a separate notification mechanism of their own.
- Session/window sidebar shows contextual metadata: git branch, linked PR number/status, current working directory, listening ports.
- Socket API (local Unix socket, tunneled over SSH for remote hosts) lets an agent or external tool spawn panes, send keystrokes, read pane output/state, wait on another pane's completion, and drive session/window/pane lifecycle. This is a control API for external processes, not a user-loadable plugin/extension mechanism — it does not conflict with the "no plugin model" rule above.
- CLI mirrors the socket API 1:1 (create/split/attach, send input, `casmux notify`) for scripts and CI.
- Session/agent resume: reattaching resumes each agent's own session/conversation state where the underlying agent CLI supports it, not just terminal scrollback.
- Custom per-project commands defined in project config (build, test, deploy, spawn-agent-with-role), invocable from keybindings, CLI, or the socket API — configuration, not an installable plugin.
- Browser automation for agents: casmux drives an external or headless browser process (e.g. via CDP) through the socket API — navigate, snapshot the accessibility tree, click, fill forms, evaluate JS, read console/network activity — without casmux embedding a graphical browser pane itself.
- Nested/sub-agent support: an agent running in a pane can spawn further agent panes via the socket API, enabling supervisor/worker agent topologies.

### Non-goals
- **No cloud sync** — session/workspace state stays local to the host; casmux never syncs state to a casmux-operated remote service.
- **Not a terminal emulator replacement** — in TUI mode casmux always runs as a guest inside a host terminal emulator; it does not implement its own terminal emulation or font rendering for that surface. GUI mode is the only surface where casmux owns rendering.
- **No native mobile app** — see "Mobile-first responsive UI" above.
- Telemetry and feature-gating policy already follow the global defaults (telemetry opt-in only, no feature gating) and are not restated here.

### Trust boundaries and constraints
- The per-session and daemon control sockets are local Unix sockets restricted to the invoking user's file permissions by default; remote control is only ever exposed by tunneling a socket over an authenticated SSH connection, or via the web UI's own token-gated daemon connection (see "Roles & access control") — casmux never opens an unauthenticated network-listening control port by itself.
- Agent processes run with the same OS privileges as the user who started casmux; casmux implements no OS-level privilege separation or sandboxing of agent processes beyond the per-session-scoped socket API being the sole control surface (no raw shell handed to an agent that only requested pane control).
- There is no plugin/extension trust boundary to define, because there is no plugin/extension loading mechanism — every feature is vetted, built, and shipped as part of the single binary.
- Session persistence (scrollback, working directories, and captured environment variables) is written to local per-user storage in plaintext by default — the same trust boundary as shell history (`.bash_history`) or tmux-resurrect — so full session resurrection works out of the box. This is an accepted, explicitly documented local-single-user risk, not an oversight: nothing captured here is transmitted anywhere by default (see "No cloud sync").
- All session/workspace state needed to resume work (layouts, scrollback, pane-to-agent bindings) is stored locally per user; no state is sent to a remote service by default.
