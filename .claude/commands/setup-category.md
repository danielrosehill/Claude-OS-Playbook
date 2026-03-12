Provision a single category from the active profile.

Usage: `/setup-category <category> [profile]`

Where `<category>` is one of: system_packages, languages, cli_tools, containers, shell, claude, desktop_apps, services.

## Instructions

1. Parse `$ARGUMENTS` to extract the category name and optional profile name.

2. Load the profile YAML (same resolution logic as `/setup` — use profile name if given, otherwise `default.yaml`, otherwise ask).

3. Extract the section matching the requested category.

4. If the category doesn't exist in the profile, inform the user and list which categories are defined.

5. Delegate to the appropriate subagent for that category, passing the relevant profile section.

6. Report results: installed, skipped, failed.
