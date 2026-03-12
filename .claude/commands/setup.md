Provision a development environment from a profile.

This is the main entrypoint for the Claude OS Playbook. It reads a YAML profile and delegates installation to specialized subagents.

## Instructions

1. **Resolve the profile.** If `$ARGUMENTS` is provided, load `profiles/$ARGUMENTS.yaml`. Otherwise, look for `profiles/default.yaml`. If neither exists, list available profiles in `profiles/` and ask the user to choose.

2. **Read and parse the profile YAML.** Identify which categories are defined (system_packages, languages, cli_tools, containers, shell, claude, desktop_apps, services).

3. **Show the user a summary** of what will be provisioned, organized by category, with estimated item counts. Ask for confirmation before proceeding.

4. **Run the OS detection check.** Confirm the target OS matches the profile's assumptions (e.g., Ubuntu/Debian for apt). Warn if there's a mismatch.

5. **Delegate to subagents in this order** (skip any category not present in the profile):
   - `system-packages` — apt, snap, flatpak installs
   - `languages` — Programming language runtimes
   - `containers` — Docker and container tooling
   - `cli-tools` — uv tools, npm globals, pip packages, cargo/go installs
   - `shell-config` — Dotfiles, aliases, environment variables
   - `desktop-apps` — GUI applications
   - `services` — systemd services and daemons
   - `claude-setup` — Claude Code commands, agents, and MCP configuration

   For each agent, pass the relevant section of the profile YAML as context.

6. **Collect results from each agent.** Each agent returns a summary of what it installed, skipped (already present), and failed.

7. **Present a final report** to the user:
   - Items installed
   - Items already present (skipped)
   - Items that failed (with error details)
   - Any manual steps the user needs to take

8. **Suggest running `/verify`** to confirm the environment matches the profile.
