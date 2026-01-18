# OpenCode TDD Plugin - Global Setup (Auto-Load Method)

The opencode-tdd plugin is configured to load **globally** across all projects using OpenCode's auto-load directory.

## Current Configuration

### Global Plugin Directory (Auto-Load)
Location: `~/.config/opencode/plugin/opencode-tdd` → symlink to `/path/to/opencode-tdd`

OpenCode **automatically loads** all plugins in `~/.config/opencode/plugin/` without needing any configuration file!

### Global Commands Directory
Location: `~/.config/opencode/command/tdd/` → symlink to `/path/to/opencode-tdd/commands`

Commands are symlinked separately because OpenCode discovers commands from `~/.config/opencode/command/`, not from plugin directories.

### Global Config (Clean)
Location: `~/.config/opencode/opencode.json`

```json
{
  "$schema": "https://opencode.ai/config.json"
}
```

No plugin specified here - the auto-load directory handles it!

## How It Works

```
OpenCode starts
    ↓
Automatically scans ~/.config/opencode/plugin/
    ↓
Finds symlink: opencode-tdd → /path/to/opencode-tdd
    ↓
Loads dist/index.js from that directory
    ↓
Plugin active in ALL projects! ✅
```

## Why This Method Is Best

✅ **No config needed** - OpenCode auto-loads from the plugin directory
✅ **Works with all package managers** - Bypasses npm/bun entirely
✅ **Simple symlink** - Points to your development directory
✅ **Instant updates** - Rebuild and restart, changes are live
✅ **No freezing** - Doesn't try to install from npm

## Available Everywhere

Open OpenCode in **any project** and you'll have access to:

### Slash Commands
- `/tdd-init` - Initialize TDD structure
- `/tdd-start` - Start Actor-Critic workflow
- `/tdd-status` - Check progress
- `/architect-full` - Generate all docs
- `/architect-prd` - Generate PRD only

### Agents
- `@actor` - TDD implementer
- `@critic` - TDD validator
- `@orchestrator` - Workflow coordinator
- `@architect` - Document generator

### Tools (available in agent contexts)
- `tdd_init`
- `tdd_status`
- `tdd_next`
- `tdd_state`

## Making Changes

When you update the plugin code:

```bash
cd /path/to/opencode-tdd

# 1. Edit source files
vim src/agents/actor.ts

# 2. Rebuild
npm run build

# 3. Restart OpenCode in any project
# Changes are now live everywhere via the symlink!
```

## Directory Structure

```
~/.config/opencode/
├── opencode.json              # Global OpenCode config (clean)
└── plugin/                    # Auto-load directory
    └── opencode-tdd/          # Symlink → /path/to/opencode-tdd
        ├── dist/              # Built JavaScript (auto-loaded)
        ├── commands/          # Slash commands (auto-discovered)
        ├── src/               # Your source code
        └── package.json
```

## Per-Project Configuration (Optional)

You can still override settings per project by creating `opencode-tdd.json`:

```json
{
  "models": {
    "actor": "anthropic/claude-sonnet-4-20250514",
    "critic": "anthropic/claude-sonnet-4-20250514"
  },
  "workflow": {
    "maxRetries": 3,
    "testCommand": "npm test"
  }
}
```

Place in:
- `{project}/opencode-tdd.json` (highest priority)
- `{project}/.opencode/opencode-tdd.json`
- `~/.config/opencode/opencode-tdd.json` (global defaults)

## Testing in a New Project

```bash
# Go to any project
cd ~/my-other-project

# Start OpenCode
opencode

# Try the plugin
> /tdd-init

# It should work! 🎉
```

## Disabling the Plugin

### Option 1: Remove the symlink
```bash
rm ~/.config/opencode/plugin/opencode-tdd
# Plugin no longer loads
```

### Option 2: Temporarily rename it
```bash
mv ~/.config/opencode/plugin/opencode-tdd ~/.config/opencode/plugin/opencode-tdd.disabled
# Plugin disabled, easy to re-enable
```

### Option 3: Override in specific project
Create `opencode.json` in a specific project to disable just there:
```json
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": []
}
```

## Re-enabling After Removal

```bash
ln -sf /path/to/opencode-tdd ~/.config/opencode/plugin/opencode-tdd
# Plugin is globally available again!
```

## Troubleshooting

### OpenCode freezes on startup

**Cause**: Plugin specified in config file trying to install from npm/bun

**Fix**: Remove plugin from `~/.config/opencode/opencode.json` and rely on auto-load directory only:
```json
{
  "$schema": "https://opencode.ai/config.json"
}
```

### Plugin not loading

Check the symlink exists:
```bash
ls -la ~/.config/opencode/plugin/opencode-tdd
# Should show: opencode-tdd -> /path/to/opencode-tdd
```

Check the build exists:
```bash
ls /path/to/opencode-tdd/dist/
# Should show: index.js and index.mjs
```

Rebuild if needed:
```bash
cd /path/to/opencode-tdd
npm run build
```

### Commands not available

OpenCode auto-discovers commands from the `commands/` directory. Check they exist:
```bash
ls /path/to/opencode-tdd/commands/
# Should show: tdd-init.md, tdd-start.md, etc.
```

## Publishing to NPM (Future)

When ready to share publicly:

```bash
# 1. Update version
npm version patch

# 2. Login to npm
npm login

# 3. Publish
npm publish
```

Then users install normally:
```bash
npm install -g opencode-tdd
# Or add to project: npm install opencode-tdd
```

And add to their `opencode.json`:
```json
{
  "plugin": ["opencode-tdd"]
}
```

## Summary

- ✅ Plugin loads in **all projects** automatically via `~/.config/opencode/plugin/`
- ✅ **No config file needed** - auto-load directory handles it
- ✅ **Symlink to development directory** - all changes immediately available after rebuild
- ✅ **No npm/bun issues** - bypasses package managers entirely
- ✅ **No freezing** - doesn't try to install anything
- ✅ Can still override settings per project

This is the **cleanest and most reliable** approach for local plugin development!
