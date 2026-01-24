# Superpowers for Codex

Codex-native skills live in `.codex/skills` and are loaded automatically by Codex when you start in this repository.

## Quick Start (Repo-Scoped Skills)

1. **Clone the repo**:

```bash
git clone https://github.com/obra/superpowers.git
cd superpowers
```

2. **Start Codex from the repo root**.

3. **Invoke skills** via `/skills` or `$skill-name` (e.g., `$brainstorming`).

## Usage

### Browse available skills

Use the `/skills` picker in Codex.

### Invoke a skill explicitly

```
$brainstorming
```

Skills can also be invoked implicitly when the task matches their description.

## Skill Locations (Codex)

Codex loads skills from multiple scopes with higher-priority locations overriding lower ones:

- **Repo**: `.codex/skills` (this repo)
- **User**: `~/.codex/skills`

If you want these skills globally, copy or symlink `.codex/skills` into `~/.codex/skills`.

## Install via Codex UI

If you prefer a UI-driven install, ask Codex to use `$skill-installer` to pull skills from this repo into your user scope. The installer supports GitHub repo paths and will place skills under `~/.codex/skills`.

## Tool Mapping Notes

Some skills reference tools from other agents. In Codex:

- `TodoWrite` → `update_plan`
- Subagents are supported; enable all `/experimental` features and use `spawn_agent`/`wait`/`send_input` for orchestration

## Updating

```bash
git pull
```

Restart Codex after updating to reload skill metadata.

## Troubleshooting

- **Skills not showing**: Ensure you launched Codex from the repo root and `.codex/skills` exists.
- **Stale metadata**: Restart Codex to reload skill definitions.

## Getting Help

- Report issues: https://github.com/obra/superpowers/issues
- Main documentation: https://github.com/obra/superpowers
