Verify the current system against a profile.

Checks what is installed vs. what the profile expects, without making any changes.

## Instructions

1. Load the profile (from `$ARGUMENTS` or `default.yaml`, or ask the user).

2. For each category in the profile, check installation status:
   - **system_packages**: Run `dpkg -l`, `snap list`, `flatpak list` as appropriate
   - **languages**: Check `python3 --version`, `node --version`, `go version`, `rustc --version`, `java --version`
   - **cli_tools**: Check `which <tool>` or `uv tool list`, `npm list -g`
   - **containers**: Check `docker --version`, `docker compose version`
   - **shell**: Check if expected lines exist in dotfiles
   - **claude**: Check for expected files in `~/.claude/commands/`, MCP profiles via `mcpm profile ls`
   - **desktop_apps**: Check via `which`, `snap list`, `flatpak list`
   - **services**: Check `systemctl status`

3. Present a report with three columns:
   - **Satisfied**: Present and matching expected version (or any version if unspecified)
   - **Missing**: Not found on the system
   - **Version mismatch**: Present but wrong version (only if profile specifies a version)

4. Show a summary count: X satisfied, Y missing, Z mismatched.
