# OpenCode Configuration

Lean configuration for OpenCode: agents, commands, prompts, and instructions.
No plugin runtime, no custom tools, no hooks — pure config assets.

## Installation

To use this configuration in your own project, copy the entire `.opencode/`
directory into the project root. OpenCode picks up `.opencode/opencode.json`
automatically: it registers the agents, slash commands, skills, and
instruction files. The configuration is self-contained and has no
dependencies outside `.opencode`.

## Features

### Agents (8)

| Agent | Description |
|-------|-------------|
| build | Primary coding agent for development work |
| planner | Implementation planning |
| architect | System design |
| code-reviewer | Code review |
| security-reviewer | Security analysis |
| build-error-resolver | Build error fixes |
| refactor-cleaner | Dead code cleanup |
| docs-lookup | Documentation lookup via Context7 |

### Commands (9)

| Command | Description |
|---------|-------------|
| `/plan` | Create implementation plan |
| `/code-review` | Review code changes |
| `/security` | Security review |
| `/build-fix` | Fix build errors |
| `/refactor-clean` | Remove dead code |
| `/orchestrate` | Multi-agent workflow |
| `/checkpoint` | Save progress |
| `/eval` | Evaluation |
| `/skill-create` | Generate skills |

### MCP Servers (3)

MCP (Model Context Protocol) is a standard for connecting external tools
to the agent. The connected servers are listed in `mcp.servers` in
`opencode.json`.

| Server | Type | Purpose |
|--------|------|---------|
| context7 | remote | Up-to-date library and API documentation (React, Next.js, Prisma, etc.) with code examples — instead of the model's outdated training knowledge |
| playwright | local | Browser automation via Playwright: navigation, clicks, form filling, screenshots, console and network request inspection |
| chrome-devtools | local | Debugging via the Chrome DevTools Protocol: inspecting open pages, performance profiling, logs, and network activity in real Chrome |

The `playwright` and `chrome-devtools` servers run locally via `npx` and
require Chrome to be installed. `context7` is a remote server that works
over HTTPS with no local installation required.

## Skills

All skills live inside `.opencode/skills/` — the configuration is fully
self-contained and has no references to skill directories outside `.opencode`.

The 3 curated skills are loaded via the `instructions` array:

- coding-standards
- security-review
- verification-loop

OpenCode also discovers `.opencode/skills/` natively, so every skill placed
there is additionally available on demand through the `skill` tool.

## Configuration

Full configuration in `opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    ".opencode/skills/security-review/SKILL.md"
  ],
  "agents": { /* 8 agents */ },
  "commands": { /* 9 commands */ }
}
```

The reference config intentionally leaves model selection to OpenCode. Connect a
provider and select a model in OpenCode; primary agent uses that global
selection, and its subagents inherit the invoking primary agent's model.

## License

MIT
