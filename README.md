# 💻 Complete Workstation Architecture

Welcome to my personal dotfiles and infrastructure repository. This workstation is an engineered, production-ready Linux development environment built on **Fedora Workstation**. It is architected for full-stack web applications, DevOps pipelines, infrastructure-as-code automation, and hardware-accelerated local AI pair-programming.

---

## 📂 System Directory Layout & File Structure

To keep the operating system clean and prevent configuration bloat, the workstation is meticulously organized using explicit, predictable pathing:

```text
/home/divyansh/
├── .zshrc                           # Active shell configuration, aliases, and runtime paths
├── .config/
│   ├── kitty/kitty.conf             # GPU-accelerated terminal styling and fonts
│   └── starship.toml                # Prompt customization configurations
│
└── Development/                     # The Unified Engineering Hub
    ├── Projects/                    # Active code repositories and application builds
    │   └── dotfiles/                # Local Git mirror tracking system configuration files
    │       ├── kitty/kitty.conf     # Backup copy of terminal configs
    │       ├── starship/starship.toml # Backup copy of prompt styling
    │       ├── .zshrc               # Backup copy of shell aliases
    │       └── README.md            # Comprehensive architecture documentation
    │
    ├── Databases/                   # Containerized local data layer
    │   └── docker-compose.yml       # Production-grade isolated database stack
    │
    └── DevOps/                      # Declarative infrastructure sandboxes
        └── test-node/               # Isolated node environment for virtual machines
            └── Vagrantfile          # Automated virtual box infrastructure configuration
```

---

## 🛠️ Multi-Layered Systems Architecture

This workstation rejects global binary pollution. The system is split into distinct functional planes, ensuring runtimes, databases, and client applications never cross-contaminate the host OS.

### 1. Host Operating System & Shell Plane

- **Base:** Fedora Workstation + RPM Fusion (non-free codecs, proprietary kernel modules)
- **Shell:** Zsh + async Starship prompt (Git branch detection, container tracking, sub-second rendering)
- **Modern CLI tools:**
  - `eza` — visual directory tree with file metadata and icons
  - `bat` — file reader with syntax highlighting and pagination
  - `zoxide` — frecency-based smart `cd`
  - `tmux` — persistent, decoupled terminal session manager
- **Consumer apps:** Flatpak/Flathub sandboxes (Discord, Telegram, Spotify, Obsidian, VLC, OBS, GIMP)

### 2. Sandbox Runtime Plane — Zero Global Host Pollution

| Ecosystem | Manager | Purpose |
|---|---|---|
| Node.js | `nvm` | Per-repo runtime switching |
| JVM | `sdkman` | Isolated openJDK targets |
| Rust | `rustup` | Toolchain + target management |
| Go | Official toolchain | Native binary compilation |

### 3. Containerized Persistence Layer

No database runs as a native system service. All orchestrated via Docker Compose on a closed internal network:

| Service | Image | Port |
|---|---|---|
| PostgreSQL 16 | alpine | 5432 |
| Redis 7 | alpine | 6379 |
| MongoDB 7.0 | official | 27017 |

Data persists in named Docker volumes (`pgdata`, `redisdata`, `mongodata`) — survives container restarts and rebuilds.

### 4. Infrastructure as Code & DevOps Pipeline

- **Terraform** — declarative cloud infra blueprints
- **AWS CLI** — full cloud automation interface
- **Vagrant** (`ubuntu/jammy64`) — local VM sandbox for provisioning and deployment testing
- **pre-commit hooks:** `shellcheck`, `shfmt`, `markdownlint-cli2` — enforced before every commit

### 5. Local Hardware-Accelerated AI

- **Ollama** — local model inference bound to NVIDIA GPU via CUDA runtime
- **Aider** — terminal AI pair programmer, operates on local Git context only, no code leaves the machine

---

## 🚀 Daily Developer Workflow

```bash
# 1. Jump to projects instantly
z ~/Development/Projects

# 2. Boot the containerized DB stack
docker compose -f ~/Development/Databases/docker-compose.yml up -d

# 3. Verify all containers are healthy
docker ps

# 4. Pin Node version for the project
nvm use 22

# 5. Launch local AI pair-programming session
aider --model ollama/qwen2.5-coder:7b
```

---

## ⚙️ Automated Maintenance — `sys-update`

One command to update the entire system:

```bash
sys-update() {
    echo "→ Fedora system packages"
    sudo dnf upgrade --refresh -y

    echo "→ Flatpak apps"
    flatpak update -y

    echo "→ Rust toolchain"
    rustup update

    echo "→ Docker image prune"
    docker image prune -f

    echo "✓ Workstation up to date"
}
```

Add to `~/.zshrc` and reload: `source ~/.zshrc`

---

## 🖥️ Fresh Machine Bootstrap

```bash
# Clone dotfiles
git clone https://github.com/divyansh-v15-06/my-fedora-configs ~/Development/Projects/dotfiles
cd ~/Development/Projects/dotfiles

# Symlink configs
ln -sf $(pwd)/kitty/kitty.conf ~/.config/kitty/kitty.conf
ln -sf $(pwd)/starship/starship.toml ~/.config/starship.toml
ln -sf $(pwd)/.zshrc ~/.zshrc

# Install modern CLI tools
sudo dnf install -y eza bat tmux
cargo install zoxide

# Start DB stack
docker compose -f ~/Development/Databases/docker-compose.yml up -d
```

---

## 🔒 Cache & Isolation Rules

1. **Project-scoped caches** — `node_modules`, `.venv`, `target/` stay inside project dirs, never global
2. **Clean config mirroring** — this repo tracks only config files; no build artifacts, no `.env` leaks
3. **Volume-backed persistence** — DB data lives in Docker volumes, isolated from host filesystem

---

*Fedora · Zsh · Kitty · KDE Plasma · Docker · Rust · Go · Node.js · Terraform · Ollama*
