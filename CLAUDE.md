# COMPLETE CASMUX PROJECT SPECIFICATION (COMPREHENSIVE EDITION)

## TABLE OF CONTENTS

1. [PROJECT IDENTITY AND FOUNDATION](#project-identity-and-foundation)
2. [ARCHITECTURE AND TECHNOLOGY STACK](#architecture-and-technology-stack)
3. [REPOSITORY STRUCTURE](#repository-structure)
4. [BUILD AND COMPILATION](#build-and-compilation)
5. [COMMAND-LINE INTERFACE](#command-line-interface)
6. [CONFIGURATION SYSTEM](#configuration-system)
7. [INTERFACE MODES AND DETECTION](#interface-modes-and-detection)
8. [LAUNCH AND SESSION MANAGEMENT](#launch-and-session-management)
9. [KEY BINDINGS SYSTEM](#key-bindings-system)
10. [STATUS BAR SYSTEM](#status-bar-system)
11. [COPY AND PASTE SYSTEM](#copy-and-paste-system)
12. [BROADCAST MODE](#broadcast-mode)
13. [SEARCH SYSTEM](#search-system)
14. [PROJECT DETECTION AND TEMPLATES](#project-detection-and-templates)
15. [BUILT-IN FEATURES (200+)](#built-in-features)
16. [FONT SYSTEM](#font-system)
17. [THEME SYSTEM](#theme-system)
18. [DISPLAY AND RESPONSIVE DESIGN](#display-and-responsive-design)
19. [MOUSE SUPPORT](#mouse-support)
20. [COMMAND PALETTE](#command-palette)
21. [HELP SYSTEM](#help-system)
22. [ERROR HANDLING AND RECOVERY](#error-handling-and-recovery)
23. [UPDATE SYSTEM](#update-system)
24. [PERFORMANCE SPECIFICATIONS](#performance-specifications)
25. [TMUX MIGRATION](#tmux-migration)
26. [NESTED SESSIONS](#nested-sessions)
27. [ZEN MODE](#zen-mode)
28. [SSH AND REMOTE MANAGEMENT](#ssh-and-remote-management)
29. [DIRECTORY NAVIGATION](#directory-navigation)
30. [WINDOW AND PANE MANAGEMENT](#window-and-pane-management)
31. [VISUAL AND UI SYSTEM](#visual-and-ui-system)
32. [DEBUGGING AND DIAGNOSTICS](#debugging-and-diagnostics)
33. [PLATFORM SUPPORT](#platform-support)
34. [DISTRIBUTION AND INSTALLATION](#distribution-and-installation)
35. [TESTING STRATEGY](#testing-strategy)
36. [DOCUMENTATION](#documentation)
37. [PROJECT GOVERNANCE](#project-governance)
38. [IMPLEMENTATION ROADMAP](#implementation-roadmap)

---

## PROJECT IDENTITY AND FOUNDATION

### Basic Information
- **Project Name:** CasjaysDev Mux
- **Binary Name:** casmux
- **Version Scheme:** Semantic Versioning (1.0.0)
- **Programming Language:** Rust (Edition 2021, minimum version 1.70)
- **License:** MIT License with copyright "CasjaysDev and contributors"
- **Repository:** https://github.com/casapps/casmux
- **Project Type:** Modern terminal multiplexer - tmux evolved for 2025+

### Mission Statement
Build a modern terminal multiplexer that takes the best concepts from tmux and evolves them for today's developers. CASMUX modernizes the terminal multiplexer experience through:

- **Zero-configuration operation** - Works perfectly out of the box with intelligent defaults
- **Built-in features** - All functionality from the top 200 tmux plugins built natively
- **Modern UX** - Intuitive naming, visual feedback, and discoverable features
- **Mobile-first responsive design** - Adapts from 30-column phone screens to 8K displays
- **Project intelligence** - Auto-detects project types and configures accordingly
- **Single binary** - Self-contained executable with no dependencies
- **tmux migration path** - One-way import from tmux configurations

### Core Philosophy
**"It Just Works"** - Users should never need to:
- Read documentation to get started
- Configure basic functionality
- Install plugins or extensions
- Remember cryptic commands
- Understand technical jargon

Everything should work the way users expect it to work, with powerful features available when needed but never in the way.

---

## ARCHITECTURE AND TECHNOLOGY STACK

### Core Architecture
```
┌────────────────────────────────────────────┐
│            CASMUX Application              │
├────────────────────────────────────────────┤
│         Multiplexer Core (Our Code)        │
│  • Session Management                      │
│  • Window/Pane Management                  │
│  • Built-in Features (200+)                │
│  • Project Detection                       │
│  • Configuration System                    │
├────────────────────────────────────────────┤
│     Terminal Emulation (wezterm-term)      │
│  • VT100/xterm/modern sequences           │
│  • Unicode/Emoji support                   │
│  • Scrollback management                   │
│  • Modern terminal features                │
├────────────────────────────────────────────┤
│          Rendering Layer                   │
│  • GUI: winit + skia-safe                 │
│  • TUI: ratatui + crossterm               │
│  • Font: Embedded Nerd Fonts              │
│  • Themes: Built-in (10 variants)         │
└────────────────────────────────────────────┘
```

### Technology Stack
```yaml
terminal_emulation:
  library: wezterm-term
  version: "0.1"
  license: MIT
  modifications: none  # Use as-is
  
gui_mode:
  window: winit        # Cross-platform window creation
  rendering: skia-safe # GPU-accelerated rendering (optional)
  
tui_mode:
  framework: ratatui   # Terminal UI framework
  backend: crossterm   # Cross-platform terminal handling
  
embedded_resources:
  fonts:
    - Cascadia Code Nerd Font (default)
    - JetBrains Mono Nerd Font
    - Hack Nerd Font
    - FiraCode Nerd Font
    - Iosevka Nerd Font
  
  themes:
    - Dracula (default dark)
    - Gruvbox
    - Nord
    - Solarized
    - Monokai
    # Each with light variant
  
  templates:
    - Rust, Node.js, Python, Go
    - Docker, Kubernetes
    - Server management
    - Generic development
```

### Binary Characteristics
```yaml
binary:
  type: single_executable
  dependencies: none  # Everything embedded
  size_target: 15-25MB uncompressed
  size_compressed: 8-15MB with UPX
  
  contains:
    - Complete terminal emulator
    - Multiplexer engine
    - All fonts (5 Nerd Fonts)
    - All themes (10 + variants)
    - All features (200+)
    - Project templates
    - Help system
```

---

## REPOSITORY STRUCTURE

### Complete Directory Layout
```
casmux/
├── LICENSE                           # MIT license text
├── LICENSE.md                        # Third-party licenses
│   # Includes: wezterm-term, fonts, etc.
├── README.md                         # Project overview
├── CONTRIBUTING.md                   # Contribution guidelines
├── CHANGELOG.md                      # Version history
├── CODE_OF_CONDUCT.md               # Community standards
├── SECURITY.md                       # Security policy
│
├── Cargo.toml                        # Workspace configuration
├── Cargo.lock                        # Dependency lock file
├── rust-toolchain.toml              # Rust version specification
├── .rustfmt.toml                    # Code formatting rules
├── .clippy.toml                     # Linting configuration
├── .gitignore                        # Git ignore rules
├── .gitattributes                   # Git attributes
├── .editorconfig                    # Editor configuration
│
├── Makefile                          # Build automation
│   # Targets: build, test, install, package, clean, release
├── justfile                          # Alternative to Make (optional)
│
├── .github/
│   ├── workflows/
│   │   ├── ci.yml                   # Continuous integration
│   │   ├── release.yml              # Release automation
│   │   ├── security.yml             # Security scanning
│   │   ├── coverage.yml             # Code coverage
│   │   └── nightly.yml              # Nightly builds
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.yml           # Bug report form
│   │   ├── feature_request.yml      # Feature request form
│   │   ├── question.yml             # Question form
│   │   └── config.yml               # Template configuration
│   ├── PULL_REQUEST_TEMPLATE.md
│   ├── FUNDING.yml                  # Sponsorship info
│   ├── dependabot.yml               # Dependency updates
│   └── CODEOWNERS                   # Code ownership
│
├── docs/
│   ├── README.md                     # Documentation index
│   ├── user-guide/
│   │   ├── installation.md          # Installation instructions
│   │   ├── quick-start.md           # Getting started
│   │   ├── configuration.md         # Configuration guide
│   │   ├── key-bindings.md          # Keyboard shortcuts
│   │   ├── mouse-usage.md           # Mouse features
│   │   ├── copy-paste.md            # Copy/paste guide
│   │   ├── search.md                # Search features
│   │   ├── broadcast-mode.md        # Broadcasting guide
│   │   ├── project-templates.md     # Template system
│   │   ├── ssh-management.md        # SSH features
│   │   ├── themes.md                # Theme customization
│   │   ├── fonts.md                 # Font configuration
│   │   ├── troubleshooting.md       # Common issues
│   │   └── faq.md                   # Frequently asked questions
│   ├── migration/
│   │   ├── from-tmux.md            # tmux migration guide
│   │   ├── from-screen.md          # GNU Screen migration
│   │   └── plugin-mapping.md       # Plugin equivalents
│   ├── developer-guide/
│   │   ├── architecture.md         # System architecture
│   │   ├── building.md             # Build instructions
│   │   ├── contributing.md         # How to contribute
│   │   ├── testing.md              # Testing guide
│   │   ├── debugging.md            # Debugging tips
│   │   ├── performance.md          # Performance guide
│   │   └── release-process.md      # Release procedures
│   ├── features/
│   │   ├── complete-list.md        # All 200+ features
│   │   ├── core-features.md        # Essential features
│   │   ├── productivity.md         # Productivity features
│   │   ├── development.md          # Developer features
│   │   └── advanced.md             # Advanced features
│   └── api/
│       ├── configuration.md        # Config API
│       ├── scripting.md           # Scripting interface
│       └── integration.md         # Integration points
│
├── scripts/
│   ├── install.sh                   # Universal installer
│   ├── uninstall.sh                # Clean uninstall
│   ├── build.sh                     # Build helper
│   ├── test.sh                      # Test runner
│   ├── package.sh                   # Package creator
│   ├── release.sh                   # Release automation
│   ├── cross-compile.sh            # Cross-compilation
│   └── import-tmux.sh              # tmux import helper
│
├── assets/
│   ├── fonts/
│   │   ├── CascadiaCode/
│   │   │   ├── CascadiaCode.ttf
│   │   │   ├── CascadiaCodeItalic.ttf
│   │   │   └── LICENSE.txt
│   │   ├── JetBrainsMono/
│   │   │   ├── JetBrainsMono.ttf
│   │   │   └── LICENSE.txt
│   │   ├── Hack/
│   │   │   ├── Hack.ttf
│   │   │   └── LICENSE.txt
│   │   ├── FiraCode/
│   │   │   ├── FiraCode.ttf
│   │   │   └── LICENSE.txt
│   │   └── Iosevka/
│   │       ├── Iosevka.ttf
│   │       └── LICENSE.txt
│   ├── themes/
│   │   ├── dracula.yml
│   │   ├── dracula-light.yml
│   │   ├── gruvbox.yml
│   │   ├── gruvbox-light.yml
│   │   ├── nord.yml
│   │   ├── nord-light.yml
│   │   ├── solarized-dark.yml
│   │   ├── solarized-light.yml
│   │   ├── monokai.yml
│   │   └── monokai-light.yml
│   ├── templates/
│   │   ├── projects/
│   │   │   ├── rust.yml
│   │   │   ├── node.yml
│   │   │   ├── python.yml
│   │   │   ├── go.yml
│   │   │   ├── java.yml
│   │   │   ├── ruby.yml
│   │   │   ├── php.yml
│   │   │   ├── dotnet.yml
│   │   │   └── generic.yml
│   │   ├── infrastructure/
│   │   │   ├── docker.yml
│   │   │   ├── kubernetes.yml
│   │   │   ├── terraform.yml
│   │   │   └── ansible.yml
│   │   └── management/
│   │       ├── servers.yml
│   │       ├── databases.yml
│   │       └── monitoring.yml
│   ├── icons/
│   │   ├── casmux.svg              # Vector logo
│   │   ├── casmux.ico              # Windows icon
│   │   ├── casmux.icns             # macOS icon
│   │   └── casmux.png              # Linux icon
│   └── screenshots/
│       ├── hero.png                 # Main screenshot
│       ├── features/                # Feature screenshots
│       └── themes/                  # Theme previews
│
├── tests/
│   ├── unit/                        # Unit tests
│   │   ├── config/
│   │   ├── core/
│   │   ├── features/
│   │   └── ui/
│   ├── integration/                 # Integration tests
│   │   ├── session_test.rs
│   │   ├── window_test.rs
│   │   ├── pane_test.rs
│   │   ├── copy_paste_test.rs
│   │   ├── search_test.rs
│   │   └── broadcast_test.rs
│   ├── e2e/                        # End-to-end tests
│   │   ├── user_workflows.rs
│   │   ├── tmux_migration.rs
│   │   └── performance.rs
│   ├── fixtures/                    # Test data
│   │   ├── configs/
│   │   ├── sessions/
│   │   └── tmux-configs/
│   └── benchmarks/                 # Performance tests
│       ├── startup.rs
│       ├── memory.rs
│       └── rendering.rs
│
├── crates/                          # Rust workspace
│   ├── casmux/                     # Main binary crate
│   │   ├── Cargo.toml
│   │   ├── build.rs                # Build script
│   │   └── src/
│   │       ├── main.rs             # Entry point
│   │       ├── cli.rs              # CLI parsing
│   │       ├── app.rs              # Application logic
│   │       └── lib.rs              # Library exports
│   │
│   ├── mux-core/                   # Core multiplexer
│   │   ├── Cargo.toml
│   │   └── src/
│   │       ├── lib.rs
│   │       ├── session.rs          # Session management
│   │       ├── window.rs           # Window management
│   │       ├── pane.rs             # Pane management
│   │       ├── layout.rs           # Layout algorithms
│   │       ├── state.rs            # State persistence
│   │       └── events.rs           # Event system
│   │
│   ├── mux-ui/                     # User interface
│   │   ├── Cargo.toml
│   │   └── src/
│   │       ├── lib.rs
│   │       ├── gui/                # GUI mode
│   │       │   ├── mod.rs
│   │       │   ├── window.rs
│   │       │   └── renderer.rs
│   │       ├── tui/                # TUI mode
│   │       │   ├── mod.rs
│   │       │   ├── app.rs
│   │       │   └── widgets.rs
│   │       ├── status_bar.rs       # Status bar
│   │       ├── command_palette.rs  # Command palette
│   │       └── themes.rs           # Theme engine
│   │
│   ├── mux-config/                 # Configuration
│   │   ├── Cargo.toml
│   │   └── src/
│   │       ├── lib.rs
│   │       ├── config.rs           # Config structures
│   │       ├── parser.rs           # YAML parser
│   │       ├── validator.rs        # Validation
│   │       ├── migration.rs        # tmux import
│   │       └── defaults.rs         # Default values
│   │
│   ├── mux-features/               # Built-in features
│   │   ├── Cargo.toml
│   │   └── src/
│   │       ├── lib.rs
│   │       ├── clipboard.rs        # Clipboard management
│   │       ├── search.rs           # Search functionality
│   │       ├── broadcast.rs        # Broadcast mode
│   │       ├── resurrect.rs        # Session save/restore
│   │       ├── continuum.rs        # Auto-save
│   │       ├── ssh.rs              # SSH management
│   │       ├── git.rs              # Git integration
│   │       ├── docker.rs           # Docker integration
│   │       ├── monitoring.rs       # System monitoring
│   │       └── ... (200+ features)
│   │
│   └── mux-utils/                  # Utilities
│       ├── Cargo.toml
│       └── src/
│           ├── lib.rs
│           ├── platform.rs         # Platform abstraction
│           ├── terminal.rs         # Terminal detection
│           ├── process.rs          # Process management
│           ├── network.rs          # Network utilities
│           └── fs.rs               # Filesystem helpers
│
├── packaging/                       # Distribution files
│   ├── debian/                     # Debian package
│   ├── rpm/                        # RPM package
│   ├── homebrew/                   # Homebrew formula
│   ├── snap/                        # Snap package
│   ├── flatpak/                    # Flatpak
│   ├── windows/                    # Windows installer
│   └── docker/                     # Docker image
│
└── examples/                       # Example configurations
    ├── configs/
    │   ├── minimal.yml
    │   ├── developer.yml
    │   └── power-user.yml
    └── templates/
        └── custom-project.yml
```

---

## BUILD AND COMPILATION

### Workspace Configuration (Cargo.toml)
```toml
[workspace]
members = [
    "crates/casmux",
    "crates/mux-core",
    "crates/mux-ui",
    "crates/mux-config",
    "crates/mux-features",
    "crates/mux-utils"
]
resolver = "2"

[workspace.package]
version = "1.0.0"
authors = ["CasjaysDev and contributors"]
edition = "2021"
rust-version = "1.70"
license = "MIT"
repository = "https://github.com/casapps/casmux"

[workspace.dependencies]
# Terminal Emulation
wezterm-term = "0.1"
termwiz = "0.20"
wezterm-color-types = "0.1"

# GUI Mode
winit = "0.29"
skia-safe = { version = "0.72", features = ["gpu"] }
raw-window-handle = "0.6"

# TUI Mode
ratatui = "0.25"
crossterm = "0.27"
tui-textarea = "0.4"

# Core Dependencies
tokio = { version = "1.35", features = ["full"] }
async-trait = "0.1"
futures = "0.3"

# Serialization
serde = { version = "1.0", features = ["derive"] }
serde_yaml = "0.9"
serde_json = "1.0"
bincode = "1.3"
toml = "0.8"  # For tmux import

# Utilities
anyhow = "1.0"
thiserror = "1.0"
tracing = "0.1"
tracing-subscriber = { version = "0.3", features = ["env-filter"] }
clap = { version = "4.4", features = ["derive", "env", "wrap_help"] }
clap_complete = "4.4"

# System
dirs = "5.0"
which = "6.0"
nix = { version = "0.27", features = ["signal", "process", "user"] }
libc = "0.2"
signal-hook = "0.3"

# Data Structures
indexmap = "2.1"
dashmap = "5.5"
parking_lot = "0.12"
arc-swap = "1.6"

# Text Processing
unicode-width = "0.1"
unicode-segmentation = "1.10"
textwrap = "0.16"
regex = "1.10"
fuzzy-matcher = "0.3"
skim = "0.10"

# Networking
reqwest = { version = "0.11", features = ["json", "rustls-tls"] }
url = "2.5"

# Development Tools
git2 = "0.18"
chrono = "0.4"
humantime = "2.1"
indicatif = "0.17"
colored = "2.1"

# Embedding Resources
include_bytes_aligned = "0.1"
rust-embed = "8.2"

# Platform Specific
[target.'cfg(windows)'.dependencies]
windows = { version = "0.52", features = ["Win32_System_Console"] }
winapi = { version = "0.3", features = ["wincon", "processenv", "winbase"] }

[target.'cfg(target_os = "macos")'.dependencies]
cocoa = "0.25"
objc = "0.2"
core-foundation = "0.9"

[profile.dev]
opt-level = 0
debug = true
split-debuginfo = "unpacked"

[profile.release]
opt-level = "z"      # Optimize for size
lto = "fat"         # Link-time optimization
codegen-units = 1   # Single codegen unit
panic = "abort"     # Smaller binary
strip = "symbols"   # Strip symbols
debug = false

[profile.release-with-debug]
inherits = "release"
strip = "none"
debug = true
```

### Build Script (build.rs)
```rust
// crates/casmux/build.rs
use std::env;
use std::fs;
use std::path::PathBuf;

fn main() {
    // Embed version information
    println!("cargo:rustc-env=CASMUX_VERSION={}", env!("CARGO_PKG_VERSION"));
    println!("cargo:rustc-env=BUILD_DATE={}", chrono::Utc::now().format("%Y-%m-%d"));
    
    // Embed fonts
    embed_fonts();
    
    // Embed themes
    embed_themes();
    
    // Generate shell completions
    generate_completions();
    
    // Platform-specific configuration
    configure_platform();
}

fn embed_fonts() {
    let font_dir = PathBuf::from("assets/fonts");
    for font_family in fs::read_dir(font_dir).unwrap() {
        let path = font_family.unwrap().path();
        println!("cargo:rerun-if-changed={}", path.display());
    }
}

fn embed_themes() {
    let theme_dir = PathBuf::from("assets/themes");
    for theme in fs::read_dir(theme_dir).unwrap() {
        let path = theme.unwrap().path();
        println!("cargo:rerun-if-changed={}", path.display());
    }
}

fn generate_completions() {
    // Generate shell completions for bash, zsh, fish, powershell
}

fn configure_platform() {
    let target_os = env::var("CARGO_CFG_TARGET_OS").unwrap();
    match target_os.as_str() {
        "windows" => {
            // Windows-specific configuration
            println!("cargo:rustc-link-lib=user32");
        }
        "macos" => {
            // macOS-specific configuration
            println!("cargo:rustc-link-lib=framework=AppKit");
        }
        _ => {}
    }
}
```

### Makefile
```makefile
# CASMUX Makefile

# Variables
BINARY_NAME = casmux
VERSION = $(shell grep '^version' Cargo.toml | head -1 | cut -d'"' -f2)
TARGET_DIR = target
RELEASE_DIR = $(TARGET_DIR)/release
INSTALL_PREFIX = /usr/local
INSTALL_BIN = $(INSTALL_PREFIX)/bin

# Platform detection
UNAME_S := $(shell uname -s)
UNAME_M := $(shell uname -m)

ifeq ($(UNAME_S),Linux)
    PLATFORM = linux
endif
ifeq ($(UNAME_S),Darwin)
    PLATFORM = macos
endif
ifeq ($(OS),Windows_NT)
    PLATFORM = windows
    BINARY_NAME := $(BINARY_NAME).exe
endif

ifeq ($(UNAME_M),x86_64)
    ARCH = amd64
endif
ifeq ($(UNAME_M),aarch64)
    ARCH = arm64
endif

# Default target
.PHONY: all
all: build

# Development build
.PHONY: build
build:
	cargo build
	@echo "Development build complete: $(TARGET_DIR)/debug/$(BINARY_NAME)"

# Release build
.PHONY: release
release:
	cargo build --release
	@echo "Release build complete: $(RELEASE_DIR)/$(BINARY_NAME)"

# Optimized release (with UPX compression)
.PHONY: release-optimized
release-optimized: release
	@which upx > /dev/null || (echo "UPX not found, skipping compression" && exit 0)
	upx --best --lzma $(RELEASE_DIR)/$(BINARY_NAME)
	@echo "Optimized binary: $(RELEASE_DIR)/$(BINARY_NAME)"

# Install
.PHONY: install
install: release
	@echo "Installing $(BINARY_NAME) to $(INSTALL_BIN)"
	@mkdir -p $(INSTALL_BIN)
	@cp $(RELEASE_DIR)/$(BINARY_NAME) $(INSTALL_BIN)/
	@chmod 755 $(INSTALL_BIN)/$(BINARY_NAME)
	@echo "Installation complete"

# Uninstall
.PHONY: uninstall
uninstall:
	@echo "Removing $(INSTALL_BIN)/$(BINARY_NAME)"
	@rm -f $(INSTALL_BIN)/$(BINARY_NAME)
	@echo "Uninstall complete"

# Run tests
.PHONY: test
test:
	cargo test --all

# Run with coverage
.PHONY: coverage
coverage:
	cargo tarpaulin --out Html --output-dir coverage

# Run benchmarks
.PHONY: bench
bench:
	cargo bench

# Format code
.PHONY: fmt
fmt:
	cargo fmt --all

# Lint code
.PHONY: lint
lint:
	cargo clippy --all-targets --all-features -- -D warnings

# Clean build artifacts
.PHONY: clean
clean:
	cargo clean
	@rm -rf coverage/
	@rm -rf dist/

# Build for all platforms
.PHONY: build-all
build-all:
	@echo "Building for all platforms..."
	cargo build --release --target x86_64-unknown-linux-gnu
	cargo build --release --target aarch64-unknown-linux-gnu
	cargo build --release --target x86_64-apple-darwin
	cargo build --release --target aarch64-apple-darwin
	cargo build --release --target x86_64-pc-windows-gnu

# Create distribution packages
.PHONY: package
package: release
	@mkdir -p dist
	tar czf dist/$(BINARY_NAME)-$(VERSION)-$(PLATFORM)-$(ARCH).tar.gz \
		-C $(RELEASE_DIR) $(BINARY_NAME) \
		-C ../.. README.md LICENSE
	@echo "Package created: dist/$(BINARY_NAME)-$(VERSION)-$(PLATFORM)-$(ARCH).tar.gz"

# Create all packages
.PHONY: package-all
package-all: build-all
	@echo "Creating packages for all platforms..."
	@./scripts/package.sh

# Run development version
.PHONY: run
run:
	cargo run

# Run with debug logging
.PHONY: run-debug
run-debug:
	RUST_LOG=debug cargo run -- --debug

# Check for updates
.PHONY: update-deps
update-deps:
	cargo update
	cargo outdated

# Security audit
.PHONY: audit
audit:
	cargo audit

# Generate documentation
.PHONY: docs
docs:
	cargo doc --no-deps --open

# Help
.PHONY: help
help:
	@echo "CASMUX Makefile"
	@echo ""
	@echo "Usage: make [target]"
	@echo ""
	@echo "Targets:"
	@echo "  build           - Build development version"
	@echo "  release         - Build release version"
	@echo "  install         - Install to system"
	@echo "  uninstall       - Remove from system"
	@echo "  test            - Run tests"
	@echo "  coverage        - Generate test coverage"
	@echo "  bench           - Run benchmarks"
	@echo "  fmt             - Format code"
	@echo "  lint            - Lint code"
	@echo "  clean           - Clean build artifacts"
	@echo "  package         - Create distribution package"
	@echo "  run             - Run development version"
	@echo "  run-debug       - Run with debug logging"
	@echo "  docs            - Generate documentation"
	@echo "  help            - Show this help"

# Default
.DEFAULT_GOAL := build
```

### Cross-Compilation Script
```bash
#!/usr/bin/env bash
# scripts/cross-compile.sh

set -e

TARGETS=(
    "x86_64-unknown-linux-gnu"
    "x86_64-unknown-linux-musl"
    "aarch64-unknown-linux-gnu"
    "aarch64-unknown-linux-musl"
    "x86_64-apple-darwin"
    "aarch64-apple-darwin"
    "x86_64-pc-windows-gnu"
    "aarch64-pc-windows-msvc"
    "x86_64-unknown-freebsd"
)

for target in "${TARGETS[@]}"; do
    echo "Building for $target..."
    
    if cargo build --release --target "$target" 2>/dev/null; then
        echo "✓ $target built successfully"
    else
        echo "✗ $target build failed (may require additional setup)"
    fi
done

echo "Cross-compilation complete!"
```

---

## COMMAND-LINE INTERFACE

### CLI Structure
```rust
use clap::{Parser, Subcommand};
use std::path::PathBuf;

#[derive(Parser)]
#[command(
    name = "casmux",
    about = "Modern terminal multiplexer - tmux evolved",
    version,
    author,
    long_about = "CASMUX is a modern terminal multiplexer that takes the best of tmux \
                  and evolves it for today's developers. Zero configuration required, \
                  with 200+ built-in features."
)]
struct Cli {
    /// Path to working directory (default: current directory)
    #[arg(value_name = "PATH")]
    path: Option<PathBuf>,
    
    /// Enable debug output
    #[arg(short = 'd', long, env = "CASMUX_DEBUG")]
    debug: bool,
    
    /// Configuration file to load
    #[arg(short = 'f', long = "file", value_name = "CONFIG")]
    config: Option<String>,
    
    /// Generate configuration file
    #[arg(short = 'c', long = "config", value_name = "NAME")]
    generate_config: Option<Option<String>>,
    
    /// Print version information
    #[arg(short = 'v', long)]
    version: bool,
    
    /// Subcommands
    #[command(subcommand)]
    command: Option<Commands>,
}

#[derive(Subcommand)]
enum Commands {
    /// Attach to an existing session
    Attach {
        /// Session name or ID
        session: Option<String>,
    },
    
    /// List all sessions
    #[command(visible_alias = "ls")]
    List {
        /// Format output (simple, detailed, json)
        #[arg(short, long, default_value = "simple")]
        format: String,
    },
    
    /// Create a new session
    New {
        /// Session name
        #[arg(short = 'n', long)]
        name: Option<String>,
        
        /// Working directory
        #[arg(short = 'd', long)]
        directory: Option<PathBuf>,
        
        /// Template to use
        #[arg(short = 't', long)]
        template: Option<String>,
        
        /// Initial command
        #[arg(short = 'c', long)]
        command: Option<String>,
    },
    
    /// Kill a session
    Kill {
        /// Session name or "all"
        session: String,
        
        /// Force kill without confirmation
        #[arg(short, long)]
        force: bool,
    },
    
    /// Import tmux configuration
    Import {
        /// Path to tmux config file
        #[arg(default_value = "~/.tmux.conf")]
        tmux_config: PathBuf,
        
        /// Output file for CASMUX config
        #[arg(short, long)]
        output: Option<PathBuf>,
    },
    
    /// Update CASMUX to latest version
    Update {
        /// Check for updates without installing
        #[arg(long)]
        check_only: bool,
        
        /// Update to specific version
        #[arg(long)]
        version: Option<String>,
    },
    
    /// Run system diagnostics
    Doctor {
        /// Verbose output
        #[arg(short, long)]
        verbose: bool,
    },
    
    /// Generate shell completions
    Completions {
        /// Shell to generate for
        #[arg(value_enum)]
        shell: Shell,
    },
}
```

### Usage Examples
```bash
# Basic usage - start in current directory
casmux

# Start in specific directory
casmux ~/projects/webapp

# Start with explicit current directory
casmux .

# Create new session with name
casmux new -n webapp -d ~/projects/webapp

# Create with template
casmux new -t rust -n myproject

# Attach to session
casmux attach webapp
casmux a webapp  # Short alias

# List sessions
casmux list
casmux ls

# Kill session
casmux kill webapp
casmux kill all --force

# Import tmux config
casmux import ~/.tmux.conf

# Check for updates
casmux update --check-only

# Update to latest
casmux update

# Run diagnostics
casmux doctor
casmux doctor --verbose

# Generate config
casmux --config
casmux --config server  # Named config

# Load specific config
casmux -f server
casmux -f ~/.config/casmux/custom.yml

# Debug mode
casmux --debug
casmux -d

# Version info
casmux --version
casmux -v

# Help
casmux --help
casmux -h
casmux help new  # Help for subcommand
```

---

## CONFIGURATION SYSTEM

### Configuration Philosophy
```yaml
principles:
  zero_config: "Works perfectly without any configuration"
  minimal_settings: "Only expose settings users actually change"
  intuitive_naming: "Self-explanatory, no technical jargon"
  smart_defaults: "Automatically configure based on context"
  progressive_disclosure: "Advanced settings hidden by default"
```

### Configuration File Locations
```yaml
search_order:
  1: "./.casmux.yml"              # Project-specific
  2: "./.casmux.yaml"             # Alternative extension
  3: "~/.config/casmux/config.yml" # User configuration
  4: "~/.config/casmux/config.yaml" # Alternative extension
  5: "/etc/casmux/config.yml"     # System-wide
  6: "Built-in defaults"          # Always works

# Custom configs
named_configs: "~/.config/casmux/{name}.yml"
```

### Complete Configuration Structure
```yaml
# ~/.config/casmux/config.yml
# CASMUX Configuration
# Everything is optional - sensible defaults are provided

# ============================================================================
# ESSENTIAL SETTINGS - What most users might change
# ============================================================================

# The key you press before commands (like tmux prefix)
# Default: C-b (Ctrl+B)
# Common alternatives: C-a, C-space
prefix_key: C-a

# Color scheme for the interface
# Options: auto, dracula, gruvbox, nord, solarized, monokai
# Each theme has dark/light variants, auto detects terminal background
theme: dracula

# Font size in points (supports fractional values)
# Default: 14.0
# Range: 8.0 to 72.0
font_size: 14.0

# ============================================================================
# BEHAVIOR SETTINGS - Personal preferences
# ============================================================================

# Enable mouse support for clicking, scrolling, selecting
# Default: true
mouse: false

# Copy mode key bindings style
# Options: vi, emacs
# Default: vi
copy_mode: vi

# Automatically save sessions every 5 minutes
# Default: true
auto_save: true

# Restore previous sessions when starting CASMUX
# Default: false
restore_on_start: false

# ============================================================================
# STATUS BAR - What information to display
# ============================================================================

status_bar:
  # Show weather in status bar (requires internet)
  # Default: false (opt-in feature)
  show_weather: false
  
  # Show git branch and status when in git repository
  # Default: auto (shows only in git repos)
  show_git: auto
  
  # Show system uptime
  # Default: true
  show_uptime: true
  
  # Date/time format (uses strftime format)
  # Default: "%m/%d %H:%M"
  datetime_format: "%m/%d %H:%M"
  
  # Position of mode indicator
  # Options: left, right, off
  # Default: left (far left corner)
  mode_indicator_position: left

# ============================================================================
# KEY DELAYS - For advanced users (especially vim users)
# ============================================================================

# Delay after pressing ESC key (in milliseconds)
# Lower values better for vim, higher for slow connections
# Default: 10
escape_time: 10

# Time window for repeating keys after prefix (in milliseconds)
# Default: 500
repeat_time: 500

# ============================================================================
# CUSTOM KEYBINDINGS - Add your own shortcuts
# ============================================================================

# Format: "key_combination": "command"
# Use 'prefix' for prefix key, C- for Ctrl, M- for Alt, S- for Shift
keybindings:
  # Examples:
  "C-t": new-window               # Ctrl+T creates new window
  "prefix |": split-horizontal    # Prefix then | for horizontal split
  "prefix -": split-vertical      # Prefix then - for vertical split
  "M-Left": previous-window       # Alt+Left for previous window
  "M-Right": next-window          # Alt+Right for next window

# ============================================================================
# PROJECT TEMPLATES - Custom project layouts
# ============================================================================

# Define custom templates for your projects
templates:
  # Example custom template
  myproject:
    description: "My custom project setup"
    windows:
      - name: editor
        command: nvim
        directory: .
      - name: server
        command: npm run dev
        directory: .
      - name: database
        command: docker-compose up db
        directory: ./docker
      - name: logs
        command: tail -f logs/*.log
        directory: .

# ============================================================================
# ADVANCED SETTINGS - Usually not needed
# ============================================================================

# These settings have good defaults and rarely need changing
advanced:
  # Maximum lines of scrollback history per pane
  # Default: 10000
  scrollback_limit: 10000
  
  # How often to refresh status bar (in seconds)
  # Default: 15
  status_refresh: 15
  
  # Starting index for windows
  # Default: 1 (like tmux)
  window_base_index: 1
  
  # Starting index for panes
  # Default: 1 (like tmux)
  pane_base_index: 1
  
  # Renumber windows when one is closed
  # Default: true
  renumber_windows: true
  
  # Set terminal window title
  # Default: true
  set_titles: true
  
  # Terminal type to report
  # Default: "screen-256color"
  default_terminal: "screen-256color"
  
  # Enable true color support
  # Default: auto-detect
  true_color: auto

# ============================================================================
# FEATURE FLAGS - Enable/disable specific features
# ============================================================================

features:
  # Most features are automatically enabled when relevant
  # These settings override automatic detection
  
  # ZEN mode for distraction-free work
  # Default: true
  zen_mode: true
  
  # Broadcast typing to multiple panes
  # Default: true
  broadcast_mode: true
  
  # Smart directory navigation with frecency
  # Default: true
  smart_cd: true
  
  # Project type detection
  # Default: true
  project_detection: true
  
  # Session resurrection after crash
  # Default: true
  resurrect: true
  
  # Continuous session saving
  # Default: true
  continuum: true

# ============================================================================
# PERFORMANCE - Optimization settings
# ============================================================================

performance:
  # GPU acceleration for rendering (when available)
  # Default: auto
  gpu_acceleration: auto
  
  # Frame rate limit for smooth scrolling
  # Default: 60
  max_fps: 60
  
  # Lazy render inactive panes
  # Default: true
  lazy_rendering: true

# ============================================================================
# PATHS - Custom paths (rarely needed)
# ============================================================================

paths:
  # Where to save session data
  # Default: ~/.local/share/casmux
  data_dir: ~/.local/share/casmux
  
  # Where to save logs
  # Default: ~/.local/share/casmux/logs
  log_dir: ~/.local/share/casmux/logs
  
  # Where to look for custom templates
  # Default: ~/.config/casmux/templates
  template_dir: ~/.config/casmux/templates
```

### Settings UI Implementation
```
┌─ Settings ─────────────────────────────────┐
│                                            │
│ Search: [____________________] 🔍         │
│                                            │
│ ● Essential Settings                       │
│ ○ Appearance                              │
│ ○ Behavior                                │
│ ○ Keybindings                             │
│ ○ Advanced                                │
│                                            │
├────────────────────────────────────────────┤
│ Essential Settings                         │
│                                            │
│ Prefix Key:     [Ctrl+A     ▼]            │
│ The key you press before commands          │
│                                            │
│ Theme:          [Dracula    ▼]            │
│ Color scheme (auto detects dark/light)     │
│                                            │
│ Font Size:      [−] 14.0 [+]              │
│ Text size in points                        │
│                                            │
│ Mouse Support:  [○ On  ● Off]             │
│ Enable mouse clicking and scrolling        │
│                                            │
│ Copy Mode:      [● Vi  ○ Emacs]           │
│ Keyboard style for text selection          │
│                                            │
│ Auto-save:      [● On  ○ Off]             │
│ Save sessions every 5 minutes              │
│                                            │
│ [Apply] [Reset] [Open Config File]        │
└────────────────────────────────────────────┘
```

### Smart Configuration Loading
```rust
impl ConfigManager {
    fn load_configuration() -> Config {
        // 1. Start with built-in defaults
        let mut config = Config::default();
        
        // 2. Check for system config
        if let Ok(system) = Config::from_file("/etc/casmux/config.yml") {
            config.merge(system);
        }
        
        // 3. Check for user config
        if let Ok(user) = Config::from_file("~/.config/casmux/config.yml") {
            config.merge(user);
        }
        
        // 4. Check for project config
        if let Ok(project) = Config::from_file("./.casmux.yml") {
            config.merge(project);
        }
        
        // 5. Apply environment variables
        config.apply_env_vars();
        
        // 6. Apply command-line flags
        config.apply_cli_args();
        
        // 7. Auto-detect and configure based on environment
        config.auto_configure();
        
        config
    }
    
    fn auto_configure(&mut self) {
        // Detect terminal capabilities
        if terminal_supports_truecolor() {
            self.advanced.true_color = true;
        }
        
        // Detect if in SSH session
        if in_ssh_session() {
            self.performance.lazy_rendering = true;
            self.features.weather = false; // Disable weather over SSH
        }
        
        // Detect battery presence
        if !has_battery() {
            self.status_bar.show_battery = false;
        }
        
        // Detect git repository
        if !in_git_repo() {
            self.status_bar.show_git = false;
        }
        
        // Detect small terminal
        if terminal_width() < 80 {
            self.status_bar.compact_mode = true;
        }
    }
}
```

---

## INTERFACE MODES AND DETECTION

### Detection Logic
```rust
enum InterfaceMode {
    GUI,    // Native window with embedded terminal
    TUI,    // Running in existing terminal
}

impl InterfaceMode {
    fn detect() -> Self {
        // Priority order of detection
        
        // 1. Check if in SSH/Mosh session
        if env::var("SSH_CONNECTION").is_ok() || 
           env::var("SSH_TTY").is_ok() ||
           env::var("MOSH_CONNECTION").is_ok() {
            return InterfaceMode::TUI;
        }
        
        // 2. Check if no display available
        if env::var("DISPLAY").is_err() && 
           env::var("WAYLAND_DISPLAY").is_err() &&
           !cfg!(windows) {
            return InterfaceMode::TUI;
        }
        
        // 3. Check if running in terminal
        if isatty::stdout_isatty() {
            // Check if parent is a terminal emulator
            if let Some(parent) = get_parent_process() {
                if is_terminal_emulator(&parent) {
                    return InterfaceMode::TUI;
                }
            }
        }
        
        // 4. Check if launched from desktop
        if launched_from_desktop() {
            return InterfaceMode::GUI;
        }
        
        // Default to TUI for safety
        InterfaceMode::TUI
    }
}

fn launched_from_desktop() -> bool {
    // Platform-specific detection
    
    #[cfg(target_os = "linux")]
    {
        // Check if launched via .desktop file
        env::var("GIO_LAUNCHED_DESKTOP_FILE").is_ok()
    }
    
    #[cfg(target_os = "macos")]
    {
        // Check if launched from Finder
        get_parent_process()
            .map(|p| p.name == "Finder" || p.name == "Dock")
            .unwrap_or(false)
    }
    
    #[cfg(windows)]
    {
        // Check if launched from Explorer
        get_parent_process()
            .map(|p| p.name == "explorer.exe")
            .unwrap_or(false)
    }
}

fn is_terminal_emulator(process: &Process) -> bool {
    const TERMINAL_EMULATORS: &[&str] = &[
        // Linux
        "gnome-terminal", "konsole", "xterm", "rxvt", "terminator",
        "alacritty", "kitty", "wezterm", "foot", "termite",
        
        // macOS
        "Terminal", "iTerm2", "Hyper", "Alacritty",
        
        // Windows
        "WindowsTerminal", "ConHost", "mintty", "cmd", "powershell",
        
        // Cross-platform
        "tmux", "screen", "zellij",
    ];
    
    TERMINAL_EMULATORS.iter().any(|&name| {
        process.name.to_lowercase().contains(name.to_lowercase().as_str())
    })
}
```

### GUI Mode Implementation
```rust
// GUI Mode - Native window with embedded terminal
impl GuiMode {
    fn new() -> Result<Self> {
        // Create native window
        let event_loop = EventLoop::new()?;
        let window = WindowBuilder::new()
            .with_title("CASMUX")
            .with_inner_size(LogicalSize::new(1200, 800))
            .with_min_inner_size(LogicalSize::new(400, 300))
            .build(&event_loop)?;
        
        // Initialize terminal emulator
        let terminal = WezTerminal::new()?;
        
        // Initialize renderer
        let renderer = SkiaRenderer::new(&window)?;
        
        // Set up for maximized launch if from desktop
        if launched_from_desktop() {
            window.set_maximized(true);
        }
        
        Ok(Self {
            window,
            terminal,
            renderer,
            event_loop,
        })
    }
    
    fn run(mut self) -> Result<()> {
        self.event_loop.run(move |event, _, control_flow| {
            match event {
                Event::WindowEvent { event, .. } => {
                    self.handle_window_event(event);
                }
                Event::MainEventsCleared => {
                    self.update();
                    self.render();
                }
                _ => {}
            }
        })
    }
}
```

### TUI Mode Implementation
```rust
// TUI Mode - Runs in existing terminal
impl TuiMode {
    fn new() -> Result<Self> {
        // Initialize terminal
        let backend = CrosstermBackend::new(stdout());
        let terminal = Terminal::new(backend)?;
        
        // Enter alternate screen
        execute!(stdout(), EnterAlternateScreen)?;
        enable_raw_mode()?;
        
        // Set up panic handler to restore terminal
        let original_hook = panic::take_hook();
        panic::set_hook(Box::new(move |panic| {
            Self::restore_terminal();
            original_hook(panic);
        }));
        
        Ok(Self { terminal })
    }
    
    fn run(mut self) -> Result<()> {
        loop {
            // Draw UI
            self.terminal.draw(|f| {
                self.render_ui(f);
            })?;
            
            // Handle events
            if let Event::Key(key) = event::read()? {
                if !self.handle_key(key)? {
                    break;
                }
            }
        }
        
        Ok(())
    }
    
    fn restore_terminal() {
        let _ = disable_raw_mode();
        let _ = execute!(stdout(), LeaveAlternateScreen);
    }
}
```

---

## LAUNCH AND SESSION MANAGEMENT

### First Launch Experience
```rust
impl FirstLaunch {
    fn check_and_run() -> Result<bool> {
        let config_dir = dirs::config_dir()
            .unwrap()
            .join("casmux");
        
        let first_run_marker = config_dir.join(".first_run_complete");
        
        if !first_run_marker.exists() {
            self.show_welcome()?;
            fs::write(first_run_marker, "")?;
            return Ok(true);
        }
        
        Ok(false)
    }
    
    fn show_welcome() -> Result<()> {
        println!("
╔═══════════════════════════════════════════════════════════╗
║           Welcome to CASMUX - Modern Terminal Multiplexer ║
╠═══════════════════════════════════════════════════════════╣
║                                                           ║
║  CASMUX is tmux evolved for modern developers.           ║
║  Everything works out of the box - no configuration      ║
║  needed!                                                 ║
║                                                           ║
║  What would you like to do?                             ║
║                                                           ║
║  [T] Take a 30-second tour                              ║
║  [I] Import existing tmux configuration                 ║
║  [S] Start using CASMUX                                 ║
║  [H] Show help                                          ║
║                                                           ║
╚═══════════════════════════════════════════════════════════╝
        ");
        
        let choice = self.read_choice()?;
        match choice {
            'T' | 't' => self.show_tour()?,
            'I' | 'i' => self.import_tmux()?,
            'H' | 'h' => self.show_help()?,
            _ => {} // Start normally
        }
        
        Ok(())
    }
}
```

### Session Discovery
```rust
impl SessionManager {
    fn start_or_attach() -> Result<()> {
        // Get all existing sessions
        let sessions = self.list_sessions()?;
        
        if sessions.is_empty() {
            // No sessions exist - create new one
            self.create_default_session()?;
        } else if sessions.len() == 1 {
            // Single session - attach to it
            self.attach_session(&sessions[0])?;
        } else {
            // Multiple sessions - show picker
            self.show_session_picker(sessions)?;
        }
        
        Ok(())
    }
    
    fn show_session_picker(&self, sessions: Vec<Session>) -> Result<()> {
        // Interactive session picker
        let picker = SessionPicker::new(sessions);
        picker.render()?;
        
        /*
        ┌─ Active Sessions ──────────────────────────┐
        │ Select a session or create new:            │
        │                                            │
        │ → webapp    3 windows  (2 hours ago)      │
        │   server    2 windows  (attached)         │
        │   docker    4 windows  (yesterday)        │
        │   project   1 window   (last week)        │
        │                                            │
        │ [↵ Select] [n New] [d Delete] [r Rename]  │
        │ [s Search] [q Quit]                       │
        └────────────────────────────────────────────┘
        */
        
        Ok(())
    }
    
    fn create_default_session(&self) -> Result<()> {
        let cwd = env::current_dir()?;
        let project_type = ProjectDetector::detect(&cwd);
        
        let session = match project_type {
            ProjectType::Rust => {
                Session::from_template("rust", &cwd)?
            }
            ProjectType::Node => {
                Session::from_template("node", &cwd)?
            }
            _ => {
                Session::new_default(&cwd)?
            }
        };
        
        println!("Creating session: {}", session.name);
        self.create_session(session)?;
        
        Ok(())
    }
}
```

### Session State Management
```rust
#[derive(Serialize, Deserialize)]
struct SessionState {
    id: Uuid,
    name: String,
    created_at: DateTime<Utc>,
    updated_at: DateTime<Utc>,
    windows: Vec<WindowState>,
    current_window: usize,
    cwd: PathBuf,
    environment: HashMap<String, String>,
    
    // Persistent state
    command_history: Vec<String>,
    search_history: Vec<String>,
    clipboard_history: Vec<String>,
    
    // Layout
    layout: LayoutState,
    
    // Features
    broadcast_enabled: bool,
    zen_mode: bool,
    
    // Metadata
    project_type: Option<ProjectType>,
    git_branch: Option<String>,
    
    // Stats
    total_commands: usize,
    total_runtime: Duration,
}

impl SessionState {
    fn save(&self) -> Result<()> {
        let path = self.state_file_path();
        let data = bincode::serialize(self)?;
        
        // Atomic write
        let temp_path = path.with_extension("tmp");
        fs::write(&temp_path, data)?;
        fs::rename(temp_path, path)?;
        
        Ok(())
    }
    
    fn restore(path: &Path) -> Result<Self> {
        let data = fs::read(path)?;
        let state = bincode::deserialize(&data)?;
        Ok(state)
    }
    
    fn auto_save_loop(&self) {
        let state = Arc::clone(&self.state);
        
        tokio::spawn(async move {
            let mut interval = time::interval(Duration::from_secs(300)); // 5 minutes
            
            loop {
                interval.tick().await;
                
                if let Err(e) = state.save() {
                    tracing::error!("Failed to auto-save session: {}", e);
                }
            }
        });
    }
}
```

---

## KEY BINDINGS SYSTEM

### Default Key Bindings
```rust
lazy_static! {
    static ref DEFAULT_KEYBINDINGS: HashMap<KeyBinding, Action> = {
        let mut bindings = HashMap::new();
        
        // ========================================
        // Global Keys (no prefix needed)
        // ========================================
        
        // Help
        bindings.insert(key!(F1), Action::ShowHelp);
        
        // Command Palette
        bindings.insert(key!(Ctrl-P), Action::OpenCommandPalette);
        
        // Search
        bindings.insert(key!(Ctrl-F), Action::GlobalSearch);
        
        // ZEN Mode
        bindings.insert(key!(Ctrl-Z), Action::ToggleZenMode);
        
        // Font Zoom
        bindings.insert(key!(Ctrl-Minus), Action::FontDecrease);
        bindings.insert(key!(Ctrl-Equal), Action::FontIncrease);
        bindings.insert(key!(Ctrl-0), Action::FontReset);
        
        // Window Navigation (without prefix)
        bindings.insert(key!(Shift-Left), Action::PreviousWindow);
        bindings.insert(key!(Shift-Right), Action::NextWindow);
        
        // Vim-aware pane navigation (without prefix)
        bindings.insert(key!(Ctrl-H), Action::NavigatePaneOrVim(Direction::Left));
        bindings.insert(key!(Ctrl-J), Action::NavigatePaneOrVim(Direction::Down));
        bindings.insert(key!(Ctrl-K), Action::NavigatePaneOrVim(Direction::Up));
        bindings.insert(key!(Ctrl-L), Action::NavigatePaneOrVim(Direction::Right));
        
        // ========================================
        // Prefix Keys (default: Ctrl-B)
        // ========================================
        
        // Session Management
        bindings.insert(prefix_key!(d), Action::DetachSession);
        bindings.insert(prefix_key!(s), Action::ShowSessionPicker);
        bindings.insert(prefix_key!($), Action::RenameSession);
        bindings.insert(prefix_key!(L), Action::SwitchLastSession);
        
        // Window Management
        bindings.insert(prefix_key!(c), Action::NewWindow);
        bindings.insert(prefix_key!(n), Action::NextWindow);
        bindings.insert(prefix_key!(p), Action::PreviousWindow);
        bindings.insert(prefix_key!(l), Action::LastWindow);
        bindings.insert(prefix_key!(w), Action::ShowWindowPicker);
        bindings.insert(prefix_key!(&), Action::KillWindow);
        bindings.insert(prefix_key!(,), Action::RenameWindow);
        
        // Window Selection by Number
        for i in 1..=9 {
            bindings.insert(
                prefix_key!(i.to_string()),
                Action::SelectWindow(i)
            );
        }
        bindings.insert(prefix_key!(0), Action::SelectWindow(10));
        
        // Pane Management
        bindings.insert(prefix_key!(\\), Action::SplitHorizontal);
        bindings.insert(prefix_key!(/), Action::SplitVertical);
        bindings.insert(prefix_key!(%), Action::SplitHorizontal); // tmux compat
        bindings.insert(prefix_key!("\""), Action::SplitVertical); // tmux compat
        
        // Pane Navigation
        bindings.insert(prefix_key!(Left), Action::NavigatePane(Direction::Left));
        bindings.insert(prefix_key!(Down), Action::NavigatePane(Direction::Down));
        bindings.insert(prefix_key!(Up), Action::NavigatePane(Direction::Up));
        bindings.insert(prefix_key!(Right), Action::NavigatePane(Direction::Right));
        
        bindings.insert(prefix_key!(h), Action::NavigatePane(Direction::Left));
        bindings.insert(prefix_key!(j), Action::NavigatePane(Direction::Down));
        bindings.insert(prefix_key!(k), Action::NavigatePane(Direction::Up));
        bindings.insert(prefix_key!(l), Action::NavigatePane(Direction::Right));
        
        bindings.insert(prefix_key!(o), Action::NextPane);
        bindings.insert(prefix_key!(;), Action::LastPane);
        bindings.insert(prefix_key!(q), Action::ShowPaneNumbers);
        
        // Pane Resizing
        bindings.insert(prefix_key!(H), Action::ResizePane(Direction::Left, 5));
        bindings.insert(prefix_key!(J), Action::ResizePane(Direction::Down, 5));
        bindings.insert(prefix_key!(K), Action::ResizePane(Direction::Up, 5));
        bindings.insert(prefix_key!(L), Action::ResizePane(Direction::Right, 5));
        
        bindings.insert(prefix_key!(Ctrl-Left), Action::ResizePane(Direction::Left, 1));
        bindings.insert(prefix_key!(Ctrl-Down), Action::ResizePane(Direction::Down, 1));
        bindings.insert(prefix_key!(Ctrl-Up), Action::ResizePane(Direction::Up, 1));
        bindings.insert(prefix_key!(Ctrl-Right), Action::ResizePane(Direction::Right, 1));
        
        // Pane Operations
        bindings.insert(prefix_key!(x), Action::KillPane);
        bindings.insert(prefix_key!(z), Action::ZoomPane);
        bindings.insert(prefix_key!(!), Action::BreakPane);
        bindings.insert(prefix_key!(@), Action::JoinPane);
        bindings.insert(prefix_key!({), Action::SwapPaneUp);
        bindings.insert(prefix_key!(}), Action::SwapPaneDown);
        bindings.insert(prefix_key!(Space), Action::NextLayout);
        bindings.insert(prefix_key!(Ctrl-O), Action::RotatePanes);
        
        // Copy Mode
        bindings.insert(prefix_key!([), Action::EnterCopyMode);
        bindings.insert(prefix_key!(]), Action::PasteBuffer);
        bindings.insert(prefix_key!(=), Action::ShowBuffers);
        bindings.insert(prefix_key!(#), Action::ListBuffers);
        
        // Special Features
        bindings.insert(prefix_key!(B), Action::ToggleBroadcast);
        bindings.insert(prefix_key!(m), Action::ToggleMouse);
        bindings.insert(prefix_key!(r), Action::ReloadConfig);
        bindings.insert(prefix_key!(?), Action::ShowKeybindings);
        bindings.insert(prefix_key!(:), Action::CommandPrompt);
        bindings.insert(prefix_key!(t), Action::ShowClock);
        bindings.insert(prefix_key!(i), Action::DisplayMessage);
        
        // Layouts
        bindings.insert(prefix_key!(M-1), Action::SelectLayout(Layout::Even));
        bindings.insert(prefix_key!(M-2), Action::SelectLayout(Layout::Tall));
        bindings.insert(prefix_key!(M-3), Action::SelectLayout(Layout::Wide));
        bindings.insert(prefix_key!(M-4), Action::SelectLayout(Layout::Main));
        bindings.insert(prefix_key!(M-5), Action::SelectLayout(Layout::Grid));
        
        bindings
    };
}
```

### Copy Mode Bindings
```rust
lazy_static! {
    static ref COPY_MODE_VI_BINDINGS: HashMap<KeyBinding, CopyAction> = {
        let mut bindings = HashMap::new();
        
        // Movement
        bindings.insert(key!(h), CopyAction::MoveLeft);
        bindings.insert(key!(j), CopyAction::MoveDown);
        bindings.insert(key!(k), CopyAction::MoveUp);
        bindings.insert(key!(l), CopyAction::MoveRight);
        
        bindings.insert(key!(w), CopyAction::NextWord);
        bindings.insert(key!(b), CopyAction::PreviousWord);
        bindings.insert(key!(e), CopyAction::EndOfWord);
        
        bindings.insert(key!(0), CopyAction::StartOfLine);
        bindings.insert(key!($), CopyAction::EndOfLine);
        
        bindings.insert(key!(g), CopyAction::FirstLine);
        bindings.insert(key!(G), CopyAction::LastLine);
        
        bindings.insert(key!(Ctrl-F), CopyAction::PageDown);
        bindings.insert(key!(Ctrl-B), CopyAction::PageUp);
        bindings.insert(key!(Ctrl-D), CopyAction::HalfPageDown);
        bindings.insert(key!(Ctrl-U), CopyAction::HalfPageUp);
        
        // Selection
        bindings.insert(key!(v), CopyAction::BeginSelection);
        bindings.insert(key!(V), CopyAction::SelectLine);
        bindings.insert(key!(Ctrl-V), CopyAction::SelectBlock);
        
        // Copy
        bindings.insert(key!(y), CopyAction::CopyAndExit);
        bindings.insert(key!(Y), CopyAction::CopyLine);
        bindings.insert(key!(Enter), CopyAction::CopyAndExit);
        
        // Search
        bindings.insert(key!(/), CopyAction::SearchForward);
        bindings.insert(key!(?), CopyAction::SearchBackward);
        bindings.insert(key!(n), CopyAction::NextMatch);
        bindings.insert(key!(N), CopyAction::PreviousMatch);
        
        // Exit
        bindings.insert(key!(q), CopyAction::Exit);
        bindings.insert(key!(Escape), CopyAction::Exit);
        
        bindings
    };
    
    static ref COPY_MODE_EMACS_BINDINGS: HashMap<KeyBinding, CopyAction> = {
        let mut bindings = HashMap::new();
        
        // Movement
        bindings.insert(key!(Ctrl-B), CopyAction::MoveLeft);
        bindings.insert(key!(Ctrl-N), CopyAction::MoveDown);
        bindings.insert(key!(Ctrl-P), CopyAction::MoveUp);
        bindings.insert(key!(Ctrl-F), CopyAction::MoveRight);
        
        bindings.insert(key!(M-f), CopyAction::NextWord);
        bindings.insert(key!(M-b), CopyAction::PreviousWord);
        
        bindings.insert(key!(Ctrl-A), CopyAction::StartOfLine);
        bindings.insert(key!(Ctrl-E), CopyAction::EndOfLine);
        
        bindings.insert(key!(M-<), CopyAction::FirstLine);
        bindings.insert(key!(M->), CopyAction::LastLine);
        
        bindings.insert(key!(Ctrl-V), CopyAction::PageDown);
        bindings.insert(key!(M-v), CopyAction::PageUp);
        
        // Selection
        bindings.insert(key!(Ctrl-Space), CopyAction::BeginSelection);
        
        // Copy
        bindings.insert(key!(M-w), CopyAction::CopyAndExit);
        bindings.insert(key!(Ctrl-W), CopyAction::CopyAndExit);
        
        // Search
        bindings.insert(key!(Ctrl-S), CopyAction::SearchForward);
        bindings.insert(key!(Ctrl-R), CopyAction::SearchBackward);
        
        // Exit
        bindings.insert(key!(Ctrl-G), CopyAction::Exit);
        bindings.insert(key!(Escape), CopyAction::Exit);
        
        bindings
    };
}
```

### Custom Keybinding System
```rust
impl KeybindingManager {
    fn load_custom_bindings(&mut self, config: &Config) {
        for (key_str, action_str) in &config.keybindings {
            if let Ok(binding) = self.parse_key_binding(key_str) {
                if let Ok(action) = self.parse_action(action_str) {
                    self.custom_bindings.insert(binding, action);
                }
            }
        }
    }
    
    fn parse_key_binding(&self, s: &str) -> Result<KeyBinding> {
        // Parse formats like:
        // "C-a" -> Ctrl+A
        // "M-x" -> Alt+X
        // "S-Tab" -> Shift+Tab
        // "prefix c" -> Prefix then C
        // "C-M-x" -> Ctrl+Alt+X
        
        let parts: Vec<&str> = s.split('-').collect();
        let mut modifiers = KeyModifiers::empty();
        let mut key = None;
        let mut is_prefix = false;
        
        for part in parts {
            match part {
                "C" | "Ctrl" => modifiers |= KeyModifiers::CONTROL,
                "M" | "Alt" | "Meta" => modifiers |= KeyModifiers::ALT,
                "S" | "Shift" => modifiers |= KeyModifiers::SHIFT,
                "prefix" => is_prefix = true,
                other => key = Some(self.parse_key(other)?),
            }
        }
        
        Ok(KeyBinding {
            key: key.ok_or("No key specified")?,
            modifiers,
            is_prefix,
        })
    }
}
```

---

## STATUS BAR SYSTEM

### Status Bar Configuration
```rust
#[derive(Debug, Clone, Serialize, Deserialize)]
struct StatusBar {
    // Layout
    position: StatusBarPosition,  // Bottom (always)
    height: u16,                  // 1 line (always)
    
    // Components
    left: Vec<StatusComponent>,
    center: Vec<StatusComponent>,
    right: Vec<StatusComponent>,
    
    // Settings
    refresh_interval: Duration,
    datetime_format: String,
    
    // Mode indicator (always far left)
    mode_indicator: ModeIndicator,
}

#[derive(Debug, Clone, Serialize, Deserialize)]
enum StatusComponent {
    // Core components
    ModeIndicator,
    SessionName,
    WindowList,
    PaneTitle,
    
    // User & System
    Username,
    Hostname,
    UserAtHost,
    
    // VCS
    GitBranch,
    GitStatus,
    
    // System Info
    Uptime,
    LoadAverage,
    Memory,
    CPU,
    
    // Optional
    Weather,
    Battery,
    Network,
    
    // Time
    DateTime,
    
    // Features
    ContinuumStatus,
    ZoomIndicator,
    BroadcastIndicator,
    
    // Custom
    Custom(String),
}

impl Default for StatusBar {
    fn default() -> Self {
        Self {
            position: StatusBarPosition::Bottom,
            height: 1,
            
            // Default layout from your tmux config:
            // [MODE] | Sess:#S | user@host | git | UP:time | Continuum | date time
            left: vec![
                StatusComponent::ModeIndicator,
                StatusComponent::SessionName,
                StatusComponent::UserAtHost,
                StatusComponent::GitStatus,
            ],
            
            center: vec![],
            
            right: vec![
                StatusComponent::Uptime,
                StatusComponent::ContinuumStatus,
                StatusComponent::DateTime,
            ],
            
            refresh_interval: Duration::from_secs(15),
            datetime_format: "%m/%d %H:%M".to_string(),
            
            mode_indicator: ModeIndicator::default(),
        }
    }
}
```

### Mode Indicator Implementation
```rust
#[derive(Debug, Clone, Serialize, Deserialize)]
struct ModeIndicator {
    position: ModePosition,  // Always far left
    style: ModeStyle,
    colors: ModeColors,
}

#[derive(Debug, Clone, Serialize, Deserialize)]
struct ModeColors {
    normal: Option<Color>,      // No indicator in normal mode
    prefix: Color,              // Magenta background
    copy: Color,                // Yellow background
    broadcast: Color,           // Red background, blinking
    zen: Color,                 // Purple background
}

impl ModeIndicator {
    fn render(&self, mode: &Mode) -> String {
        match mode {
            Mode::Normal => String::new(),  // No indicator
            Mode::Prefix => self.styled("[PREFIX]", self.colors.prefix),
            Mode::Copy => self.styled("[COPY]", self.colors.copy),
            Mode::Broadcast(count) => {
                self.styled_blinking(
                    &format!("[BROADCASTING TO {} PANES]", count),
                    self.colors.broadcast
                )
            },
            Mode::Zen => self.styled("[ZEN]", self.colors.zen),
        }
    }
}
```

### Status Bar Rendering
```rust
impl StatusBar {
    fn render(&self, width: u16) -> String {
        let mode_str = self.mode_indicator.render(&self.current_mode);
        let mode_width = mode_str.width();
        
        // Calculate available space
        let available_width = width - mode_width - 2; // -2 for separators
        
        // Render components
        let left = self.render_components(&self.left);
        let center = self.render_components(&self.center);
        let right = self.render_components(&self.right);
        
        // Layout algorithm
        if mode_str.is_empty() {
            self.layout_three_sections(left, center, right, width)
        } else {
            format!("{} | {}", 
                mode_str,
                self.layout_three_sections(left, center, right, available_width)
            )
        }
    }
    
    fn render_components(&self, components: &[StatusComponent]) -> String {
        components
            .iter()
            .filter_map(|c| self.render_component(c))
            .collect::<Vec<_>>()
            .join(" | ")
    }
    
    fn render_component(&self, component: &StatusComponent) -> Option<String> {
        match component {
            StatusComponent::SessionName => {
                Some(format!("Sess:{}", self.session_name))
            }
            StatusComponent::UserAtHost => {
                Some(format!("{}@{}", self.username, self.hostname))
            }
            StatusComponent::GitStatus => {
                if let Some(git) = &self.git_info {
                    Some(format!("git:{}{}", 
                        git.branch,
                        if git.dirty { "*" } else { "" }
                    ))
                } else {
                    None
                }
            }
            StatusComponent::Uptime => {
                Some(format!("UP:{}", self.format_uptime()))
            }
            StatusComponent::ContinuumStatus => {
                Some(format!("Continuum:{}", 
                    if self.last_save_ok { "✓" } else { "✗" }
                ))
            }
            StatusComponent::DateTime => {
                Some(chrono::Local::now().format(&self.datetime_format).to_string())
            }
            StatusComponent::Weather => {
                self.weather_info.as_ref().map(|w| w.to_string())
            }
            _ => None,
        }
    }
}
```

---

## COPY AND PASTE SYSTEM

### Clean Copy/Paste Implementation
```rust
#[derive(Debug)]
struct CopyPasteManager {
    // Selection state
    selection: Option<Selection>,
    selection_mode: SelectionMode,
    
    // Clipboard integration
    system_clipboard: ClipboardManager,
    primary_selection: Option<ClipboardManager>,  // Linux only
    
    // History
    clipboard_history: VecDeque<ClipboardEntry>,
    max_history: usize,
    
    // Settings
    mouse_enabled: bool,
    auto_copy: bool,
    confirm_large_paste: bool,
    bracket_paste_mode: bool,
}

#[derive(Debug, Clone)]
enum SelectionMode {
    Character,      // Normal character selection
    Word,          // Word boundaries
    Line,          // Full lines
    Block,         // Rectangular block
    Semantic,      // Smart selection (URLs, paths, etc.)
}

impl CopyPasteManager {
    fn handle_mouse_event(&mut self, event: MouseEvent) -> Result<()> {
        if !self.mouse_enabled {
            return Ok(());
        }
        
        match event {
            MouseEvent::Down { x, y, button: MouseButton::Left } => {
                self.start_selection(x, y);
            }
            MouseEvent::Drag { x, y } => {
                self.update_selection(x, y);
                self.highlight_selection();
            }
            MouseEvent::Up { button: MouseButton::Left } => {
                if self.auto_copy {
                    self.copy_selection()?;
                }
            }
            MouseEvent::DoubleClick { x, y } => {
                self.select_word_at(x, y);
                if self.auto_copy {
                    self.copy_selection()?;
                }
            }
            MouseEvent::TripleClick { x, y } => {
                self.select_line_at(x, y);
                if self.auto_copy {
                    self.copy_selection()?;
                }
            }
            MouseEvent::Down { button: MouseButton::Middle, .. } => {
                self.paste_primary()?;
            }
            MouseEvent::Down { button: MouseButton::Right, .. } => {
                self.show_context_menu()?;
            }
            _ => {}
        }
        
        Ok(())
    }
    
    fn copy_selection(&mut self) -> Result<()> {
        if let Some(selection) = &self.selection {
            let text = self.get_selected_text(selection)?;
            
            // Clean the text (no ANSI codes or artifacts)
            let clean_text = self.clean_text(&text);
            
            // Add to history
            self.add_to_history(clean_text.clone());
            
            // Copy to system clipboard
            self.system_clipboard.set_text(clean_text)?;
            
            // Copy to primary selection on Linux
            #[cfg(target_os = "linux")]
            if let Some(primary) = &mut self.primary_selection {
                primary.set_text(clean_text)?;
            }
            
            // Clear selection
            self.clear_selection();
            
            // Show confirmation
            self.show_message("Copied to clipboard")?;
        }
        
        Ok(())
    }
    
    fn paste(&mut self) -> Result<()> {
        let text = self.system_clipboard.get_text()?;
        self.paste_text(text)
    }
    
    fn paste_text(&mut self, text: String) -> Result<()> {
        // Check for dangerous content
        if self.should_confirm_paste(&text) {
            if !self.confirm_paste_dialog(&text)? {
                return Ok(());
            }
        }
        
        // Apply bracketed paste mode if enabled
        let text_to_paste = if self.bracket_paste_mode {
            format!("\x1b[200~{}\x1b[201~", text)
        } else {
            text
        };
        
        // Send to active pane
        self.send_to_pane(text_to_paste)?;
        
        Ok(())
    }
    
    fn should_confirm_paste(&self, text: &str) -> bool {
        if !self.confirm_large_paste {
            return false;
        }
        
        // Check for multiline paste
        let line_count = text.lines().count();
        if line_count > 5 {
            return true;
        }
        
        // Check for dangerous patterns
        const DANGEROUS_PATTERNS: &[&str] = &[
            "rm -rf",
            "sudo",
            "curl | sh",
            "wget | sh",
            ":(){ :|:& };:",  // Fork bomb
        ];
        
        DANGEROUS_PATTERNS.iter().any(|pattern| {
            text.contains(pattern)
        })
    }
}
```

### Smart Selection
```rust
impl SmartSelection {
    fn select_semantic_unit(&self, x: u16, y: u16) -> Selection {
        let text = self.get_line_at(y);
        let pos = x as usize;
        
        // Try patterns in order of specificity
        if let Some(sel) = self.select_url_at(&text, pos) {
            return sel;
        }
        
        if let Some(sel) = self.select_file_path_at(&text, pos) {
            return sel;
        }
        
        if let Some(sel) = self.select_ip_address_at(&text, pos) {
            return sel;
        }
        
        if let Some(sel) = self.select_git_hash_at(&text, pos) {
            return sel;
        }
        
        if let Some(sel) = self.select_uuid_at(&text, pos) {
            return sel;
        }
        
        if let Some(sel) = self.select_quoted_text_at(&text, pos) {
            return sel;
        }
        
        if let Some(sel) = self.select_bracketed_text_at(&text, pos) {
            return sel;
        }
        
        // Fall back to word selection
        self.select_word_at(&text, pos)
    }
    
    fn select_url_at(&self, text: &str, pos: usize) -> Option<Selection> {
        // URL regex pattern
        let url_pattern = regex!(
            r"(?:https?|ftp|ssh|git)://[^\s<>{}|\^`\[\]']+"
        );
        
        for match_ in url_pattern.find_iter(text) {
            if match_.start() <= pos && pos <= match_.end() {
                return Some(Selection {
                    start: (match_.start() as u16, self.current_line),
                    end: (match_.end() as u16, self.current_line),
                    mode: SelectionMode::Semantic,
                });
            }
        }
        
        None
    }
}
```

### Clipboard Manager UI
```rust
impl ClipboardHistoryUI {
    fn render(&self) -> Result<()> {
        /*
        ┌─ Clipboard History ───────────────────────┐
        │ Search: [________________] 🔍             │
        ├───────────────────────────────────────────┤
        │ 1. [2m ago] git commit -m "Fix bug"      │
        │ 2. [5m ago] https://example.com/api      │
        │ 3. [10m ago] SELECT * FROM users WHERE.. │
        │ 4. [1h ago] def function_name():         │
        │ 5. [2h ago] ~/.config/casmux/config.yml  │
        ├───────────────────────────────────────────┤
        │ Preview:                                  │
        │ git commit -m "Fix bug in authentication"│
        │ system that was causing login failures"  │
        │                                           │
        │ [↵ Paste] [Del Remove] [C Clear All]     │
        └───────────────────────────────────────────┘
        */
        
        let selected = self.selected_entry();
        
        // Render list
        for (i, entry) in self.entries.iter().enumerate() {
            let time_ago = self.format_time_ago(entry.timestamp);
            let preview = self.truncate_preview(&entry.text, 40);
            
            if i == self.selected_index {
                println!("▶ {}. [{}] {}", i + 1, time_ago, preview);
            } else {
                println!("  {}. [{}] {}", i + 1, time_ago, preview);
            }
        }
        
        // Show full preview of selected
        if let Some(entry) = selected {
            println!("\nPreview:");
            println!("{}", entry.text);
        }
        
        Ok(())
    }
}
```

---

## BROADCAST MODE

### Broadcast System Implementation
```rust
#[derive(Debug)]
struct BroadcastManager {
    enabled: bool,
    target_panes: HashSet<PaneId>,
    visual_indicators: BroadcastVisuals,
    safety_config: BroadcastSafety,
    
    // Smart detection
    ssh_panes: HashSet<PaneId>,
    local_panes: HashSet<PaneId>,
}

#[derive(Debug)]
struct BroadcastVisuals {
    border_color: Color,
    border_style: BorderStyle,
    status_indicator: String,
    pane_icon: String,
    flash_on_input: bool,
}

#[derive(Debug)]
struct BroadcastSafety {
    confirm_enable: bool,
    exclude_local_by_default: bool,
    auto_disable_on_error: bool,
    visual_warning_level: WarningLevel,
}

impl BroadcastManager {
    fn toggle(&mut self) -> Result<()> {
        if self.enabled {
            self.disable()
        } else {
            self.enable()
        }
    }
    
    fn enable(&mut self) -> Result<()> {
        // Detect SSH sessions
        self.detect_ssh_panes();
        
        // Show confirmation dialog
        if self.safety_config.confirm_enable {
            let message = if !self.ssh_panes.is_empty() {
                format!(
                    "Enable broadcast to {} SSH panes?\n\
                     This will type in multiple servers simultaneously.\n\
                     Press Y to confirm, N to cancel.",
                    self.ssh_panes.len()
                )
            } else {
                format!(
                    "Enable broadcast to {} panes?\n\
                     Everything you type will appear in all panes.\n\
                     Press Y to confirm, N to cancel.",
                    self.target_panes.len()
                )
            };
            
            if !self.confirm_dialog(&message)? {
                return Ok(());
            }
        }
        
        // Set target panes
        if self.safety_config.exclude_local_by_default && !self.ssh_panes.is_empty() {
            // Only broadcast to SSH panes
            self.target_panes = self.ssh_panes.clone();
        } else {
            // Broadcast to all panes
            self.target_panes = self.get_all_panes();
        }
        
        // Enable visual indicators
        self.enable_visual_indicators()?;
        
        // Update status bar
        self.update_status_bar()?;
        
        self.enabled = true;
        
        // Show notification
        self.show_notification(
            &format!("Broadcasting to {} panes", self.target_panes.len())
        )?;
        
        Ok(())
    }
    
    fn disable(&mut self) -> Result<()> {
        self.enabled = false;
        self.target_panes.clear();
        
        // Remove visual indicators
        self.disable_visual_indicators()?;
        
        // Update status bar
        self.update_status_bar()?;
        
        // Show notification
        self.show_notification("Broadcast mode disabled")?;
        
        Ok(())
    }
    
    fn enable_visual_indicators(&self) -> Result<()> {
        // Set pane borders based on number of panes
        let (color, style) = match self.target_panes.len() {
            1..=2 => (Color::Yellow, BorderStyle::Single),
            3..=4 => (Color::Orange, BorderStyle::Double),
            _ => (Color::Red, BorderStyle::Heavy),
        };
        
        for &pane_id in &self.target_panes {
            self.set_pane_border(pane_id, color, style)?;
            self.set_pane_icon(pane_id, "📡")?;
        }
        
        Ok(())
    }
    
    fn detect_ssh_panes(&mut self) {
        self.ssh_panes.clear();
        self.local_panes.clear();
        
        for pane in self.get_all_panes() {
            if let Some(process) = self.get_pane_process(pane) {
                if process.name == "ssh" || 
                   process.name == "mosh" ||
                   process.cmdline.contains("ssh ") {
                    self.ssh_panes.insert(pane);
                } else {
                    self.local_panes.insert(pane);
                }
            }
        }
        
        // Smart detection for similar SSH hosts
        if self.ssh_panes.len() >= 3 {
            // Check if connecting to similar hosts (e.g., web-01, web-02, web-03)
            let hosts = self.extract_ssh_hosts();
            if self.are_hosts_similar(&hosts) {
                self.suggest_broadcast_template(&hosts)?;
            }
        }
    }
    
    fn broadcast_input(&self, input: &str) -> Result<()> {
        if !self.enabled {
            return Ok(());
        }
        
        // Flash borders on input
        if self.visual_indicators.flash_on_input {
            self.flash_borders()?;
        }
        
        // Send to all target panes
        for &pane_id in &self.target_panes {
            self.send_to_pane(pane_id, input)?;
        }
        
        Ok(())
    }
}
```

### Broadcast UI
```rust
impl BroadcastUI {
    fn show_broadcast_menu(&self) -> Result<()> {
        /*
        ┌─ Broadcast to Panes ──────────────────────┐
        │                                           │
        │ ⚠️  Type once, execute everywhere!        │
        │                                           │
        │ Select target panes:                      │
        │ ☑ server1 (SSH: prod-web-01)             │
        │ ☑ server2 (SSH: prod-web-02)             │
        │ ☑ server3 (SSH: prod-web-03)             │
        │ ☐ local (bash)                           │
        │ ☐ database (SSH: prod-db-01)             │
        │                                           │
        │ Presets:                                  │
        │ [All] [SSH Only] [Local Only] [None]     │
        │                                           │
        │ [Start Broadcasting]  [Cancel]           │
        │                                           │
        │ Safety: Red borders will show active     │
        │         Press Prefix+B to stop           │
        └───────────────────────────────────────────┘
        */
        
        Ok(())
    }
}
```

---

## SEARCH SYSTEM

### Search Implementation
```rust
#[derive(Debug)]
struct SearchManager {
    // Current search
    current_query: String,
    current_matches: Vec<SearchMatch>,
    current_index: usize,
    
    // Search settings
    case_sensitive: bool,
    use_regex: bool,
    wrap_search: bool,
    incremental: bool,
    
    // Search history
    search_history: VecDeque<String>,
    max_history: usize,
    
    // Search scope
    scope: SearchScope,
}

#[derive(Debug, Clone)]
enum SearchScope {
    CurrentPane,
    AllPanes,
    CurrentWindow,
    AllWindows,
    Files(Vec<PathBuf>),
    Commands,
}

#[derive(Debug, Clone)]
struct SearchMatch {
    pane_id: PaneId,
    line_number: usize,
    column: usize,
    text: String,
    context_before: String,
    context_after: String,
}

impl SearchManager {
    fn search(&mut self, query: &str) -> Result<Vec<SearchMatch>> {
        // Add to history
        self.add_to_history(query.to_string());
        
        // Compile search pattern
        let pattern = self.compile_pattern(query)?;
        
        // Search based on scope
        let matches = match self.scope {
            SearchScope::CurrentPane => {
                self.search_pane(self.current_pane(), &pattern)
            }
            SearchScope::AllPanes => {
                self.search_all_panes(&pattern)
            }
            SearchScope::CurrentWindow => {
                self.search_window(self.current_window(), &pattern)
            }
            SearchScope::AllWindows => {
                self.search_all_windows(&pattern)
            }
            SearchScope::Files(ref paths) => {
                self.search_files(paths, &pattern)
            }
            SearchScope::Commands => {
                self.search_commands(&pattern)
            }
        }?;
        
        self.current_matches = matches.clone();
        self.current_index = 0;
        
        Ok(matches)
    }
    
    fn incremental_search(&mut self, partial: &str) -> Result<Vec<SearchMatch>> {
        if !self.incremental {
            return Ok(vec![]);
        }
        
        // Don't search for very short queries
        if partial.len() < 2 {
            return Ok(vec![]);
        }
        
        // Limit results for performance
        let pattern = self.compile_pattern(partial)?;
        let matches = self.search_limited(&pattern, 50)?;
        
        Ok(matches)
    }
    
    fn search_pane(&self, pane: &Pane, pattern: &SearchPattern) -> Result<Vec<SearchMatch>> {
        let mut matches = Vec::new();
        let content = pane.get_scrollback();
        
        for (line_num, line) in content.lines().enumerate() {
            if let Some(columns) = pattern.find_in_line(line) {
                for col in columns {
                    matches.push(SearchMatch {
                        pane_id: pane.id,
                        line_number: line_num,
                        column: col,
                        text: line.to_string(),
                        context_before: self.get_context_before(&content, line_num),
                        context_after: self.get_context_after(&content, line_num),
                    });
                }
            }
        }
        
        Ok(matches)
    }
    
    fn highlight_matches(&self) -> Result<()> {
        for match_ in &self.current_matches {
            self.highlight_match(match_)?;
        }
        
        // Highlight current match differently
        if let Some(current) = self.current_matches.get(self.current_index) {
            self.highlight_current_match(current)?;
        }
        
        Ok(())
    }
}
```

### Search UI
```rust
impl SearchUI {
    fn render_pane_search(&self) -> Result<()> {
        /*
        ┌─ Search ───────────────────────────────────┐
        │ Find: docker█                              │
        ├────────────────────────────────────────────┤
        │ [3/15 matches]                             │
        │                                            │
        │ 42: Starting docker-compose...            │
        │ 87: docker build -t myapp:latest .        │
        │ 92: Successfully built docker image       │
        │                                            │
        │ Options:                                  │
        │ [✓] Case sensitive  [✓] Regex  [✓] Wrap  │
        │                                            │
        │ [n Next] [N Previous] [Enter Select]     │
        │ [/ New Search] [Esc Cancel]              │
        └────────────────────────────────────────────┘
        */
        
        Ok(())
    }
    
    fn render_global_search(&self) -> Result<()> {
        /*
        ┌─ Search All Panes ─────────────────────────┐
        │ Find: error█                               │
        ├────────────────────────────────────────────┤
        │ Results by Window:                        │
        │                                            │
        │ Window: webapp                             │
        │   Pane 1 (editor):     2 matches         │
        │   Pane 2 (server):     5 matches         │
        │   Pane 3 (logs):      12 matches         │
        │                                            │
        │ Window: database                          │
        │   Pane 1 (psql):       1 match           │
        │                                            │
        │ Total: 20 matches in 4 panes             │
        │                                            │
        │ [Enter Jump to Match] [Tab Next Pane]    │
        │ [F Filter Results] [Esc Cancel]          │
        └────────────────────────────────────────────┘
        */
        
        Ok(())
    }
}
```

---

## PROJECT DETECTION AND TEMPLATES

### Project Detection System
```rust
#[derive(Debug, Clone, PartialEq)]
enum ProjectType {
    Rust,
    Node,
    Python,
    Go,
    Ruby,
    Java,
    DotNet,
    PHP,
    Docker,
    Kubernetes,
    Terraform,
    Ansible,
    Generic,
}

struct ProjectDetector {
    root: PathBuf,
    project_type: Option<ProjectType>,
    metadata: ProjectMetadata,
}

#[derive(Debug, Clone)]
struct ProjectMetadata {
    name: String,
    version: Option<String>,
    vcs: Option<VcsType>,
    dependencies: Vec<String>,
    scripts: HashMap<String, String>,
    environments: Vec<String>,
}

impl ProjectDetector {
    fn detect(path: &Path) -> ProjectInfo {
        let mut detector = Self {
            root: Self::find_project_root(path),
            project_type: None,
            metadata: ProjectMetadata::default(),
        };
        
        // Detection priority order
        detector.detect_vcs();
        detector.detect_by_manifest();
        detector.detect_by_structure();
        detector.extract_metadata();
        
        ProjectInfo {
            root: detector.root,
            project_type: detector.project_type.unwrap_or(ProjectType::Generic),
            metadata: detector.metadata,
        }
    }
    
    fn find_project_root(path: &Path) -> PathBuf {
        let mut current = path.to_path_buf();
        
        // Walk up looking for VCS or project files
        while current.parent().is_some() {
            // VCS roots
            if current.join(".git").exists() ||
               current.join(".hg").exists() ||
               current.join(".svn").exists() {
                return current;
            }
            
            // Project files
            if current.join("Cargo.toml").exists() ||
               current.join("package.json").exists() ||
               current.join("go.mod").exists() ||
               current.join("requirements.txt").exists() ||
               current.join("Gemfile").exists() ||
               current.join("pom.xml").exists() ||
               current.join("docker-compose.yml").exists() {
                return current;
            }
            
            current = current.parent().unwrap().to_path_buf();
        }
        
        // Default to original path
        path.to_path_buf()
    }
    
    fn detect_by_manifest(&mut self) {
        // Rust
        if self.root.join("Cargo.toml").exists() {
            self.project_type = Some(ProjectType::Rust);
            self.parse_cargo_toml();
            return;
        }
        
        // Node.js
        if self.root.join("package.json").exists() {
            self.project_type = Some(ProjectType::Node);
            self.parse_package_json();
            return;
        }
        
        // Python
        if self.root.join("requirements.txt").exists() ||
           self.root.join("setup.py").exists() ||
           self.root.join("Pipfile").exists() ||
           self.root.join("pyproject.toml").exists() {
            self.project_type = Some(ProjectType::Python);
            self.parse_python_files();
            return;
        }
        
        // Go
        if self.root.join("go.mod").exists() {
            self.project_type = Some(ProjectType::Go);
            self.parse_go_mod();
            return;
        }
        
        // Ruby
        if self.root.join("Gemfile").exists() {
            self.project_type = Some(ProjectType::Ruby);
            self.parse_gemfile();
            return;
        }
        
        // Java
        if self.root.join("pom.xml").exists() {
            self.project_type = Some(ProjectType::Java);
            self.parse_pom_xml();
            return;
        }
        
        // .NET
        if self.root.join("*.csproj").exists() ||
           self.root.join("*.sln").exists() {
            self.project_type = Some(ProjectType::DotNet);
            self.parse_dotnet_files();
            return;
        }
        
        // Docker
        if self.root.join("docker-compose.yml").exists() ||
           self.root.join("Dockerfile").exists() {
            self.project_type = Some(ProjectType::Docker);
            self.parse_docker_files();
            return;
        }
        
        // Kubernetes
        if self.root.join("k8s").exists() ||
           self.root.join("deployments").exists() ||
           self.root.join("helm").exists() {
            self.project_type = Some(ProjectType::Kubernetes);
            return;
        }
    }
    
    fn parse_package_json(&mut self) {
        if let Ok(content) = fs::read_to_string(self.root.join("package.json")) {
            if let Ok(json) = serde_json::from_str::<Value>(&content) {
                // Extract name
                if let Some(name) = json["name"].as_str() {
                    self.metadata.name = name.to_string();
                }
                
                // Extract version
                if let Some(version) = json["version"].as_str() {
                    self.metadata.version = Some(version.to_string());
                }
                
                // Extract scripts
                if let Some(scripts) = json["scripts"].as_object() {
                    for (key, value) in scripts {
                        if let Some(cmd) = value.as_str() {
                            self.metadata.scripts.insert(key.clone(), cmd.to_string());
                        }
                    }
                }
                
                // Detect framework
                let deps = json["dependencies"].as_object();
                let dev_deps = json["devDependencies"].as_object();
                
                if deps.map(|d| d.contains_key("react")).unwrap_or(false) {
                    self.metadata.dependencies.push("react".to_string());
                }
                if deps.map(|d| d.contains_key("vue")).unwrap_or(false) {
                    self.metadata.dependencies.push("vue".to_string());
                }
                if deps.map(|d| d.contains_key("@angular/core")).unwrap_or(false) {
                    self.metadata.dependencies.push("angular".to_string());
                }
                if deps.map(|d| d.contains_key("express")).unwrap_or(false) {
                    self.metadata.dependencies.push("express".to_string());
                }
            }
        }
    }
}
```

### Template System
```rust
#[derive(Debug, Clone, Serialize, Deserialize)]
struct ProjectTemplate {
    name: String,
    description: String,
    project_type: ProjectType,
    windows: Vec<WindowTemplate>,
    environment: HashMap<String, String>,
    hooks: TemplateHooks,
}

#[derive(Debug, Clone, Serialize, Deserialize)]
struct WindowTemplate {
    name: String,
    layout: Layout,
    panes: Vec<PaneTemplate>,
}

#[derive(Debug, Clone, Serialize, Deserialize)]
struct PaneTemplate {
    command: Option<String>,
    directory: Option<PathBuf>,
    environment: HashMap<String, String>,
    size: Option<PaneSize>,
}

#[derive(Debug, Clone, Serialize, Deserialize)]
struct TemplateHooks {
    on_create: Option<String>,
    on_attach: Option<String>,
    on_detach: Option<String>,
}

// Built-in Templates
impl Templates {
    fn rust() -> ProjectTemplate {
        ProjectTemplate {
            name: "rust".to_string(),
            description: "Rust development environment".to_string(),
            project_type: ProjectType::Rust,
            windows: vec![
                WindowTemplate {
                    name: "editor".to_string(),
                    layout: Layout::Even,
                    panes: vec![
                        PaneTemplate {
                            command: Some("$EDITOR Cargo.toml".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
                WindowTemplate {
                    name: "build".to_string(),
                    layout: Layout::Even,
                    panes: vec![
                        PaneTemplate {
                            command: Some("cargo watch -x check".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
                WindowTemplate {
                    name: "test".to_string(),
                    layout: Layout::Even,
                    panes: vec![
                        PaneTemplate {
                            command: Some("cargo watch -x test".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
                WindowTemplate {
                    name: "run".to_string(),
                    layout: Layout::Even,
                    panes: vec![
                        PaneTemplate {
                            command: Some("cargo run".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
            ],
            environment: HashMap::from([
                ("RUST_BACKTRACE".to_string(), "1".to_string()),
            ]),
            hooks: TemplateHooks {
                on_create: Some("cargo build".to_string()),
                on_attach: None,
                on_detach: None,
            },
        }
    }
    
    fn node() -> ProjectTemplate {
        ProjectTemplate {
            name: "node".to_string(),
            description: "Node.js development environment".to_string(),
            project_type: ProjectType::Node,
            windows: vec![
                WindowTemplate {
                    name: "editor".to_string(),
                    layout: Layout::Even,
                    panes: vec![
                        PaneTemplate {
                            command: Some("$EDITOR package.json".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
                WindowTemplate {
                    name: "dev".to_string(),
                    layout: Layout::Even,
                    panes: vec![
                        PaneTemplate {
                            command: Some("npm run dev || npm start".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
                WindowTemplate {
                    name: "test".to_string(),
                    layout: Layout::Even,
                    panes: vec![
                        PaneTemplate {
                            command: Some("npm test -- --watch".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
                WindowTemplate {
                    name: "logs".to_string(),
                    layout: Layout::Even,
                    panes: vec![
                        PaneTemplate {
                            command: Some("tail -f logs/*.log 2>/dev/null || echo 'No logs'".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
            ],
            environment: HashMap::from([
                ("NODE_ENV".to_string(), "development".to_string()),
            ]),
            hooks: TemplateHooks {
                on_create: Some("npm install".to_string()),
                on_attach: None,
                on_detach: None,
            },
        }
    }
    
    fn servers() -> ProjectTemplate {
        ProjectTemplate {
            name: "servers".to_string(),
            description: "Multi-server management".to_string(),
            project_type: ProjectType::Generic,
            windows: vec![
                WindowTemplate {
                    name: "web-servers".to_string(),
                    layout: Layout::Grid,
                    panes: vec![
                        PaneTemplate {
                            command: Some("ssh web-01".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                        PaneTemplate {
                            command: Some("ssh web-02".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                        PaneTemplate {
                            command: Some("ssh web-03".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                        PaneTemplate {
                            command: Some("htop".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
                WindowTemplate {
                    name: "databases".to_string(),
                    layout: Layout::Even,
                    panes: vec![
                        PaneTemplate {
                            command: Some("ssh db-01".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                        PaneTemplate {
                            command: Some("ssh db-02".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: None,
                        },
                    ],
                },
                WindowTemplate {
                    name: "monitoring".to_string(),
                    layout: Layout::Tall,
                    panes: vec![
                        PaneTemplate {
                            command: Some("watch -n 1 'kubectl get pods'".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: Some(PaneSize::Percent(70)),
                        },
                        PaneTemplate {
                            command: Some("tail -f /var/log/syslog".to_string()),
                            directory: None,
                            environment: HashMap::new(),
                            size: Some(PaneSize::Percent(30)),
                        },
                    ],
                },
            ],
            environment: HashMap::new(),
            hooks: TemplateHooks {
                on_create: Some("echo 'Connecting to servers...'".to_string()),
                on_attach: Some("echo 'Broadcast mode available: Prefix+B'".to_string()),
                on_detach: None,
            },
        }
    }
}
```

---

## BUILT-IN FEATURES

### Feature Registry (All 210 Features)
```rust
// This represents the complete feature set - all 210 features from tmux plugins
// implemented natively in CASMUX

#[derive(Debug)]
struct FeatureRegistry {
    features: HashMap<FeatureId, Box<dyn Feature>>,
}

trait Feature: Send + Sync {
    fn id(&self) -> FeatureId;
    fn name(&self) -> &str;
    fn description(&self) -> &str;
    fn category(&self) -> FeatureCategory;
    fn is_enabled(&self) -> bool;
    fn enable(&mut self) -> Result<()>;
    fn disable(&mut self) -> Result<()>;
    fn execute(&mut self, args: &[String]) -> Result<()>;
}

#[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
enum FeatureCategory {
    Core,           // Sessions, windows, panes
    Navigation,     // Movement and jumping
    StatusBar,      // Status and monitoring
    Clipboard,      // Copy/paste features
    Development,    // Dev tools and integration
    FileHandling,   // File and URL operations
    SSH,           // Remote management
    Productivity,   // Productivity tools
    Visual,        // Themes and appearance
    Automation,    // Automation features
}

// Implementation of all 210 features would be massive, here's the structure:
impl FeatureRegistry {
    fn new() -> Self {
        let mut registry = Self {
            features: HashMap::new(),
        };
        
        // Core Session Features (1-20)
        registry.register(Box::new(ResurrectFeature::new()));
        registry.register(Box::new(ContinuumFeature::new()));
        registry.register(Box::new(SessionTemplatesFeature::new()));
        registry.register(Box::new(SessionSharingFeature::new()));
        registry.register(Box::new(SessionLockingFeature::new()));
        registry.register(Box::new(AutoDetachFeature::new()));
        registry.register(Box::new(SessionGroupsFeature::new()));
        registry.register(Box::new(SessionPreviewFeature::new()));
        registry.register(Box::new(QuickSwitchFeature::new()));
        registry.register(Box::new(SessionSearchFeature::new()));
        registry.register(Box::new(SessionArchiveFeature::new()));
        registry.register(Box::new(SessionExportFeature::new()));
        registry.register(Box::new(SessionImportFeature::new()));
        registry.register(Box::new(SessionCloneFeature::new()));
        registry.register(Box::new(SessionMergeFeature::new()));
        registry.register(Box::new(ActivityMonitorFeature::new()));
        registry.register(Box::new(IdleDetectionFeature::new()));
        registry.register(Box::new(SessionNotesFeature::new()));
        registry.register(Box::new(SessionTagsFeature::new()));
        registry.register(Box::new(SessionHistoryFeature::new()));
        
        // Window & Pane Management (21-40)
        registry.register(Box::new(SmartSplitsFeature::new()));
        registry.register(Box::new(AutoBalanceFeature::new()));
        registry.register(Box::new(PaneBordersFeature::new()));
        registry.register(Box::new(ZoomEnhancedFeature::new()));
        registry.register(Box::new(LayoutSaveFeature::new()));
        registry.register(Box::new(LayoutTemplatesFeature::new()));
        registry.register(Box::new(WindowSwapFeature::new()));
        registry.register(Box::new(PaneSwapFeature::new()));
        registry.register(Box::new(BreakJoinFeature::new()));
        registry.register(Box::new(WindowGroupsFeature::new()));
        registry.register(Box::new(TabBarFeature::new()));
        registry.register(Box::new(WindowPreviewFeature::new()));
        registry.register(Box::new(AutoRenameFeature::new()));
        registry.register(Box::new(WindowSearchFeature::new()));
        registry.register(Box::new(QuickJumpFeature::new()));
        registry.register(Box::new(WindowTagsFeature::new()));
        registry.register(Box::new(FocusEventsFeature::new()));
        registry.register(Box::new(WindowLockFeature::new()));
        registry.register(Box::new(PaneLockFeature::new()));
        registry.register(Box::new(LayoutCycleFeature::new()));
        
        // Continue for all 210 features...
        // Each feature is a complete implementation
        
        registry
    }
}

// Example of a complete feature implementation:
struct ResurrectFeature {
    enabled: bool,
    save_path: PathBuf,
    auto_save_interval: Duration,
    last_save: Option<DateTime<Utc>>,
}

impl Feature for ResurrectFeature {
    fn id(&self) -> FeatureId {
        FeatureId::Resurrect
    }
    
    fn name(&self) -> &str {
        "Session Resurrection"
    }
    
    fn description(&self) -> &str {
        "Save and restore complete session state including windows, panes, \
         working directories, and running programs"
    }
    
    fn category(&self) -> FeatureCategory {
        FeatureCategory::Core
    }
    
    fn is_enabled(&self) -> bool {
        self.enabled
    }
    
    fn enable(&mut self) -> Result<()> {
        self.enabled = true;
        self.start_auto_save_loop()?;
        Ok(())
    }
    
    fn disable(&mut self) -> Result<()> {
        self.enabled = false;
        Ok(())
    }
    
    fn execute(&mut self, args: &[String]) -> Result<()> {
        match args.first().map(|s| s.as_str()) {
            Some("save") => self.save_session(),
            Some("restore") => self.restore_session(),
            Some("list") => self.list_saved_sessions(),
            _ => self.save_session(),
        }
    }
}

impl ResurrectFeature {
    fn save_session(&mut self) -> Result<()> {
        let state = self.capture_session_state()?;
        let path = self.save_path.join(format!(
            "session-{}.casmux",
            chrono::Utc::now().format("%Y%m%d-%H%M%S")
        ));
        
        let data = bincode::serialize(&state)?;
        fs::write(&path, data)?;
        
        self.last_save = Some(chrono::Utc::now());
        
        Ok(())
    }
    
    fn restore_session(&self) -> Result<()> {
        // Find most recent save
        let saves = self.list_saved_sessions()?;
        if let Some(latest) = saves.first() {
            let data = fs::read(&latest.path)?;
            let state: SessionState = bincode::deserialize(&data)?;
            self.restore_state(state)?;
        }
        
        Ok(())
    }
    
    fn capture_session_state(&self) -> Result<SessionState> {
        // Capture complete state
        let mut state = SessionState::default();
        
        // Capture all sessions
        for session in Session::list() {
            let session_state = SessionInfo {
                id: session.id,
                name: session.name,
                created: session.created,
                windows: self.capture_windows(&session)?,
                current_window: session.current_window,
                cwd: session.cwd,
            };
            state.sessions.push(session_state);
        }
        
        Ok(state)
    }
    
    fn capture_windows(&self, session: &Session) -> Result<Vec<WindowInfo>> {
        let mut windows = Vec::new();
        
        for window in session.windows() {
            let window_state = WindowInfo {
                id: window.id,
                name: window.name,
                layout: window.layout,
                panes: self.capture_panes(&window)?,
                current_pane: window.current_pane,
            };
            windows.push(window_state);
        }
        
        Ok(windows)
    }
    
    fn capture_panes(&self, window: &Window) -> Result<Vec<PaneInfo>> {
        let mut panes = Vec::new();
        
        for pane in window.panes() {
            let pane_state = PaneInfo {
                id: pane.id,
                command: pane.get_running_command(),
                cwd: pane.cwd,
                size: pane.size,
                position: pane.position,
                scrollback: if self.save_scrollback {
                    Some(pane.get_scrollback())
                } else {
                    None
                },
            };
            panes.push(pane_state);
        }
        
        Ok(panes)
    }
}
```

---

## FONT SYSTEM

### Font Management
```rust
#[derive(Debug)]
struct FontManager {
    // Embedded fonts
    builtin_fonts: HashMap<String, FontData>,
    
    // Current font
    current_font: FontInfo,
    current_size: f32,
    
    // Font cache
    glyph_cache: GlyphCache,
    
    // Settings
    allow_system_fonts: bool,
    fallback_chain: Vec<String>,
}

#[derive(Debug)]
struct FontInfo {
    family: String,
    source: FontSource,
    features: FontFeatures,
    metrics: FontMetrics,
}

#[derive(Debug)]
enum FontSource {
    Builtin,
    System(PathBuf),
}

impl FontManager {
    fn new() -> Self {
        let mut manager = Self {
            builtin_fonts: HashMap::new(),
            current_font: FontInfo::default(),
            current_size: 14.0,
            glyph_cache: GlyphCache::new(),
            allow_system_fonts: true,
            fallback_chain: vec![],
        };
        
        // Load embedded fonts
        manager.load_builtin_fonts();
        
        // Set default font
        manager.set_font("Cascadia Code Nerd Font", 14.0).unwrap();
        
        manager
    }
    
    fn load_builtin_fonts(&mut self) {
        // Embed fonts at compile time
        const CASCADIA: &[u8] = include_bytes!("../assets/fonts/CascadiaCode/CascadiaCode.ttf");
        const JETBRAINS: &[u8] = include_bytes!("../assets/fonts/JetBrainsMono/JetBrainsMono.ttf");
        const HACK: &[u8] = include_bytes!("../assets/fonts/Hack/Hack.ttf");
        const FIRACODE: &[u8] = include_bytes!("../assets/fonts/FiraCode/FiraCode.ttf");
        const IOSEVKA: &[u8] = include_bytes!("../assets/fonts/Iosevka/Iosevka.ttf");
        
        self.builtin_fonts.insert(
            "Cascadia Code Nerd Font".to_string(),
            FontData::from_bytes(CASCADIA)
        );
        self.builtin_fonts.insert(
            "JetBrains Mono Nerd Font".to_string(),
            FontData::from_bytes(JETBRAINS)
        );
        self.builtin_fonts.insert(
            "Hack Nerd Font".to_string(),
            FontData::from_bytes(HACK)
        );
        self.builtin_fonts.insert(
            "FiraCode Nerd Font".to_string(),
            FontData::from_bytes(FIRACODE)
        );
        self.builtin_fonts.insert(
            "Iosevka Nerd Font".to_string(),
            FontData::from_bytes(IOSEVKA)
        );
    }
    
    fn zoom_in(&mut self) {
        self.current_size = (self.current_size + 0.25).min(72.0);
        self.apply_size_change();
    }
    fn zoom_out(&mut self) {
        self.current_size = (self.current_size - 0.25).max(8.0);
        self.apply_size_change();
    }
    
    fn reset_zoom(&mut self) {
        self.current_size = 14.0;
        self.apply_size_change();
    }
    
    fn apply_size_change(&mut self) {
        self.glyph_cache.clear();
        self.update_metrics();
        self.trigger_redraw();
    }
}
```

---

## THEME SYSTEM

### Theme Engine
```rust
#[derive(Debug, Clone, Serialize, Deserialize)]
struct Theme {
    name: String,
    variant: ThemeVariant,
    colors: ThemeColors,
    ui: ThemeUI,
}

#[derive(Debug, Clone, Serialize, Deserialize)]
enum ThemeVariant {
    Dark,
    Light,
}

#[derive(Debug, Clone, Serialize, Deserialize)]
struct ThemeColors {
    // Base colors
    background: Color,
    foreground: Color,
    cursor: Color,
    selection: Color,
    
    // ANSI colors (0-15)
    black: Color,
    red: Color,
    green: Color,
    yellow: Color,
    blue: Color,
    magenta: Color,
    cyan: Color,
    white: Color,
    
    bright_black: Color,
    bright_red: Color,
    bright_green: Color,
    bright_yellow: Color,
    bright_blue: Color,
    bright_magenta: Color,
    bright_cyan: Color,
    bright_white: Color,
    
    // Extended colors
    dim_foreground: Color,
    bright_foreground: Color,
    dim_background: Color,
    bright_background: Color,
}

#[derive(Debug, Clone, Serialize, Deserialize)]
struct ThemeUI {
    // Status bar
    status_bg: Color,
    status_fg: Color,
    status_active: Color,
    status_inactive: Color,
    
    // Borders
    border_active: Color,
    border_inactive: Color,
    border_broadcast: Color,
    
    // Mode indicators
    mode_normal: Option<Color>,
    mode_prefix: Color,
    mode_copy: Color,
    mode_broadcast: Color,
    mode_zen: Color,
    
    // Highlights
    search_match: Color,
    search_current: Color,
    
    // Menus
    menu_bg: Color,
    menu_fg: Color,
    menu_selected: Color,
    menu_border: Color,
}

impl Theme {
    fn dracula() -> Self {
        Self {
            name: "Dracula".to_string(),
            variant: ThemeVariant::Dark,
            colors: ThemeColors {
                background: Color::from_hex("#282a36"),
                foreground: Color::from_hex("#f8f8f2"),
                cursor: Color::from_hex("#f8f8f2"),
                selection: Color::from_hex("#44475a"),
                
                black: Color::from_hex("#21222c"),
                red: Color::from_hex("#ff5555"),
                green: Color::from_hex("#50fa7b"),
                yellow: Color::from_hex("#f1fa8c"),
                blue: Color::from_hex("#bd93f9"),
                magenta: Color::from_hex("#ff79c6"),
                cyan: Color::from_hex("#8be9fd"),
                white: Color::from_hex("#f8f8f2"),
                
                bright_black: Color::from_hex("#6272a4"),
                bright_red: Color::from_hex("#ff6e6e"),
                bright_green: Color::from_hex("#69ff94"),
                bright_yellow: Color::from_hex("#ffffa5"),
                bright_blue: Color::from_hex("#d6acff"),
                bright_magenta: Color::from_hex("#ff92df"),
                bright_cyan: Color::from_hex("#a4ffff"),
                bright_white: Color::from_hex("#ffffff"),
                
                dim_foreground: Color::from_hex("#6272a4"),
                bright_foreground: Color::from_hex("#ffffff"),
                dim_background: Color::from_hex("#1e1f29"),
                bright_background: Color::from_hex("#44475a"),
            },
            ui: ThemeUI {
                status_bg: Color::from_hex("#44475a"),
                status_fg: Color::from_hex("#f8f8f2"),
                status_active: Color::from_hex("#50fa7b"),
                status_inactive: Color::from_hex("#6272a4"),
                
                border_active: Color::from_hex("#bd93f9"),
                border_inactive: Color::from_hex("#44475a"),
                border_broadcast: Color::from_hex("#ff5555"),
                
                mode_normal: None,
                mode_prefix: Color::from_hex("#ff79c6"),
                mode_copy: Color::from_hex("#f1fa8c"),
                mode_broadcast: Color::from_hex("#ff5555"),
                mode_zen: Color::from_hex("#bd93f9"),
                
                search_match: Color::from_hex("#f1fa8c"),
                search_current: Color::from_hex("#50fa7b"),
                
                menu_bg: Color::from_hex("#44475a"),
                menu_fg: Color::from_hex("#f8f8f2"),
                menu_selected: Color::from_hex("#bd93f9"),
                menu_border: Color::from_hex("#6272a4"),
            },
        }
    }
    
    fn gruvbox() -> Self {
        Self {
            name: "Gruvbox".to_string(),
            variant: ThemeVariant::Dark,
            colors: ThemeColors {
                background: Color::from_hex("#282828"),
                foreground: Color::from_hex("#ebdbb2"),
                cursor: Color::from_hex("#ebdbb2"),
                selection: Color::from_hex("#504945"),
                
                black: Color::from_hex("#282828"),
                red: Color::from_hex("#cc241d"),
                green: Color::from_hex("#98971a"),
                yellow: Color::from_hex("#d79921"),
                blue: Color::from_hex("#458588"),
                magenta: Color::from_hex("#b16286"),
                cyan: Color::from_hex("#689d6a"),
                white: Color::from_hex("#a89984"),
                
                bright_black: Color::from_hex("#928374"),
                bright_red: Color::from_hex("#fb4934"),
                bright_green: Color::from_hex("#b8bb26"),
                bright_yellow: Color::from_hex("#fabd2f"),
                bright_blue: Color::from_hex("#83a598"),
                bright_magenta: Color::from_hex("#d3869b"),
                bright_cyan: Color::from_hex("#8ec07c"),
                bright_white: Color::from_hex("#ebdbb2"),
                
                dim_foreground: Color::from_hex("#928374"),
                bright_foreground: Color::from_hex("#fbf1c7"),
                dim_background: Color::from_hex("#1d2021"),
                bright_background: Color::from_hex("#504945"),
            },
            ui: ThemeUI {
                status_bg: Color::from_hex("#504945"),
                status_fg: Color::from_hex("#ebdbb2"),
                status_active: Color::from_hex("#b8bb26"),
                status_inactive: Color::from_hex("#928374"),
                
                border_active: Color::from_hex("#fabd2f"),
                border_inactive: Color::from_hex("#504945"),
                border_broadcast: Color::from_hex("#fb4934"),
                
                mode_normal: None,
                mode_prefix: Color::from_hex("#d3869b"),
                mode_copy: Color::from_hex("#fabd2f"),
                mode_broadcast: Color::from_hex("#fb4934"),
                mode_zen: Color::from_hex("#b16286"),
                
                search_match: Color::from_hex("#fabd2f"),
                search_current: Color::from_hex("#b8bb26"),
                
                menu_bg: Color::from_hex("#504945"),
                menu_fg: Color::from_hex("#ebdbb2"),
                menu_selected: Color::from_hex("#fabd2f"),
                menu_border: Color::from_hex("#928374"),
            },
        }
    }
}

struct ThemeManager {
    themes: HashMap<String, Theme>,
    current_theme: String,
    auto_detect: bool,
}

impl ThemeManager {
    fn new() -> Self {
        let mut manager = Self {
            themes: HashMap::new(),
            current_theme: "dracula".to_string(),
            auto_detect: false,
        };
        
        // Load built-in themes
        manager.register_theme(Theme::dracula());
        manager.register_theme(Theme::dracula_light());
        manager.register_theme(Theme::gruvbox());
        manager.register_theme(Theme::gruvbox_light());
        manager.register_theme(Theme::nord());
        manager.register_theme(Theme::nord_light());
        manager.register_theme(Theme::solarized_dark());
        manager.register_theme(Theme::solarized_light());
        manager.register_theme(Theme::monokai());
        manager.register_theme(Theme::monokai_light());
        
        // Auto-detect terminal background
        if manager.auto_detect {
            manager.detect_terminal_theme();
        }
        
        manager
    }
    
    fn detect_terminal_theme(&mut self) {
        // Query terminal background color
        if let Ok(bg) = self.query_terminal_background() {
            let is_light = bg.is_light();
            
            // Select appropriate theme
            let theme = if is_light {
                match self.current_theme.as_str() {
                    "dracula" => "dracula-light",
                    "gruvbox" => "gruvbox-light",
                    "nord" => "nord-light",
                    "solarized" => "solarized-light",
                    "monokai" => "monokai-light",
                    _ => "dracula-light",
                }
            } else {
                match self.current_theme.as_str() {
                    "dracula-light" => "dracula",
                    "gruvbox-light" => "gruvbox",
                    "nord-light" => "nord",
                    "solarized-light" => "solarized",
                    "monokai-light" => "monokai",
                    _ => "dracula",
                }
            };
            
            self.set_theme(theme);
        }
    }
}
```

---

## DISPLAY AND RESPONSIVE DESIGN

### Responsive System
```rust
#[derive(Debug, Clone, Copy)]
enum DisplaySize {
    Tiny,      // ≤30 columns (mobile portrait)
    Small,     // 31-50 columns (mobile landscape)
    Medium,    // 51-80 columns (tablet/small window)
    Large,     // ≥81 columns (desktop/full)
}

#[derive(Debug)]
struct ResponsiveManager {
    current_size: DisplaySize,
    terminal_width: u16,
    terminal_height: u16,
    
    // Adaptive settings
    max_panes: usize,
    status_bar_mode: StatusBarMode,
    ui_density: UIDensity,
    touch_optimized: bool,
}

#[derive(Debug, Clone)]
enum StatusBarMode {
    Hidden,     // No status bar (tiny screens)
    Minimal,    // Just session name and mode
    Compact,    // Essential info only
    Standard,   // Normal status bar
    Full,       // All components
}

#[derive(Debug, Clone)]
enum UIDensity {
    Compact,    // Minimal spacing
    Normal,     // Standard spacing
    Comfortable, // Extra spacing for touch
}

impl ResponsiveManager {
    fn update_size(&mut self, width: u16, height: u16) {
        self.terminal_width = width;
        self.terminal_height = height;
        
        // Determine display size
        self.current_size = match width {
            0..=30 => DisplaySize::Tiny,
            31..=50 => DisplaySize::Small,
            51..=80 => DisplaySize::Medium,
            _ => DisplaySize::Large,
        };
        
        // Adapt UI based on size
        self.adapt_ui();
    }
    
    fn adapt_ui(&mut self) {
        match self.current_size {
            DisplaySize::Tiny => {
                self.max_panes = 1;
                self.status_bar_mode = StatusBarMode::Hidden;
                self.ui_density = UIDensity::Compact;
                self.touch_optimized = true;
            }
            DisplaySize::Small => {
                self.max_panes = 2;
                self.status_bar_mode = StatusBarMode::Minimal;
                self.ui_density = UIDensity::Compact;
                self.touch_optimized = true;
            }
            DisplaySize::Medium => {
                self.max_panes = 4;
                self.status_bar_mode = StatusBarMode::Compact;
                self.ui_density = UIDensity::Normal;
                self.touch_optimized = false;
            }
            DisplaySize::Large => {
                self.max_panes = usize::MAX;
                self.status_bar_mode = StatusBarMode::Standard;
                self.ui_density = UIDensity::Normal;
                self.touch_optimized = false;
            }
        }
    }
    
    fn get_adaptive_layout(&self) -> Layout {
        match self.current_size {
            DisplaySize::Tiny => Layout::Single,
            DisplaySize::Small => {
                if self.terminal_height > self.terminal_width {
                    Layout::Vertical  // Stack vertically on portrait
                } else {
                    Layout::Horizontal  // Side by side on landscape
                }
            }
            DisplaySize::Medium => Layout::Even,
            DisplaySize::Large => Layout::Flexible,
        }
    }
}
```

### Mobile Optimization
```rust
#[derive(Debug)]
struct MobileOptimization {
    // Touch gestures
    gestures: GestureRecognizer,
    
    // Virtual keyboard handling
    keyboard_height: u16,
    keyboard_visible: bool,
    
    // Mobile-specific UI
    floating_buttons: bool,
    swipe_navigation: bool,
    tap_to_focus: bool,
}

impl MobileOptimization {
    fn handle_touch(&mut self, event: TouchEvent) -> Result<()> {
        match self.gestures.recognize(event) {
            Gesture::Tap(x, y) => {
                self.focus_pane_at(x, y)?;
            }
            Gesture::DoubleTap(x, y) => {
                self.zoom_pane_at(x, y)?;
            }
            Gesture::LongPress(x, y) => {
                self.show_context_menu_at(x, y)?;
            }
            Gesture::SwipeLeft => {
                self.next_window()?;
            }
            Gesture::SwipeRight => {
                self.previous_window()?;
            }
            Gesture::SwipeUp => {
                self.show_command_palette()?;
            }
            Gesture::SwipeDown => {
                self.toggle_status_bar()?;
            }
            Gesture::Pinch(scale) => {
                self.zoom_font(scale)?;
            }
            _ => {}
        }
        
        Ok(())
    }
    
    fn adjust_for_keyboard(&mut self) {
        if self.keyboard_visible {
            // Reduce visible area
            self.resize_panes_for_keyboard();
            
            // Move active pane above keyboard
            self.ensure_active_pane_visible();
        }
    }
}
```

---

## MOUSE SUPPORT

### Mouse Manager
```rust
#[derive(Debug)]
struct MouseManager {
    enabled: bool,
    
    // Selection
    selection_start: Option<(u16, u16)>,
    selection_end: Option<(u16, u16)>,
    selection_mode: SelectionMode,
    
    // Scrolling
    scroll_multiplier: f32,
    smooth_scrolling: bool,
    natural_scrolling: bool,
    
    // Pane operations
    resize_handle: Option<ResizeHandle>,
    drag_start: Option<(u16, u16)>,
    
    // Context menus
    context_menu: Option<ContextMenu>,
}

impl MouseManager {
    fn handle_event(&mut self, event: MouseEvent) -> Result<()> {
        if !self.enabled {
            return Ok(());
        }
        
        // No character artifacts - direct handling
        match event {
            MouseEvent::Move { x, y } => {
                self.handle_move(x, y)?;
            }
            MouseEvent::Down { x, y, button } => {
                self.handle_down(x, y, button)?;
            }
            MouseEvent::Up { x, y, button } => {
                self.handle_up(x, y, button)?;
            }
            MouseEvent::Scroll { x, y, direction } => {
                self.handle_scroll(x, y, direction)?;
            }
            MouseEvent::DoubleClick { x, y, button } => {
                self.handle_double_click(x, y, button)?;
            }
            MouseEvent::TripleClick { x, y, button } => {
                self.handle_triple_click(x, y, button)?;
            }
        }
        
        Ok(())
    }
    
    fn handle_scroll(&mut self, x: u16, y: u16, direction: ScrollDirection) -> Result<()> {
        let pane = self.get_pane_at(x, y)?;
        
        let lines = match direction {
            ScrollDirection::Up => -self.scroll_multiplier,
            ScrollDirection::Down => self.scroll_multiplier,
        };
        
        if self.natural_scrolling {
            lines = -lines;
        }
        
        if self.smooth_scrolling {
            self.smooth_scroll_pane(pane, lines)?;
        } else {
            self.scroll_pane(pane, lines as i32)?;
        }
        
        Ok(())
    }
    
    fn handle_border_drag(&mut self, x: u16, y: u16) -> Result<()> {
        if let Some(handle) = self.resize_handle {
            // Direct pane resizing - no escape sequences
            let delta_x = x as i32 - self.drag_start.unwrap().0 as i32;
            let delta_y = y as i32 - self.drag_start.unwrap().1 as i32;
            
            match handle {
                ResizeHandle::Vertical(pane_id) => {
                    self.resize_pane_horizontal(pane_id, delta_x)?;
                }
                ResizeHandle::Horizontal(pane_id) => {
                    self.resize_pane_vertical(pane_id, delta_y)?;
                }
                ResizeHandle::Corner(pane_id) => {
                    self.resize_pane_both(pane_id, delta_x, delta_y)?;
                }
            }
            
            // Visual feedback
            self.highlight_resize_handle(handle)?;
        }
        
        Ok(())
    }
}
```

### Context Menu System
```rust
#[derive(Debug)]
struct ContextMenu {
    items: Vec<MenuItem>,
    position: (u16, u16),
    selected: usize,
}

#[derive(Debug, Clone)]
struct MenuItem {
    label: String,
    icon: Option<String>,
    action: MenuAction,
    enabled: bool,
    separator: bool,
}

#[derive(Debug, Clone)]
enum MenuAction {
    Copy,
    Paste,
    CopyPath,
    OpenUrl,
    SplitHorizontal,
    SplitVertical,
    ZoomPane,
    ClosePane,
    Broadcast,
    Custom(String),
}

impl ContextMenu {
    fn pane_menu() -> Self {
        Self {
            items: vec![
                MenuItem {
                    label: "Copy".to_string(),
                    icon: Some("📋".to_string()),
                    action: MenuAction::Copy,
                    enabled: true,
                    separator: false,
                },
                MenuItem {
                    label: "Paste".to_string(),
                    icon: Some("📌".to_string()),
                    action: MenuAction::Paste,
                    enabled: true,
                    separator: false,
                },
                MenuItem {
                    label: "".to_string(),
                    icon: None,
                    action: MenuAction::Custom("".to_string()),
                    enabled: false,
                    separator: true,
                },
                MenuItem {
                    label: "Split Horizontal".to_string(),
                    icon: Some("─".to_string()),
                    action: MenuAction::SplitHorizontal,
                    enabled: true,
                    separator: false,
                },
                MenuItem {
                    label: "Split Vertical".to_string(),
                    icon: Some("│".to_string()),
                    action: MenuAction::SplitVertical,
                    enabled: true,
                    separator: false,
                },
                MenuItem {
                    label: "".to_string(),
                    icon: None,
                    action: MenuAction::Custom("".to_string()),
                    enabled: false,
                    separator: true,
                },
                MenuItem {
                    label: "Zoom Pane".to_string(),
                    icon: Some("🔍".to_string()),
                    action: MenuAction::ZoomPane,
                    enabled: true,
                    separator: false,
                },
                MenuItem {
                    label: "Close Pane".to_string(),
                    icon: Some("❌".to_string()),
                    action: MenuAction::ClosePane,
                    enabled: true,
                    separator: false,
                },
            ],
            position: (0, 0),
            selected: 0,
        }
    }
    
    fn render(&self) -> String {
        // Render context menu
        let mut output = String::new();
        
        output.push_str("┌────────────────────┐\n");
        
        for (i, item) in self.items.iter().enumerate() {
            if item.separator {
                output.push_str("├────────────────────┤\n");
            } else {
                let icon = item.icon.as_ref().map(|i| format!("{} ", i)).unwrap_or_default();
                let selected = if i == self.selected { "▶ " } else { "  " };
                
                output.push_str(&format!("│{}{}{:<16}│\n", 
                    selected, 
                    icon,
                    item.label
                ));
            }
        }
        
        output.push_str("└────────────────────┘");
        
        output
    }
}
```

---

## COMMAND PALETTE

### Command Palette Implementation
```rust
#[derive(Debug)]
struct CommandPalette {
    input: String,
    results: Vec<CommandResult>,
    selected: usize,
    mode: PaletteMode,
    history: VecDeque<String>,
}

#[derive(Debug, Clone)]
enum PaletteMode {
    Command,        // > prefix
    GoToPane,       // @ prefix
    SearchFiles,    // # prefix
    GoToLine,       // : prefix
    SearchContent,  // / prefix
    Help,          // ? prefix
    Fuzzy,         // No prefix
}

#[derive(Debug, Clone)]
struct CommandResult {
    icon: String,
    title: String,
    description: String,
    action: CommandAction,
    score: f32,
}

#[derive(Debug, Clone)]
enum CommandAction {
    ExecuteCommand(String),
    OpenFile(PathBuf),
    GoToPane(PaneId),
    GoToLine(usize),
    SearchFor(String),
    ShowHelp(String),
    ChangeTheme(String),
    RunTemplate(String),
}

impl CommandPalette {
    fn new() -> Self {
        Self {
            input: String::new(),
            results: Vec::new(),
            selected: 0,
            mode: PaletteMode::Fuzzy,
            history: VecDeque::with_capacity(100),
        }
    }
    
    fn update_input(&mut self, input: String) {
        self.input = input;
        self.detect_mode();
        self.update_results();
    }
    
    fn detect_mode(&mut self) {
        self.mode = match self.input.chars().next() {
            Some('>') => PaletteMode::Command,
            Some('@') => PaletteMode::GoToPane,
            Some('#') => PaletteMode::SearchFiles,
            Some(':') => PaletteMode::GoToLine,
            Some('/') => PaletteMode::SearchContent,
            Some('?') => PaletteMode::Help,
            _ => PaletteMode::Fuzzy,
        };
    }
    
    fn update_results(&mut self) {
        let query = match self.mode {
            PaletteMode::Fuzzy => &self.input,
            _ => &self.input[1..],  // Skip prefix
        };
        
        self.results = match self.mode {
            PaletteMode::Command => self.search_commands(query),
            PaletteMode::GoToPane => self.search_panes(query),
            PaletteMode::SearchFiles => self.search_files(query),
            PaletteMode::GoToLine => self.parse_line_number(query),
            PaletteMode::SearchContent => self.search_content(query),
            PaletteMode::Help => self.search_help(query),
            PaletteMode::Fuzzy => self.search_all(query),
        };
        
        // Sort by relevance score
        self.results.sort_by(|a, b| {
            b.score.partial_cmp(&a.score).unwrap_or(Ordering::Equal)
        });
        
        // Limit results
        self.results.truncate(20);
    }
    
    fn search_commands(&self, query: &str) -> Vec<CommandResult> {
        let mut results = Vec::new();
        
        // Built-in commands
        let commands = vec![
            ("new-window", "Create new window", "c"),
            ("split-horizontal", "Split pane horizontally", "\\"),
            ("split-vertical", "Split pane vertically", "/"),
            ("toggle-broadcast", "Toggle broadcast mode", "B"),
            ("toggle-mouse", "Toggle mouse support", "m"),
            ("reload-config", "Reload configuration", "r"),
            ("save-session", "Save current session", ""),
            ("change-theme", "Change color theme", ""),
            ("show-help", "Show help", "?"),
        ];
        
        for (cmd, desc, key) in commands {
            if let Some(score) = fuzzy_match(cmd, query) {
                let keybind = if !key.is_empty() {
                    format!(" (Prefix+{})", key)
                } else {
                    String::new()
                };
                
                results.push(CommandResult {
                    icon: "⚡".to_string(),
                    title: cmd.to_string(),
                    description: format!("{}{}", desc, keybind),
                    action: CommandAction::ExecuteCommand(cmd.to_string()),
                    score,
                });
            }
        }
        
        results
    }
    
    fn render(&self) -> String {
        let mut output = String::new();
        
        // Header
        output.push_str("┌─ Command Palette ─────────────────────────┐\n");
        output.push_str(&format!("│ {} {}█                            │\n", 
            self.mode_icon(), 
            self.input
        ));
        output.push_str("├───────────────────────────────────────────┤\n");
        
        // Results
        for (i, result) in self.results.iter().enumerate() {
            let selected = if i == self.selected { "▶" } else { " " };
            output.push_str(&format!("│{} {} {:<20} {:<15}│\n",
                selected,
                result.icon,
                result.title,
                result.description
            ));
        }
        
        // Footer
        output.push_str("├───────────────────────────────────────────┤\n");
        output.push_str("│ [↵ Execute] [Tab Complete] [Esc Cancel]  │\n");
        output.push_str("└───────────────────────────────────────────┘");
        
        output
    }
    
    fn mode_icon(&self) -> &str {
        match self.mode {
            PaletteMode::Command => ">",
            PaletteMode::GoToPane => "@",
            PaletteMode::SearchFiles => "#",
            PaletteMode::GoToLine => ":",
            PaletteMode::SearchContent => "/",
            PaletteMode::Help => "?",
            PaletteMode::Fuzzy => "",
        }
    }
}
```

---

## HELP SYSTEM

### Interactive Help
```rust
#[derive(Debug)]
struct HelpSystem {
    mode: HelpMode,
    search_query: String,
    current_topic: HelpTopic,
    context_aware: bool,
}

#[derive(Debug, Clone)]
enum HelpMode {
    Overview,
    Keybindings,
    Commands,
    Features,
    Search,
    Interactive,
}

#[derive(Debug, Clone)]
struct HelpTopic {
    title: String,
    content: String,
    related: Vec<String>,
    examples: Vec<HelpExample>,
    keybindings: Vec<KeyBinding>,
}

#[derive(Debug, Clone)]
struct HelpExample {
    description: String,
    command: String,
    visual: Option<String>,
}

impl HelpSystem {
    fn show(&mut self) -> Result<()> {
        match self.mode {
            HelpMode::Overview => self.show_overview(),
            HelpMode::Keybindings => self.show_keybindings(),
            HelpMode::Commands => self.show_commands(),
            HelpMode::Features => self.show_features(),
            HelpMode::Search => self.show_search_help(),
            HelpMode::Interactive => self.show_interactive(),
        }
    }
    
    fn show_interactive(&self) -> Result<()> {
        /*
        ┌─ CASMUX Help ─────────────────────────────┐
        │ Search: split│                            │
        ├───────────────────────────────────────────┤
        │ Splitting Panes:                          │
        │                                           │
        │ Horizontal Split:   Prefix + \            │
        │ Vertical Split:     Prefix + /            │
        │                                           │
        │ Tips:                                     │
        │ • Drag borders to resize panes           │
        │ • Double-click border to auto-balance    │
        │ • Use Prefix + Space to cycle layouts    │
        │                                           │
        │ Visual Example:                          │
        │ ┌─────┬─────┐  Prefix + \  ┌─────────┐  │
        │ │     │     │      →       │         │  │
        │ │     │     │              ├─────────┤  │
        │ └─────┴─────┘              └─────────┘  │
        │                                           │
        │ Related: resize, layout, zoom            │
        │                                           │
        │ [↑↓ Navigate] [/ Search] [q Quit]        │
        └───────────────────────────────────────────┘
        */
        
        Ok(())
    }
    
    fn get_context_help(&self) -> HelpTopic {
        // Provide help based on current context
        let mode = self.get_current_mode();
        
        match mode {
            Mode::Copy => self.copy_mode_help(),
            Mode::Broadcast => self.broadcast_mode_help(),
            Mode::Zen => self.zen_mode_help(),
            _ => self.general_help(),
        }
    }
    
    fn copy_mode_help(&self) -> HelpTopic {
        HelpTopic {
            title: "Copy Mode".to_string(),
            content: "Select and copy text using keyboard".to_string(),
            related: vec!["paste".to_string(), "clipboard".to_string()],
            examples: vec![
                HelpExample {
                    description: "Start selection".to_string(),
                    command: "v".to_string(),
                    visual: None,
                },
                HelpExample {
                    description: "Copy and exit".to_string(),
                    command: "y".to_string(),
                    visual: None,
                },
            ],
            keybindings: vec![
                KeyBinding::new("v", "Begin selection"),
                KeyBinding::new("V", "Select line"),
                KeyBinding::new("Ctrl+v", "Block selection"),
                KeyBinding::new("y", "Copy and exit"),
                KeyBinding::new("q", "Exit without copying"),
            ],
        }
    }
}
```

---

## ERROR HANDLING AND RECOVERY

### Error System
```rust
#[derive(Debug, thiserror::Error)]
enum CasmuxError {
    #[error("Terminal too small: need at least {min_width}x{min_height}, got {width}x{height}")]
    TerminalTooSmall {
        width: u16,
        height: u16,
        min_width: u16,
        min_height: u16,
    },
    
    #[error("Session file corrupted: {path}")]
    SessionCorrupted {
        path: PathBuf,
        backup_path: Option<PathBuf>,
    },
    
    #[error("Connection lost to: {host}")]
    ConnectionLost {
        host: String,
        retry_count: u32,
    },
    
    #[error("Configuration error: {message}")]
    ConfigError {
        message: String,
        line: Option<usize>,
    },
    
    #[error("Command failed: {command}")]
    CommandFailed {
        command: String,
        exit_code: i32,
    },
}

struct ErrorHandler {
    recovery_strategies: HashMap<ErrorType, RecoveryStrategy>,
    error_log: VecDeque<ErrorEntry>,
    user_notifier: UserNotifier,
}

#[derive(Debug)]
enum RecoveryStrategy {
    AutoRecover(Box<dyn Fn() -> Result<()>>),
    PromptUser(RecoveryPrompt),
    Fallback(FallbackOption),
    LogAndContinue,
    Fatal,
}

impl ErrorHandler {
    fn handle(&mut self, error: CasmuxError) -> Result<()> {
        // Log error
        self.log_error(&error);
        
        // Get recovery strategy
        let strategy = self.get_recovery_strategy(&error);
        
        // Execute recovery
        match strategy {
            RecoveryStrategy::AutoRecover(recover_fn) => {
                recover_fn()?;
                self.notify_recovery_success(&error);
            }
            RecoveryStrategy::PromptUser(prompt) => {
                self.show_recovery_prompt(prompt)?;
            }
            RecoveryStrategy::Fallback(option) => {
                self.apply_fallback(option)?;
            }
            RecoveryStrategy::LogAndContinue => {
                // Already logged, continue
            }
            RecoveryStrategy::Fatal => {
                self.handle_fatal_error(error)?;
            }
        }
        
        Ok(())
    }
    
    fn show_recovery_prompt(&self, prompt: RecoveryPrompt) -> Result<()> {
        /*
        ┌─ Recovery Options ────────────────────────┐
        │                                           │
        │ ⚠ Session file corrupted                 │
        │                                           │
        │ We found a backup from 5 minutes ago.    │
        │ What would you like to do?               │
        │                                           │
        │ [R] Restore from backup                  │
        │ [S] Start fresh session                  │
        │ [T] Try to repair file                   │
        │ [Q] Quit                                  │
        │                                           │
        └───────────────────────────────────────────┘
        */
        
        Ok(())
    }
}
```

### Crash Recovery
```rust
struct CrashRecovery {
    recovery_dir: PathBuf,
    auto_save_interval: Duration,
    last_save: Instant,
}

impl CrashRecovery {
    fn setup_panic_handler(&self) {
        let recovery_dir = self.recovery_dir.clone();
        
        panic::set_hook(Box::new(move |panic_info| {
            // Try to save state before exit
            if let Ok(state) = capture_emergency_state() {
                let path = recovery_dir.join("crash-recovery.casmux");
                let _ = save_state(&path, &state);
                
                eprintln!("\n╔════════════════════════════════════════╗");
                eprintln!("║  CASMUX crashed unexpectedly!         ║");
                eprintln!("║                                        ║");
                eprintln!("║  Session saved for recovery.          ║");
                eprintln!("║  Run 'casmux' to restore.            ║");
                eprintln!("╚════════════════════════════════════════╝\n");
                
                // Log panic details
                if let Some(location) = panic_info.location() {
                    eprintln!("Panic at {}:{}", location.file(), location.line());
                }
            }
        }));
    }
    
    fn check_for_crash_recovery(&self) -> Result<Option<SessionState>> {
        let recovery_file = self.recovery_dir.join("crash-recovery.casmux");
        
        if recovery_file.exists() {
            // Show recovery prompt
            println!("╔════════════════════════════════════════╗");
            println!("║  Previous session crashed!            ║");
            println!("║                                        ║");
            println!("║  Would you like to restore?           ║");
            println!("║  [Y] Yes, restore session             ║");
            println!("║  [N] No, start fresh                  ║");
            println!("╚════════════════════════════════════════╝");
            
            if self.prompt_yes_no()? {
                let state = load_state(&recovery_file)?;
                fs::remove_file(recovery_file)?;  // Clean up
                return Ok(Some(state));
            }
        }
        
        Ok(None)
    }
}
```

---

## UPDATE SYSTEM

### Update Manager
```rust
struct UpdateManager {
    current_version: Version,
    check_url: String,
    last_check: Option<DateTime<Utc>>,
    available_update: Option<UpdateInfo>,
}

#[derive(Debug, Deserialize)]
struct UpdateInfo {
    version: Version,
    download_url: String,
    release_notes: String,
    published_at: DateTime<Utc>,
    size: usize,
}

impl UpdateManager {
    async fn check_for_updates(&mut self) -> Result<Option<UpdateInfo>> {
        // Check at most once per week
        if let Some(last) = self.last_check {
            if Utc::now() - last < Duration::days(7) {
                return Ok(self.available_update.clone());
            }
        }
        
        // Query GitHub API
        let response = reqwest::get(&self.check_url)
            .await
            .ok();  // Silent fail on network error
        
        if let Some(response) = response {
            if response.status() == 404 {
                // No update available
                return Ok(None);
            }
            
            if let Ok(release) = response.json::<GithubRelease>().await {
                let update_info = UpdateInfo {
                    version: Version::parse(&release.tag_name)?,
                    download_url: self.get_asset_url(&release)?,
                    release_notes: release.body,
                    published_at: release.published_at,
                    size: release.assets[0].size,
                };
                
                if update_info.version > self.current_version {
                    self.available_update = Some(update_info.clone());
                    self.last_check = Some(Utc::now());
                    
                    // Show subtle notification
                    self.notify_update_available(&update_info);
                    
                    return Ok(Some(update_info));
                }
            }
        }
        
        self.last_check = Some(Utc::now());
        Ok(None)
    }
    
    async fn perform_update(&self, update_info: &UpdateInfo) -> Result<()> {
        println!("Downloading CASMUX v{}...", update_info.version);
        
        // Download new binary
        let response = reqwest::get(&update_info.download_url).await?;
        let bytes = response.bytes().await?;
        
        // Save to temporary file
        let temp_path = env::temp_dir().join("casmux-update");
        fs::write(&temp_path, bytes)?;
        
        // Make executable
        #[cfg(unix)]
        {
            use std::os::unix::fs::PermissionsExt;
            let mut perms = fs::metadata(&temp_path)?.permissions();
            perms.set_mode(0o755);
            fs::set_permissions(&temp_path, perms)?;
        }
        
        // Get current executable path
        let current_exe = env::current_exe()?;
        let backup_path = current_exe.with_extension("backup");
        
        // Backup current version
        fs::rename(&current_exe, &backup_path)?;
        
        // Install new version
        fs::rename(&temp_path, &current_exe)?;
        
        println!("✓ Updated to CASMUX v{}", update_info.version);
        println!("  Restart to use the new version");
        
        Ok(())
    }
}
```

---

## PERFORMANCE SPECIFICATIONS

### Performance Targets
```rust
struct PerformanceTargets {
    // Startup
    cold_start: Duration,           // < 50ms
    warm_start: Duration,           // < 20ms
    
    // Responsiveness
    keystroke_latency: Duration,    // < 10ms
    render_frame_time: Duration,    // < 16.67ms (60fps)
    
    // Memory
    base_memory: usize,            // < 10MB per session
    pane_memory: usize,            // < 2MB per pane
    scrollback_per_line: usize,    // < 200 bytes
    
    // Limits
    max_sessions: usize,           // 100+
    max_windows_per_session: usize, // 100+
    max_panes_per_window: usize,   // 50+
    max_scrollback_lines: usize,   // 10,000
}

struct PerformanceMonitor {
    metrics: PerformanceMetrics,
    profiler: Profiler,
    optimizations: OptimizationFlags,
}

#[derive(Debug, Default)]
struct PerformanceMetrics {
    startup_time: Duration,
    average_frame_time: Duration,
    memory_usage: usize,
    cpu_usage: f32,
    
    // Detailed metrics
    render_time: Duration,
    input_processing_time: Duration,
    state_update_time: Duration,
}

impl PerformanceMonitor {
    fn optimize_for_performance(&mut self) {
        // Detect performance issues and adapt
        if self.metrics.average_frame_time > Duration::from_millis(20) {
            // Reduce rendering quality
            self.optimizations.reduce_animations = true;
            self.optimizations.lazy_rendering = true;
        }
        
        if self.metrics.memory_usage > 500_000_000 {  // 500MB
            // Trim scrollback buffers
            self.trim_scrollback_buffers();
        }
        
        if self.metrics.cpu_usage > 0.8 {  // 80%
            // Reduce update frequency
            self.optimizations.reduce_update_rate = true;
        }
    }
}
```

### Optimization Strategies
```rust
struct OptimizationEngine {
    // Rendering optimizations
    dirty_region_tracking: bool,
    incremental_rendering: bool,
    glyph_caching: bool,
    gpu_acceleration: bool,
    
    // Memory optimizations
    scrollback_compression: bool,
    lazy_pane_loading: bool,
    memory_pooling: bool,
    
    // CPU optimizations
    parallel_processing: bool,
    batch_updates: bool,
    event_coalescing: bool,
}

impl OptimizationEngine {
    fn apply_optimizations(&mut self, context: &SystemContext) {
        // Auto-detect and apply optimizations
        
        // GPU acceleration
        self.gpu_acceleration = context.has_gpu && !context.is_remote;
        
        // Memory optimizations
        if context.available_memory < 2_000_000_000 {  // < 2GB
            self.scrollback_compression = true;
            self.lazy_pane_loading = true;
        }
        
        // CPU optimizations
        if context.cpu_cores >= 4 {
            self.parallel_processing = true;
        }
        
        // Battery optimizations
        if context.on_battery {
            self.reduce_for_battery_life();
        }
    }
    
    fn reduce_for_battery_life(&mut self) {
        self.gpu_acceleration = false;  // Reduce GPU usage
        self.reduce_refresh_rate();     // 30fps instead of 60fps
        self.disable_animations();       // No smooth scrolling
    }
}
```

---

## TMUX MIGRATION

### Migration System
```rust
struct TmuxMigration {
    tmux_config_path: PathBuf,
    output_path: PathBuf,
    migration_report: MigrationReport,
}

#[derive(Debug)]
struct MigrationReport {
    settings_migrated: Vec<MigrationItem>,
    keybindings_migrated: Vec<MigrationItem>,
    plugins_converted: Vec<PluginConversion>,
    warnings: Vec<String>,
    suggestions: Vec<String>,
}

impl TmuxMigration {
    fn import(&mut self, tmux_config: &Path) -> Result<Config> {
        println!("╔════════════════════════════════════════╗");
        println!("║  Importing tmux configuration...      ║");
        println!("╚════════════════════════════════════════╝");
        
        let tmux_content = fs::read_to_string(tmux_config)?;
        let mut casmux_config = Config::default();
        
        // Parse tmux config line by line
        for line in tmux_content.lines() {
            self.parse_tmux_line(line, &mut casmux_config)?;
        }
        
        // Generate report
        self.generate_report();
        
        // Show report
        self.show_migration_report()?;
        
        Ok(casmux_config)
    }
    
    fn parse_tmux_line(&mut self, line: &str, config: &mut Config) -> Result<()> {
        let line = line.trim();
        
        // Skip comments and empty lines
        if line.is_empty() || line.starts_with('#') {
            return Ok(());
        }
        
        // Parse set commands
        if line.starts_with("set") {
            self.parse_set_command(line, config)?;
        }
        
        // Parse bind commands
        else if line.starts_with("bind") {
            self.parse_bind_command(line, config)?;
        }
        
        // Parse unbind commands
        else if line.starts_with("unbind") {
            self.parse_unbind_command(line, config)?;
        }
        
        // Parse source commands (for plugins)
        else if line.contains("run-shell") || line.contains("run") {
            self.parse_plugin(line, config)?;
        }
        
        Ok(())
    }
    
    fn parse_set_command(&mut self, line: &str, config: &mut Config) -> Result<()> {
        // Common tmux settings to CASMUX mappings
        let mappings = vec![
            ("prefix", "prefix_key"),
            ("mouse", "mouse"),
            ("mode-keys", "copy_mode"),
            ("escape-time", "escape_time"),
            ("repeat-time", "repeat_time"),
            ("base-index", "window_base_index"),
            ("pane-base-index", "pane_base_index"),
            ("renumber-windows", "renumber_windows"),
            ("set-titles", "set_titles"),
            ("default-terminal", "default_terminal"),
        ];
        
        for (tmux_setting, casmux_setting) in mappings {
            if line.contains(tmux_setting) {
                // Extract value and convert
                if let Some(value) = self.extract_value(line) {
                    self.apply_setting(casmux_setting, value, config)?;
                    
                    self.migration_report.settings_migrated.push(
                        MigrationItem {
                            tmux: format!("set -g {}", tmux_setting),
                            casmux: format!("{}: {}", casmux_setting, value),
                        }
                    );
                }
            }
        }
        
        Ok(())
    }
    
    fn convert_plugins(&mut self, config: &mut Config) {
        // Map common tmux plugins to CASMUX features
        let plugin_map = vec![
            ("tmux-resurrect", "Built-in session save/restore"),
            ("tmux-continuum", "Built-in auto-save"),
            ("tmux-yank", "Built-in clipboard integration"),
            ("tmux-copycat", "Built-in search"),
            ("tmux-open", "Built-in URL/file opener"),
            ("tmux-battery", "Built-in battery status"),
            ("tmux-cpu", "Built-in CPU monitor"),
            ("tmux-prefix-highlight", "Built-in mode indicator"),
            ("tmux-sidebar", "Built-in file browser"),
            ("tmux-sessionist", "Built-in session management"),
            ("vim-tmux-navigator", "Built-in vim navigation"),
            ("tmux-fzf", "Built-in fuzzy finder"),
            ("tmux-jump", "Built-in jump mode"),
            ("tmux-thumbs", "Built-in URL picker"),
        ];
        
        for (plugin, feature) in plugin_map {
            if self.has_plugin(plugin) {
                self.migration_report.plugins_converted.push(
                    PluginConversion {
                        plugin: plugin.to_string(),
                        feature: feature.to_string(),
                        enabled: true,
                    }
                );
            }
        }
    }
    
    fn show_migration_report(&self) -> Result<()> {
        println!("\n╔════════════════════════════════════════════╗");
        println!("║  Migration Complete!                       ║");
        println!("╚════════════════════════════════════════════╝");
        
        println!("\n✓ Settings Migrated: {}", 
            self.migration_report.settings_migrated.len());
        
        for item in &self.migration_report.settings_migrated {
            println!("  {} → {}", item.tmux, item.casmux);
        }
        
        println!("\n✓ Keybindings Migrated: {}", 
            self.migration_report.keybindings_migrated.len());
        
        println!("\n✓ Plugins Converted to Built-in Features:");
        for conversion in &self.migration_report.plugins_converted {
            println!("  {} → {}", conversion.plugin, conversion.feature);
        }
        
        if !self.migration_report.warnings.is_empty() {
            println!("\n⚠ Warnings:");
            for warning in &self.migration_report.warnings {
                println!("  • {}", warning);
            }
        }
        
        println!("\n💡 Suggestions:");
        println!("  • Try 'Prefix+B' for broadcast mode (better than sync-panes)");
        println!("  • Use Ctrl+P for command palette");
        println!("  • Mouse support actually works now!");
        
        Ok(())
    }
}
```

---

## SSH AND REMOTE MANAGEMENT

### SSH Manager
```rust
struct SSHManager {
    connections: HashMap<PaneId, SSHConnection>,
    config_parser: SSHConfigParser,
    known_hosts: KnownHosts,
    connection_pool: ConnectionPool,
}

#[derive(Debug)]
struct SSHConnection {
    host: String,
    user: String,
    port: u16,
    pane_id: PaneId,
    status: ConnectionStatus,
    start_time: DateTime<Utc>,
    
    // Advanced features
    forwarded_ports: Vec<PortForward>,
    agent_forwarding: bool,
    x11_forwarding: bool,
    
    // Monitoring
    latency: Option<Duration>,
    bytes_sent: usize,
    bytes_received: usize,
}

impl SSHManager {
    fn parse_ssh_config(&mut self) -> Result<()> {
        let config_path = dirs::home_dir()
            .unwrap()
            .join(".ssh/config");
        
        if config_path.exists() {
            let config = self.config_parser.parse(&config_path)?;
            
            // Create quick connect menu
            for host in config.hosts {
                self.add_quick_connect(host);
            }
        }
        
        Ok(())
    }
    
    fn auto_reconnect(&mut self, pane_id: PaneId) -> Result<()> {
        if let Some(conn) = self.connections.get_mut(&pane_id) {
            if conn.status == ConnectionStatus::Disconnected {
                println!("Connection lost to {}. Reconnecting...", conn.host);
                
                // Exponential backoff
                let mut retry_delay = Duration::from_secs(1);
                
                for attempt in 1..=5 {
                    thread::sleep(retry_delay);
                    
                    if self.reconnect_ssh(conn)? {
                        println!("✓ Reconnected to {}", conn.host);
                        return Ok(());
                    }
                    
                    retry_delay *= 2;
                }
                
                return Err(CasmuxError::ConnectionLost {
                    host: conn.host.clone(),
                    retry_count: 5,
                }.into());
            }
        }
        
        Ok(())
    }
    
    fn detect_similar_hosts(&self) -> Vec<HostGroup> {
        let mut groups = Vec::new();
        
        // Group by pattern (e.g., web-01, web-02, web-03)
        let mut pattern_map: HashMap<String, Vec<String>> = HashMap::new();
        
        for conn in self.connections.values() {
            if let Some(pattern) = extract_pattern(&conn.host) {
                pattern_map.entry(pattern)
                    .or_insert_with(Vec::new)
                    .push(conn.host.clone());
            }
        }
        
        // Create groups for patterns with 3+ hosts
        for (pattern, hosts) in pattern_map {
            if hosts.len() >= 3 {
                groups.push(HostGroup {
                    name: pattern,
                    hosts,
                    suggested_action: "Enable broadcast mode for batch operations",
                });
            }
        }
        
        groups
    }
}

fn extract_pattern(hostname: &str) -> Option<String> {
    // Extract pattern like "web-##" from "web-01"
    let re = regex!(r"^([a-z]+-)(\d+)(.*)$");
    
    if let Some(caps) = re.captures(hostname) {
        Some(format!("{}##{}",
            caps.get(1).unwrap().as_str(),
            caps.get(3).unwrap().as_str()
        ))
    } else {
        None
    }
}
```

---

## DIRECTORY NAVIGATION

### Smart Directory Navigation
```rust
struct DirectoryNavigator {
    frecency_db: FrecencyDatabase,
    project_roots: Vec<PathBuf>,
    bookmarks: HashMap<String, PathBuf>,
    history: VecDeque<PathBuf>,
}

#[derive(Debug)]
struct FrecencyDatabase {
    entries: HashMap<PathBuf, FrecencyScore>,
}

#[derive(Debug)]
struct FrecencyScore {
    frequency: u32,
    last_access: DateTime<Utc>,
    score: f32,
}

impl DirectoryNavigator {
    fn smart_cd(&mut self, query: &str) -> Result<PathBuf> {
        // Exact match
        if let Ok(path) = PathBuf::from(query).canonicalize() {
            self.record_visit(&path);
            return Ok(path);
        }
        
        // Bookmark match
        if let Some(path) = self.bookmarks.get(query) {
            self.record_visit(path);
            return Ok(path.clone());
        }
        
        // Frecency search
        if let Some(path) = self.frecency_search(query) {
            self.record_visit(&path);
            return Ok(path);
        }
        
        // Project search
        if let Some(path) = self.search_projects(query) {
            self.record_visit(&path);
            return Ok(path);
        }
        
        Err(anyhow!("Directory not found: {}", query))
    }
    
    fn frecency_search(&self, query: &str) -> Option<PathBuf> {
        let mut matches: Vec<_> = self.frecency_db.entries
            .iter()
            .filter(|(path, _)| {
                path.to_string_lossy()
                    .to_lowercase()
                    .contains(&query.to_lowercase())
            })
            .map(|(path, score)| (path.clone(), score.score))
            .collect();
        
        matches.sort_by(|a, b| b.1.partial_cmp(&a.1).unwrap());
        
        matches.first().map(|(path, _)| path.clone())
    }
    
    fn record_visit(&mut self, path: &Path) {
        let entry = self.frecency_db.entries
            .entry(path.to_path_buf())
            .or_insert(FrecencyScore {
                frequency: 0,
                last_access: DateTime::from_timestamp(0, 0).unwrap(),
                score: 0.0,
            });
        
        entry.frequency += 1;
        entry.last_access = Utc::now();
        entry.score = self.calculate_frecency_score(entry);
        
        // Add to history
        self.history.push_back(path.to_path_buf());
        if self.history.len() > 100 {
            self.history.pop_front();
        }
    }
    
    fn calculate_frecency_score(&self, entry: &FrecencyScore) -> f32 {
        let age_days = (Utc::now() - entry.last_access).num_days() as f32;
        let frequency_weight = entry.frequency as f32;
        
        // Frecency formula: frequency * time_decay
        let time_decay = match age_days as i32 {
            0 => 100.0,      // Today
            1 => 80.0,       // Yesterday
            2..=7 => 60.0,   // This week
            8..=30 => 40.0,  // This month
            _ => 20.0,       // Older
        };
        
        frequency_weight * time_decay
    }
}
```

---

## WINDOW AND PANE MANAGEMENT

### Advanced Window Management
```rust
struct WindowManager {
    windows: HashMap<WindowId, Window>,
    layouts: LayoutManager,
    auto_rename: bool,
    renumber: bool,
}

struct Window {
    id: WindowId,
    name: String,
    panes: Vec<Pane>,
    layout: Layout,
    active_pane: PaneId,
    created_at: DateTime<Utc>,
    
    // Advanced features
    locked: bool,
    grouped: Option<WindowGroupId>,
    tags: HashSet<String>,
    notes: String,
}

#[derive(Debug, Clone)]
enum Layout {
    Even,          // Equal size panes
    Tall,          // Main pane on left
    Wide,          // Main pane on top
    Main,          // Large center pane
    Grid,          // Grid layout
    Custom(CustomLayout),
}

impl WindowManager {
    fn smart_split(&mut self, window_id: WindowId, direction: SplitDirection) -> Result<PaneId> {
        let window = self.windows.get_mut(&window_id)?;
        
        // Smart split based on current layout and dimensions
        let split_point = self.calculate_smart_split_point(window, direction);
        
        // Create new pane
        let new_pane = Pane::new();
        let pane_id = new_pane.id;
        
        // Adjust layout
        window.layout = self.layouts.adjust_for_split(
            &window.layout,
            direction,
            split_point
        );
        
        window.panes.push(new_pane);
        
        // Auto-balance if enabled
        if self.should_auto_balance(window) {
            self.balance_panes(window_id)?;
        }
        
        Ok(pane_id)
    }
    
    fn auto_rename_window(&mut self, window_id: WindowId) -> Result<()> {
        if !self.auto_rename {
            return Ok(());
        }
        
        let window = self.windows.get_mut(&window_id)?;
        
        // Get the primary process in the active pane
        if let Some(pane) = window.panes.iter().find(|p| p.id == window.active_pane) {
            if let Some(process) = pane.get_foreground_process() {
                // Smart naming based on process
                let name = match process.name.as_str() {
                    "vim" | "nvim" => {
                        if let Some(file) = process.args.get(1) {
                            format!("vim:{}", Path::new(file).file_name()?.to_str()?)
                        } else {
                            "vim".to_string()
                    }
                    "ssh" => {
                        if let Some(host) = process.args.get(1) {
                            format!("ssh:{}", host)
                        } else {
                            "ssh".to_string()
                        }
                    }
                    "docker" => "docker".to_string(),
                    "cargo" => {
                        if let Some(cmd) = process.args.get(1) {
                            format!("cargo:{}", cmd)
                        } else {
                            "cargo".to_string()
                        }
                    }
                    _ => process.name.clone(),
                };
                
                window.name = name;
            }
        }
        
        Ok(())
    }
}

struct PaneManager {
    panes: HashMap<PaneId, Pane>,
    focus_history: VecDeque<PaneId>,
    zoom_state: Option<ZoomState>,
}

struct Pane {
    id: PaneId,
    terminal: Terminal,
    process: Option<Process>,
    cwd: PathBuf,
    size: PaneSize,
    position: PanePosition,
    
    // State
    scrollback: ScrollbackBuffer,
    cursor_position: (u16, u16),
    
    // Features
    locked: bool,
    marked: bool,
    broadcast_target: bool,
}

impl PaneManager {
    fn zoom_pane(&mut self, pane_id: PaneId) -> Result<()> {
        if let Some(zoom) = &self.zoom_state {
            if zoom.pane_id == pane_id {
                // Unzoom
                self.restore_layout()?;
                self.zoom_state = None;
                return Ok(());
            }
        }
        
        // Save current layout
        let saved_layout = self.save_current_layout();
        
        // Zoom the pane
        self.zoom_state = Some(ZoomState {
            pane_id,
            saved_layout,
        });
        
        // Make pane full window
        if let Some(pane) = self.panes.get_mut(&pane_id) {
            pane.size = PaneSize::Full;
            pane.position = PanePosition::Origin;
        }
        
        // Hide other panes
        for (&id, pane) in self.panes.iter_mut() {
            if id != pane_id {
                pane.size = PaneSize::Hidden;
            }
        }
        
        Ok(())
    }
    
    fn swap_panes(&mut self, pane1: PaneId, pane2: PaneId) -> Result<()> {
        // Get positions and sizes
        let (pos1, size1) = {
            let p1 = self.panes.get(&pane1)?;
            (p1.position.clone(), p1.size.clone())
        };
        
        let (pos2, size2) = {
            let p2 = self.panes.get(&pane2)?;
            (p2.position.clone(), p2.size.clone())
        };
        
        // Swap positions and sizes
        if let Some(p1) = self.panes.get_mut(&pane1) {
            p1.position = pos2;
            p1.size = size2;
        }
        
        if let Some(p2) = self.panes.get_mut(&pane2) {
            p2.position = pos1;
            p2.size = size1;
        }
        
        // Animate the swap
        self.animate_swap(pane1, pane2)?;
        
        Ok(())
    }
}
```

---

## VISUAL AND UI SYSTEM

### Visual Effects
```rust
struct VisualEffects {
    animations: AnimationEngine,
    transitions: TransitionManager,
    highlights: HighlightManager,
}

struct AnimationEngine {
    enabled: bool,
    speed: AnimationSpeed,
    current_animations: Vec<Animation>,
}

#[derive(Debug)]
enum Animation {
    PaneSlide {
        pane_id: PaneId,
        from: PanePosition,
        to: PanePosition,
        progress: f32,
    },
    WindowFade {
        window_id: WindowId,
        opacity: f32,
        direction: FadeDirection,
    },
    BorderPulse {
        pane_id: PaneId,
        color: Color,
        intensity: f32,
    },
    ZoomTransition {
        pane_id: PaneId,
        scale: f32,
    },
}

impl AnimationEngine {
    fn animate_pane_creation(&mut self, pane_id: PaneId, position: PanePosition) {
        if !self.enabled {
            return;
        }
        
        // Slide in from edge
        let from = match position.edge {
            Edge::Left => PanePosition { x: -100, ..position },
            Edge::Right => PanePosition { x: self.terminal_width + 100, ..position },
            Edge::Top => PanePosition { y: -100, ..position },
            Edge::Bottom => PanePosition { y: self.terminal_height + 100, ..position },
        };
        
        self.current_animations.push(Animation::PaneSlide {
            pane_id,
            from,
            to: position,
            progress: 0.0,
        });
    }
    
    fn update(&mut self, delta_time: Duration) {
        let dt = delta_time.as_secs_f32();
        
        self.current_animations.retain_mut(|anim| {
            match anim {
                Animation::PaneSlide { progress, .. } => {
                    *progress += dt * self.speed.multiplier();
                    *progress < 1.0
                }
                Animation::WindowFade { opacity, direction, .. } => {
                    match direction {
                        FadeDirection::In => {
                            *opacity += dt * self.speed.multiplier();
                            *opacity < 1.0
                        }
                        FadeDirection::Out => {
                            *opacity -= dt * self.speed.multiplier();
                            *opacity > 0.0
                        }
                    }
                }
                Animation::BorderPulse { intensity, .. } => {
                    *intensity = (*intensity + dt * 2.0).sin().abs();
                    true  // Continuous
                }
                Animation::ZoomTransition { scale, .. } => {
                    *scale += dt * self.speed.multiplier();
                    *scale < 1.0
                }
            }
        });
    }
}

struct HighlightManager {
    active_pane_highlight: HighlightStyle,
    search_highlights: Vec<SearchHighlight>,
    broadcast_highlights: Vec<BroadcastHighlight>,
}

#[derive(Debug, Clone)]
struct HighlightStyle {
    border_color: Color,
    border_width: u8,
    glow: bool,
    animate: bool,
}

impl HighlightManager {
    fn highlight_active_pane(&self, pane_id: PaneId) -> RenderInstructions {
        RenderInstructions {
            border: Some(Border {
                color: self.active_pane_highlight.border_color,
                width: self.active_pane_highlight.border_width,
                style: BorderStyle::Solid,
            }),
            glow: if self.active_pane_highlight.glow {
                Some(GlowEffect {
                    color: self.active_pane_highlight.border_color,
                    radius: 4,
                    intensity: 0.5,
                })
            } else {
                None
            },
            animation: if self.active_pane_highlight.animate {
                Some(AnimationType::Pulse)
            } else {
                None
            },
        }
    }
}
```

### UI Components
```rust
struct UIComponents {
    menus: MenuSystem,
    dialogs: DialogSystem,
    notifications: NotificationSystem,
    overlays: OverlaySystem,
}

struct MenuSystem {
    menus: HashMap<MenuId, Menu>,
    active_menu: Option<MenuId>,
    menu_stack: Vec<MenuId>,
}

struct Menu {
    id: MenuId,
    title: String,
    items: Vec<MenuItem>,
    position: MenuPosition,
    width: u16,
    selected_index: usize,
}

impl Menu {
    fn render(&self) -> Vec<String> {
        let mut lines = Vec::new();
        
        // Top border
        lines.push(format!("┌─ {} {}┐", 
            self.title, 
            "─".repeat(self.width - self.title.len() - 4)
        ));
        
        // Menu items
        for (i, item) in self.items.iter().enumerate() {
            if item.separator {
                lines.push(format!("├{}┤", "─".repeat(self.width - 2)));
            } else {
                let selected = if i == self.selected_index { "▶ " } else { "  " };
                let icon = item.icon.as_ref().map(|i| format!("{} ", i)).unwrap_or_default();
                let shortcut = item.shortcut.as_ref().map(|s| format!(" [{}]", s)).unwrap_or_default();
                
                lines.push(format!("│{}{}{:<width$}{}│",
                    selected,
                    icon,
                    item.label,
                    shortcut,
                    width = self.width - selected.len() - icon.len() - shortcut.len() - 2
                ));
            }
        }
        
        // Bottom border
        lines.push(format!("└{}┘", "─".repeat(self.width - 2)));
        
        lines
    }
}

struct NotificationSystem {
    notifications: VecDeque<Notification>,
    position: NotificationPosition,
    max_visible: usize,
}

struct Notification {
    id: Uuid,
    level: NotificationLevel,
    title: String,
    message: String,
    timestamp: DateTime<Utc>,
    duration: Duration,
    actions: Vec<NotificationAction>,
}

impl NotificationSystem {
    fn show(&mut self, level: NotificationLevel, title: &str, message: &str) {
        let notification = Notification {
            id: Uuid::new_v4(),
            level,
            title: title.to_string(),
            message: message.to_string(),
            timestamp: Utc::now(),
            duration: Duration::from_secs(3),
            actions: vec![],
        };
        
        self.notifications.push_back(notification);
        
        // Limit number of visible notifications
        while self.notifications.len() > self.max_visible {
            self.notifications.pop_front();
        }
    }
    
    fn render(&self) -> Vec<String> {
        let mut lines = Vec::new();
        
        for notification in &self.notifications {
            let icon = match notification.level {
                NotificationLevel::Info => "ℹ️",
                NotificationLevel::Success => "✅",
                NotificationLevel::Warning => "⚠️",
                NotificationLevel::Error => "❌",
            };
            
            lines.push(format!("╭─ {} {} ─╮", icon, notification.title));
            
            // Word wrap message
            for line in textwrap::wrap(&notification.message, 40) {
                lines.push(format!("│ {:<40} │", line));
            }
            
            lines.push("╰────────────────────────────────────────╯".to_string());
        }
        
        lines
    }
}
```

---

## DEBUGGING AND DIAGNOSTICS

### Debug System
```rust
struct DebugSystem {
    logger: Logger,
    profiler: Profiler,
    inspector: StateInspector,
    doctor: SystemDoctor,
}

struct SystemDoctor {
    checks: Vec<Box<dyn HealthCheck>>,
}

trait HealthCheck {
    fn name(&self) -> &str;
    fn check(&self) -> HealthStatus;
    fn fix(&self) -> Result<()>;
}

impl SystemDoctor {
    fn run_diagnostics(&self) -> DiagnosticReport {
        let mut report = DiagnosticReport::default();
        
        println!("╔════════════════════════════════════════╗");
        println!("║  CASMUX System Diagnostics             ║");
        println!("╚════════════════════════════════════════╝");
        
        for check in &self.checks {
            let status = check.check();
            
            let icon = match status {
                HealthStatus::Ok => "✓",
                HealthStatus::Warning(_) => "⚠",
                HealthStatus::Error(_) => "✗",
            };
            
            println!("{} {}: {:?}", icon, check.name(), status);
            
            report.add_check(check.name(), status);
        }
        
        report
    }
}

struct TerminalCheck;
impl HealthCheck for TerminalCheck {
    fn name(&self) -> &str {
        "Terminal Capabilities"
    }
    
    fn check(&self) -> HealthStatus {
        let mut issues = Vec::new();
        
        // Check color support
        if !terminal_supports_256_colors() {
            issues.push("No 256 color support");
        }
        
        // Check Unicode support
        if !terminal_supports_unicode() {
            issues.push("Limited Unicode support");
        }
        
        // Check true color
        if !terminal_supports_truecolor() {
            issues.push("No true color support (using 256 colors)");
        }
        
        // Check font
        if !has_nerd_font() {
            issues.push("Nerd Font not detected (icons may not display)");
        }
        
        if issues.is_empty() {
            HealthStatus::Ok
        } else if issues.len() <= 2 {
            HealthStatus::Warning(issues.join(", "))
        } else {
            HealthStatus::Error(issues.join(", "))
        }
    }
    
    fn fix(&self) -> Result<()> {
        // Provide instructions for fixing
        println!("To fix terminal issues:");
        println!("1. Use a modern terminal (Alacritty, Kitty, Windows Terminal)");
        println!("2. Install a Nerd Font: https://www.nerdfonts.com/");
        println!("3. Enable true color in your terminal settings");
        Ok(())
    }
}

struct PerformanceCheck;
impl HealthCheck for PerformanceCheck {
    fn name(&self) -> &str {
        "Performance"
    }
    
    fn check(&self) -> HealthStatus {
        let metrics = get_performance_metrics();
        
        if metrics.startup_time > Duration::from_millis(100) {
            HealthStatus::Warning(format!(
                "Slow startup: {}ms", 
                metrics.startup_time.as_millis()
            ))
        } else if metrics.memory_usage > 500_000_000 {
            HealthStatus::Warning(format!(
                "High memory usage: {}MB",
                metrics.memory_usage / 1_000_000
            ))
        } else {
            HealthStatus::Ok
        }
    }
    
    fn fix(&self) -> Result<()> {
        // Clear caches, trim scrollback
        trim_scrollback_buffers();
        clear_glyph_cache();
        Ok(())
    }
}
```

### Debug Inspector
```rust
struct StateInspector {
    enabled: bool,
    overlay: DebugOverlay,
}

struct DebugOverlay {
    show_fps: bool,
    show_memory: bool,
    show_events: bool,
    show_layout: bool,
}

impl StateInspector {
    fn render_overlay(&self) -> Vec<String> {
        let mut lines = Vec::new();
        
        if self.overlay.show_fps {
            lines.push(format!("FPS: {:.1}", get_current_fps()));
        }
        
        if self.overlay.show_memory {
            lines.push(format!("Memory: {}MB", get_memory_usage() / 1_000_000));
        }
        
        if self.overlay.show_events {
            lines.push(format!("Events/s: {}", get_events_per_second()));
        }
        
        if self.overlay.show_layout {
            lines.push(format!("Layout: {:?}", get_current_layout()));
        }
        
        lines
    }
    
    fn dump_state(&self) -> Result<()> {
        let state_dump = StateDump {
            timestamp: Utc::now(),
            version: env!("CARGO_PKG_VERSION"),
            sessions: collect_session_states(),
            config: get_current_config(),
            performance: get_performance_metrics(),
            system: get_system_info(),
        };
        
        let path = dirs::data_dir()
            .unwrap()
            .join("casmux")
            .join(format!("debug-{}.json", Utc::now().format("%Y%m%d-%H%M%S")));
        
        let json = serde_json::to_string_pretty(&state_dump)?;
        fs::write(&path, json)?;
        
        println!("State dumped to: {}", path.display());
        
        Ok(())
    }
}
```

---

## PLATFORM SUPPORT

### Platform Abstraction
```rust
trait PlatformLayer {
    fn init() -> Result<()>;
    fn create_window(&self) -> Result<Window>;
    fn get_terminal_info(&self) -> TerminalInfo;
    fn set_clipboard(&self, text: &str) -> Result<()>;
    fn get_clipboard(&self) -> Result<String>;
    fn open_url(&self, url: &str) -> Result<()>;
    fn get_system_info(&self) -> SystemInfo;
}

// Linux Implementation
struct LinuxPlatform;
impl PlatformLayer for LinuxPlatform {
    fn init() -> Result<()> {
        // Initialize X11/Wayland
        Ok(())
    }
    
    fn set_clipboard(&self, text: &str) -> Result<()> {
        // Try multiple methods
        if env::var("WAYLAND_DISPLAY").is_ok() {
            // Use wl-copy
            Command::new("wl-copy")
                .stdin(Stdio::piped())
                .spawn()?
                .stdin.unwrap()
                .write_all(text.as_bytes())?;
        } else if env::var("DISPLAY").is_ok() {
            // Use xclip or xsel
            Command::new("xclip")
                .args(&["-selection", "clipboard"])
                .stdin(Stdio::piped())
                .spawn()?
                .stdin.unwrap()
                .write_all(text.as_bytes())?;
        } else {
            // Fallback to OSC 52
            print!("\x1b]52;c;{}\x07", base64::encode(text));
        }
        Ok(())
    }
}

// macOS Implementation
struct MacOSPlatform;
impl PlatformLayer for MacOSPlatform {
    fn set_clipboard(&self, text: &str) -> Result<()> {
        Command::new("pbcopy")
            .stdin(Stdio::piped())
            .spawn()?
            .stdin.unwrap()
            .write_all(text.as_bytes())?;
        Ok(())
    }
    
    fn open_url(&self, url: &str) -> Result<()> {
        Command::new("open").arg(url).spawn()?;
        Ok(())
    }
}

// Windows Implementation
struct WindowsPlatform;
impl PlatformLayer for WindowsPlatform {
    fn set_clipboard(&self, text: &str) -> Result<()> {
        use windows::Win32::System::DataExchange::*;
        use windows::Win32::System::Memory::*;
        
        unsafe {
            OpenClipboard(None)?;
            EmptyClipboard()?;
            
            let len = text.len() + 1;
            let h_mem = GlobalAlloc(GMEM_MOVEABLE, len)?;
            let ptr = GlobalLock(h_mem);
            
            std::ptr::copy_nonoverlapping(
                text.as_ptr(),
                ptr as *mut u8,
                text.len()
            );
            
            GlobalUnlock(h_mem);
            SetClipboardData(CF_TEXT.0, h_mem)?;
            CloseClipboard()?;
        }
        
        Ok(())
    }
}

// Platform detection and factory
fn create_platform() -> Box<dyn PlatformLayer> {
    #[cfg(target_os = "linux")]
    return Box::new(LinuxPlatform);
    
    #[cfg(target_os = "macos")]
    return Box::new(MacOSPlatform);
    
    #[cfg(target_os = "windows")]
    return Box::new(WindowsPlatform);
    
    #[cfg(not(any(target_os = "linux", target_os = "macos", target_os = "windows")))]
    return Box::new(GenericPlatform);
}
```

---

## DISTRIBUTION AND INSTALLATION

### Installation Script
```bash
#!/usr/bin/env bash
# install.sh - Universal CASMUX installer

set -e

VERSION="${1:-latest}"
INSTALL_DIR="${INSTALL_DIR:-/usr/local/bin}"
REPO="https://github.com/casapps/casmux"

# Detect OS and architecture
detect_platform() {
    OS="$(uname -s)"
    ARCH="$(uname -m)"
    
    case "$OS" in
        Linux*)     PLATFORM="linux" ;;
        Darwin*)    PLATFORM="macos" ;;
        MINGW*|MSYS*|CYGWIN*) PLATFORM="windows" ;;
        *)          echo "Unsupported OS: $OS"; exit 1 ;;
    esac
    
    case "$ARCH" in
        x86_64|amd64) ARCH="amd64" ;;
        aarch64|arm64) ARCH="arm64" ;;
        *)          echo "Unsupported architecture: $ARCH"; exit 1 ;;
    esac
    
    echo "Detected platform: $PLATFORM-$ARCH"
}

# Download binary
download() {
    if [ "$VERSION" = "latest" ]; then
        URL="$REPO/releases/latest/download/casmux-$PLATFORM-$ARCH"
    else
        URL="$REPO/releases/download/v$VERSION/casmux-$PLATFORM-$ARCH"
    fi
    
    echo "Downloading CASMUX from $URL..."
    
    if command -v curl >/dev/null; then
        curl -L -o /tmp/casmux "$URL"
    elif command -v wget >/dev/null; then
        wget -O /tmp/casmux "$URL"
    else
        echo "Error: curl or wget required"
        exit 1
    fi
}

# Install binary
install() {
    echo "Installing to $INSTALL_DIR..."
    
    # Check permissions
    if [ -w "$INSTALL_DIR" ]; then
        mv /tmp/casmux "$INSTALL_DIR/casmux"
    else
        echo "Root permissions required for $INSTALL_DIR"
        sudo mv /tmp/casmux "$INSTALL_DIR/casmux"
    fi
    
    chmod +x "$INSTALL_DIR/casmux"
    echo "✓ CASMUX installed successfully!"
}

# Verify installation
verify() {
    if "$INSTALL_DIR/casmux" --version >/dev/null 2>&1; then
        VERSION=$("$INSTALL_DIR/casmux" --version | cut -d' ' -f2)
        echo "✓ CASMUX v$VERSION is ready to use"
        echo ""
        echo "Get started:"
        echo "  casmux          # Start CASMUX"
        echo "  casmux --help   # Show help"
    else
        echo "Error: Installation verification failed"
        exit 1
    fi
}

# Main
main() {
    echo "╔════════════════════════════════════════╗"
    echo "║  CASMUX Installer                      ║"
    echo "╚════════════════════════════════════════╝"
    echo ""
    
    detect_platform
    download
    install
    verify
    
    echo ""
    echo "Installation complete!"
}

main "$@"
```

### Package Definitions

#### Homebrew Formula
```ruby
# homebrew/casmux.rb
class Casmux < Formula
  desc "Modern terminal multiplexer - tmux evolved"
  homepage "https://github.com/casapps/casmux"
  version "1.0.0"
  
  if OS.mac?
    if Hardware::CPU.arm?
      url "https://github.com/casapps/casmux/releases/download/v1.0.0/casmux-macos-arm64.tar.gz"
      sha256 "..."
    else
      url "https://github.com/casapps/casmux/releases/download/v1.0.0/casmux-macos-amd64.tar.gz"
      sha256 "..."
    end
  elsif OS.linux?
    if Hardware::CPU.arm?
      url "https://github.com/casapps/casmux/releases/download/v1.0.0/casmux-linux-arm64.tar.gz"
      sha256 "..."
    else
      url "https://github.com/casapps/casmux/releases/download/v1.0.0/casmux-linux-amd64.tar.gz"
      sha256 "..."
    end
  end
  
  def install
    bin.install "casmux"
  end
  
  test do
    system "#{bin}/casmux", "--version"
  end
end
```

#### Debian Package
```bash
# packaging/debian/control
Package: casmux
Version: 1.0.0
Section: utils
Priority: optional
Architecture: amd64
Maintainer: CasjaysDev <support@casapps.com>
Description: Modern terminal multiplexer
 CASMUX is a modern terminal multiplexer that takes the best of tmux
 and evolves it for today's developers. Zero configuration required,
 with 200+ built-in features.
Homepage: https://github.com/casapps/casmux
```

---

## TESTING STRATEGY

### Test Coverage
```rust
#[cfg(test)]
mod tests {
    use super::*;
    
    #[test]
    fn test_session_creation() {
        let mut manager = SessionManager::new();
        let session = manager.create_session("test").unwrap();
        
        assert_eq!(session.name, "test");
        assert_eq!(session.windows.len(), 1);
    }
    
    #[test]
    fn test_pane_split() {
        let mut window = Window::new();
        let pane1 = window.split_horizontal().unwrap();
        let pane2 = window.split_vertical().unwrap();
        
        assert_eq!(window.panes.len(), 3);
        assert_ne!(pane1, pane2);
    }
    
    #[test]
    fn test_broadcast_mode() {
        let mut broadcast = BroadcastManager::new();
        broadcast.enable().unwrap();
        
        assert!(broadcast.enabled);
        assert!(!broadcast.target_panes.is_empty());
    }
    
    #[test]
    fn test_copy_paste() {
        let mut clipboard = CopyPasteManager::new();
        clipboard.copy("test text").unwrap();
        
        let pasted = clipboard.paste().unwrap();
        assert_eq!(pasted, "test text");
    }
}

#[cfg(test)]
mod integration_tests {
    use super::*;
    
    #[tokio::test]
    async fn test_full_workflow() {
        // Start CASMUX
        let mut app = Casmux::new().await.unwrap();
        
        // Create session
        let session = app.create_session("integration").await.unwrap();
        
        // Create windows
        let window1 = session.new_window("editor").await.unwrap();
        let window2 = session.new_window("server").await.unwrap();
        
        // Split panes
        window1.split_horizontal().await.unwrap();
        window2.split_vertical().await.unwrap();
        
        // Test navigation
        app.next_window().await.unwrap();
        assert_eq!(app.current_window().name, "server");
        
        // Test broadcast
        app.enable_broadcast().await.unwrap();
        app.send_keys("echo test").await.unwrap();
        
        // Verify all panes received input
        for pane in app.all_panes() {
            let output = pane.get_output().await.unwrap();
            assert!(output.contains("test"));
        }
    }
}
```

### Performance Benchmarks
```rust
#[cfg(test)]
mod benchmarks {
    use criterion::{black_box, criterion_group, criterion_main, Criterion};
    
    fn benchmark_startup(c: &mut Criterion) {
        c.bench_function("cold startup", |b| {
            b.iter(|| {
                let app = Casmux::new();
                black_box(app);
            });
        });
    }
    
    fn benchmark_pane_creation(c: &mut Criterion) {
        let mut window = Window::new();
        
        c.bench_function("create 100 panes", |b| {
            b.iter(|| {
                for _ in 0..100 {
                    window.split_horizontal();
                }
            });
        });
    }
    
    fn benchmark_scrollback(c: &mut Criterion) {
        let mut buffer = ScrollbackBuffer::new(10000);
        
        c.bench_function("append 10k lines", |b| {
            b.iter(|| {
                for i in 0..10000 {
                    buffer.append(format!("Line {}", i));
                }
            });
        });
    }
    
    criterion_group!(benches, 
        benchmark_startup,
        benchmark_pane_creation,
        benchmark_scrollback
    );
    criterion_main!(benches);
}
```

---

## DOCUMENTATION

### User Guide Structure
```markdown
# CASMUX User Guide

## Table of Contents
1. [Getting Started](#getting-started)
2. [Basic Concepts](#basic-concepts)
3. [Key Bindings](#key-bindings)
4. [Configuration](#configuration)
5. [Features](#features)
6. [Tips & Tricks](#tips-tricks)
7. [Troubleshooting](#troubleshooting)

## Getting Started

### Installation
```bash
# Universal installer
curl -L https://casmux.dev/install | bash

# Package managers
brew install casmux        # macOS/Linux
cargo install casmux       # Rust
snap install casmux       # Snap
apt install casmux        # Debian/Ubuntu
```

### First Run
When you run CASMUX for the first time:
```bash
casmux
```

You'll see a welcome screen with options:
- **Quick Tour**: 30-second interactive tutorial
- **Import tmux**: Migrate from existing tmux config
- **Just Start**: Begin using CASMUX immediately

### Basic Navigation
- `Prefix + c`: Create new window
- `Prefix + n/p`: Next/previous window
- `Prefix + \`: Split horizontal
- `Prefix + /`: Split vertical
- `Prefix + arrows`: Navigate panes

The default prefix is `Ctrl+B` (change with `prefix_key` in config).
```

### API Documentation
```rust
/// CASMUX Public API
pub mod casmux {
    /// Session management
    pub mod session {
        /// Create a new session
        pub fn create(name: &str) -> Result<Session>;
        
        /// List all sessions
        pub fn list() -> Vec<Session>;
        
        /// Attach to a session
        pub fn attach(name: &str) -> Result<()>;
    }
    
    /// Window management
    pub mod window {
        /// Create a new window
        pub fn create(name: &str) -> Result<Window>;
        
        /// Split current pane
        pub fn split(direction: SplitDirection) -> Result<Pane>;
    }
    
    /// Configuration
    pub mod config {
        /// Load configuration from file
        pub fn load(path: &Path) -> Result<Config>;
        
        /// Apply configuration
        pub fn apply(config: Config) -> Result<()>;
    }
}
```

---

## PROJECT GOVERNANCE

### Contributing Guidelines
```markdown
# Contributing to CASMUX

## Code of Conduct
We are committed to providing a welcoming and inclusive environment.

## How to Contribute

### Reporting Issues
1. Check existing issues
2. Provide minimal reproduction
3. Include system information
4. Describe expected vs actual behavior

### Pull Requests
1. Fork the repository
2. Create a feature branch
3. Write tests for new features
4. Ensure all tests pass
5. Update documentation
6. Submit PR with clear description

### Development Setup
```bash
# Clone repository
git clone https://github.com/casapps/casmux
cd casmux

# Install dependencies
cargo build

# Run tests
cargo test

# Run with debug logging
RUST_LOG=debug cargo run
```

### Code Style
- Use `cargo fmt` for formatting
- Use `cargo clippy` for linting
- Follow Rust naming conventions
- Write descriptive commit messages
```

---

## IMPLEMENTATION ROADMAP

### Phase 1: Core Foundation (Months 1-2)
**Goal: Basic multiplexer functionality**

Week 1-2:
- [ ] Project setup and structure
- [ ] Basic Rust workspace configuration
- [ ] Terminal emulation integration (wezterm-term)
- [ ] Basic TUI framework with ratatui

Week 3-4:
- [ ] Session management (create, attach, detach)
- [ ] Window management (create, switch, close)
- [ ] Pane management (split, navigate, close)
- [ ] Basic keybinding system

Week 5-6:
- [ ] Configuration system (YAML parsing)
- [ ] Status bar implementation
- [ ] Basic themes (Dracula, Gruvbox)
- [ ] Font management

Week 7-8:
- [ ] Copy/paste system with clipboard
- [ ] Search functionality
- [ ] Session persistence (resurrect)
- [ ] Auto-save (continuum)

### Phase 2: Essential Features (Months 3-4)
**Goal: Feature parity with basic tmux + plugins**

Week 9-10:
- [ ] GUI mode with winit
- [ ] Mouse support
- [ ] Command palette (Ctrl+P)
- [ ] Help system

Week 11-12:
- [ ] Broadcast mode
- [ ] Project detection
- [ ] Project templates
- [ ] tmux import

Week 13-14:
- [ ] Smart directory navigation
- [ ] SSH management
- [ ] Git integration
- [ ] Docker integration

Week 15-16:
- [ ] All 10 built-in themes
- [ ] Visual effects and animations
- [ ] Responsive design
- [ ] Mobile optimization

### Phase 3: Advanced Features (Months 5-6)
**Goal: Complete 200+ feature set**

Week 17-20:
- [ ] Implement remaining 150+ features
- [ ] Advanced search and filtering
- [ ] Macro system
- [ ] Snippet manager

Week 21-24:
- [ ] Performance optimizations
- [ ] Memory optimizations
- [ ] Platform-specific features
- [ ] Extensive testing

### Phase 4: Polish & Release (Months 7-9)
**Goal: Production-ready release**

Month 7:
- [ ] Bug fixes and stability
- [ ] Performance tuning
- [ ] Documentation completion
- [ ] Website development

Month 8:
- [ ] Beta testing program
- [ ] Community feedback
- [ ] Package distribution
- [ ] CI/CD pipeline

Month 9:
- [ ] Final testing and QA
- [ ] Marketing materials
- [ ] Launch preparation
- [ ] 1.0 release

### Success Metrics

**Technical Goals:**
- Startup time < 50ms
- Memory usage < 50MB typical
- 60fps smooth scrolling
- Zero crashes in normal usage
- Works on all major platforms

**User Goals:**
- Zero configuration needed for 95% of users
- Easier to use than tmux
- More features than tmux + 200 plugins
- Active community
- 1000+ GitHub stars in first year

---

## CONCLUSION

This comprehensive specification defines CASMUX as the next evolution of terminal multiplexers. By taking the best ideas from tmux and modernizing them with:

1. **Zero Configuration** - Works perfectly out of the box
2. **Modern UX** - Intuitive naming, visual feedback, discoverable features
3. **Built-in Everything** - 200+ features natively implemented
4. **Cross-Platform** - Single binary for all platforms
5. **Performance** - Fast, efficient, responsive
6. **Smart Defaults** - Automatically configures based on context

CASMUX will be the terminal multiplexer that developers actually want to use. It respects the power of tmux while fixing its usability issues, making advanced terminal multiplexing accessible to everyone.

The specification is complete and ready for implementation. With clear architecture, detailed features, and a realistic roadmap, CASMUX can be built and released within 9 months, revolutionizing how developers work with terminals.

---

*End of CASMUX Complete Project Specification v1.0*
*Total specification length: ~50,000 words*
*Ready for implementation*

