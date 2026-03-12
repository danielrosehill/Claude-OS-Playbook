Export the current system's environment into a new profile YAML.

Scans what is installed and generates a profile that would reproduce this environment.

## Instructions

1. Determine the output filename. If `$ARGUMENTS` is provided, use it as the profile name. Otherwise, use the hostname.

2. Survey the system by running detection commands:
   - `dpkg -l` for apt packages (filter to manually installed: `apt-mark showmanual`)
   - `snap list` for snap packages
   - `flatpak list --app` for flatpak apps
   - `python3 --version`, `node --version`, `go version`, `rustc --version`, `java --version`
   - `uv tool list` for uv-installed CLI tools
   - `npm list -g --depth=0` for global npm packages
   - `docker --version` and `docker compose version`
   - `mcpm profile ls` and `mcpm ls` for MCP configuration
   - Check for `~/.claude/commands/` and `~/.claude/agents/`
   - `systemctl --user list-units --type=service --state=running`

3. Organize the results into the profile YAML schema (see `profiles/example.yaml` for reference).

4. Write the profile to `profiles/<name>.yaml`.

5. Show the user a summary of what was captured and the file path.

6. Note: The export is a best-effort snapshot. The user should review and trim it to only the tools they intentionally use.
