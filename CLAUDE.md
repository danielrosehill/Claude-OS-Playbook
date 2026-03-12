# Claude OS Playbook

This repository is a GitHub template for provisioning development environments using Claude Code slash commands and subagents.

## Structure

- `profiles/` — YAML environment profiles defining what to install
- `.claude/commands/` — Slash commands (entrypoints and utilities)
- `.claude/agents/` — Subagents for each provisioning category

## Key Commands

- `/setup` — Main entrypoint. Reads a profile and orchestrates all agents.
- `/setup-category` — Run a single category (e.g., `/setup-category languages`)
- `/verify` — Audit current system against a profile
- `/export` — Snapshot the current environment into a new profile YAML

## Conventions

- Always check if something is already installed before attempting to install it
- Never run `rm -rf`, `dd`, or other destructive commands without explicit user confirmation
- Use `sudo` only when necessary (package installs, service management)
- Prefer the OS-native package manager (apt) but respect the profile's declared install method
- Log what was changed so the user can review after provisioning
- When a package name or install method fails, adapt rather than abort. Ask the user or try alternatives.

## Profile Resolution

1. If the user specifies a profile name with `/setup <name>`, load `profiles/<name>.yaml`
2. If no name is given, look for `profiles/default.yaml`
3. If that doesn't exist, list available profiles and ask the user to choose
