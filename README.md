[![Claude Code Index](https://img.shields.io/badge/Claude%20Code-Repos%20Index-blue?style=flat-square&logo=github)](https://github.com/danielrosehill/Claude-Code-Repos-Index)

# Claude OS Playbook

A GitHub template for provisioning development environments using Claude Code slash commands and subagents instead of Ansible.

## Why Not Ansible?

Ansible playbooks are powerful but brittle. Package names change between distros, PPAs go stale, shell scripts break silently, and maintaining idempotent YAML across Ubuntu versions is a constant tax. This project takes a different approach: describe your desired environment in a human-readable profile, then let Claude Code provision it conversationally, adapting to whatever it finds on the target system.

## How It Works

```
/setup                      # Entrypoint: reads your profile and orchestrates everything
/setup-category languages   # Provision just one category
/verify                     # Check what's installed vs. what's expected
/export                     # Snapshot your current environment into a new profile
```

The `/setup` command reads a profile YAML from `profiles/`, then delegates to specialized subagents for each category:

| Agent | Responsibility |
|-------|---------------|
| `system-packages` | apt, snap, flatpak packages |
| `languages` | Python (uv), Node (nvm), Go, Rust, Java (SDKMAN) |
| `containers` | Docker, Docker Compose, container runtimes |
| `cli-tools` | uv tools, npm globals, pip packages, cargo installs |
| `shell-config` | Shell dotfiles, aliases, environment variables |
| `claude-setup` | Claude Code commands, agents, MCP profiles via MCPM |
| `desktop-apps` | GUI applications via snap, flatpak, or apt |
| `services` | systemd services, VPN, background daemons |

Each agent checks what is already present before installing anything, reports what it changed, and skips items that are already satisfied.

## Quick Start

1. **Use this template** to create your own repo
2. Copy `profiles/example.yaml` to `profiles/my-machine.yaml`
3. Edit it to match your desired environment
4. Open the repo in Claude Code and run `/setup`

## Customization

This template ships with an example profile based on the author's Ubuntu workstation. You will want to:

- Replace `profiles/example.yaml` with your own tool list
- Adjust the agents in `.claude/agents/` if you use different package managers or OS
- Add or remove categories to match your workflow
- Modify `.claude/commands/setup.md` if you want a different orchestration flow

The slash commands and agents are plain Markdown. Read them, edit them, make them yours.

## Profile Format

Profiles are YAML files in `profiles/`. See `profiles/example.yaml` for the full schema. Each profile declares:

- **system_packages**: apt, snap, and flatpak packages
- **languages**: Runtime versions and install methods
- **cli_tools**: Tools installed via uv, npm, cargo, or go
- **containers**: Docker and related tooling
- **shell**: Dotfile snippets, aliases, environment variables
- **claude**: Slash commands repos, agent definitions, MCP profiles
- **desktop_apps**: GUI applications and their install method
- **services**: systemd units and background processes

## Requirements

- Ubuntu or Debian-based Linux (adaptable to other distros by modifying agents)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated
- Basic shell access with sudo

## License

MIT

---

For more Claude Code projects, visit my [Claude Code Repos Index](https://github.com/danielrosehill/Claude-Code-Repos-Index).
