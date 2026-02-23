# My Unison Repository

This repository contains your AI configuration — snippets that customize how your AI assistant behaves. Unison syncs these files to your local machine automatically via a background daemon.

## Repository Structure

| Directory | Purpose |
|-----------|---------|
| `rules/` | Rules that guide AI behavior — coding standards, response style, project conventions |
| `commands/` | Reusable commands (slash commands) that trigger specific AI workflows |
| `agents/` | Agent configurations that define specialized AI personas or task runners |
| `skills/` | Skills that provide specialized capabilities your AI assistant can invoke |

Each snippet is a markdown file with YAML frontmatter that declares its metadata.

## CLI Quick Reference

| Command | Description |
|---------|-------------|
| `unison new <type> <name>` | Create a new snippet (opens your editor with a template) |
| `unison edit <name>` | Edit an existing snippet from any of your repositories |
| `unison delete <type> <name>` | Delete a snippet from your repository |
| `unison sync` | Trigger an immediate sync from GitHub to your local machine |
| `unison open` | Open the Unison dashboard in your browser |
| `unison profile list` | List all available profiles |
| `unison profile status` | Show which profile is currently active |
| `unison profile set <name>` | Activate a profile for the current directory |
| `unison profile set <name> --default` | Set a profile as the global default |
| `unison profile unset` | Deactivate the current profile |

## AI Agent Integration

Your default profile includes a Unison agent rule that lets your AI assistant manage snippets directly during conversations. You can ask your agent to:

- **Create** new rules, commands, agents, or skills based on your discussion
- **Edit** existing snippets when you want to refine behavior
- **Delete** snippets you no longer need

For example, during a conversation you might say:

> "Save that coding standard we just discussed as a Unison rule called error-handling"

> "Update my deploy command to include the new staging step"

The agent handles the rest — creating the file, updating the manifest, and syncing changes. Run `unison sync` afterwards to refresh your local library.

## Discovering Community Snippets

Your default profile includes a `/discover` skill that lets you search and install community snippets without leaving Claude Code. Just type:

> /discover code review

The skill will search Unison's public registry, compare relevant options, and recommend the best fit. When you find a snippet you like, it installs automatically as an external reference — you'll receive updates when the original author improves it.

You can also search directly from the terminal:

| Command | Description |
|---------|-------------|
| `unison explore search "query"` | Search public snippets by keyword |
| `unison explore show <repoId> <snippetId>` | View full snippet details |
| `unison adopt <owner/repo> <path>` | Adopt a snippet as an external reference |

## Daemon

The Unison daemon runs in the background and keeps your snippets in sync.

| Command | Description |
|---------|-------------|
| `unison daemon start` | Start the background daemon |
| `unison daemon stop` | Stop the daemon |
| `unison daemon status` | Check if the daemon is running |

## Manifest (`unison.yaml`)

The `unison.yaml` file at the root of your repository declares your snippets and profiles.

```yaml
# Which profile to activate by default
base: "default"

# Declare your snippets
snippets:
  - path: "rules/my-rule.md"
    title: "My Rule"
    type: "rule"
    target: ["claude"]
    description: "What this rule does"

# Group snippets into profiles
profiles:
  - name: "default"
    description: "Base profile with essential snippets"
    snippets:
      - "rules/my-rule.md"

  - name: "frontend"
    description: "Frontend development profile"
    snippets:
      - "rules/my-rule.md"
      - "rules/react-conventions.md"
```

**Snippet types:** `rule`, `command`, `agent`, `skill`
**Targets:** `claude` (more coming soon)

After editing `unison.yaml`, commit and push to GitHub. The daemon will pick up changes automatically.

## Learn More

Visit [rununison.ai](https://rununison.ai) for documentation and guides.
