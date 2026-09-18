# hermes-labs-ai/copilot-plugins

Hermes Labs plugin marketplace for **VS Code / GitHub Copilot Agent Plugins**.
This repo holds only the marketplace manifest
(`.claude-plugin/marketplace.json`) — each plugin's code stays in its own
repo and is pulled in at install time. It mirrors the plugin set from
[`hermes-labs-ai/claude-plugins`](https://github.com/hermes-labs-ai/claude-plugins),
filtered to the entries with a verified VS Code/Copilot Agent Plugin payload.

## Add the marketplace in VS Code

Add this repo as a plugin marketplace via the `chat.plugins.marketplaces`
setting (`settings.json`):

```json
{
  "chat.plugins.marketplaces": [
    "hermes-labs-ai/copilot-plugins"
  ]
}
```

Then install what you need through the Copilot Agent plugin picker, or via
the command palette's "Chat: Install Plugin" flow, selecting a plugin from
the `hermes-labs-copilot` marketplace.

## Plugins

| Name | Version | What it does | Source repo |
|---|---|---|---|
| `hermes-blind` | 0.3.0 | Local recovery anchors for a Claude Code or Codex session, plus evidence-gated evaluation prompts — for picking a session back up without trusting its own self-report | [hermes-blind](https://github.com/hermes-labs-ai/hermes-blind) (`claude-plugin`) |
| `lintlang` | 0.1.2 | Runs LintLang after an agent edits a supported prompt/config file and returns concise repair guidance for what it finds | [lintlang](https://github.com/hermes-labs-ai/lintlang) (`integrations/claude-code`) |
| `rule-audit` | 0.4.0 | On-demand static analysis of an AI system prompt or AGENTS.md: contradictions, coverage gaps, priority ambiguities, meta-paradoxes | [rule-audit](https://github.com/hermes-labs-ai/rule-audit) (repo root) |
| `little-canary` | 0.3.7 | Blocks an agent turn when a local Little Canary server rejects the submitted prompt | [little-canary](https://github.com/hermes-labs-ai/little-canary) (`plugins/claude-code`) |
| `claude-trash-guard` | 0.1.3 | Blocks permanent-delete shell commands (`rm -rf` and friends) and redirects the agent to a recoverable trash workflow instead | [agent-trash-guard](https://github.com/hermes-labs-ai/agent-trash-guard) (`integrations/claude`) |
| `agent-signage` | 0.2.1 | A `PreToolUse` hook that reports actionable git-state facts before file operations | [agent-signage](https://github.com/hermes-labs-ai/agent-signage) (`claude-plugin`) |
| `hermes-jailbench` | 0.2.1 | Jailbreak regression benchmark for LLM endpoints with repeatable known-pattern attacks and deterministic scoring | [hermes-jailbench](https://github.com/hermes-labs-ai/hermes-jailbench) (repo root) |
| `agent-kickstart` | 0.3.0 | A guided, project-local first experience that helps beginners start making something real | [agent-kickstart](https://github.com/hermes-labs-ai/agent-kickstart) (repo root) |
| `intent-verify` | 0.2.0 | Maps markdown acceptance items to explicit implementation evidence for advisory spec-drift checks | [intent-verify](https://github.com/hermes-labs-ai/intent-verify) (repo root) |
| `quick-gate-python` | 0.3.1 | Runs the deterministic `pygate` Python quality gate (Ruff, Pyright, pytest) and reads its `gate-result/v1` verdict | [quick-gate-python](https://github.com/hermes-labs-ai/quick-gate-python) (repo root) |
| `quick-gate-js` | 0.3.0 | Runs the released `quick-gate` JS/TS quality gate (ESLint, TypeScript, build, Lighthouse) and reads its `gate-result/v1` verdict | [quick-gate-js](https://github.com/hermes-labs-ai/quick-gate-js) (repo root) |
| `hermes-gate` | 0.1.5 | Receipt-bound completion rail: `SessionStart` injects the completion contract, `Stop` runs the cached fast gate advisory-only, `PreToolUse` denies a Bash commit/push/PR boundary command missing its matching receipt | [hermes-gate](https://github.com/hermes-labs-ai/hermes-gate) (`claude-plugin`, `v0.1.5`) |

All 12 versions above were verified against each upstream repo's current
`.claude-plugin/plugin.json` at the pinned `ref` before being listed here.

## Not included

`hermeneutic-gate` (from the Claude Code marketplace this repo mirrors) is
**excluded**: it's a legacy advisory Stop-hook bundle without a verified
VS Code/Copilot Agent Plugin payload adapter. See
[`hermes-labs-ai/claude-plugins`](https://github.com/hermes-labs-ai/claude-plugins)
if you're running Claude Code instead of VS Code/Copilot and want that entry.

## Adding a plugin

1. The upstream repo needs a `.claude-plugin/plugin.json` (see any repo above
   for the shape: `name`, `version`, `description`, `author`, `homepage`,
   `repository`, `license`) and a verified VS Code/Copilot Agent Plugin
   payload adapter.
2. Add an entry to `plugins` in `.claude-plugin/marketplace.json`. If
   `plugin.json` lives at the repo root, use the HTTPS `url` source so the
   complete plugin is cloned:
   ```json
   {
     "name": "<plugin name>",
     "description": "<one line>",
     "version": "<matches upstream plugin.json>",
     "category": "<development|developer-tools|security|...>",
     "source": {
       "source": "url",
       "url": "https://github.com/hermes-labs-ai/<repo>.git",
       "ref": "main"
     }
   }
   ```
   If the plugin lives in a subdirectory of the repo, use `git-subdir`
   instead, which sparsely clones just that subdirectory:
   ```json
   {
     "name": "<plugin name>",
     "description": "<one line>",
     "version": "<matches upstream plugin.json>",
     "category": "<development|developer-tools|security|...>",
     "source": {
       "source": "git-subdir",
       "url": "https://github.com/hermes-labs-ai/<repo>.git",
       "path": "<subdir containing .claude-plugin/plugin.json>",
       "ref": "main"
     }
   }
   ```
3. Validate before committing: `claude plugin validate .claude-plugin/marketplace.json --strict`.
4. Bump `version` here whenever the upstream plugin's own
   `.claude-plugin/plugin.json` bumps its version — this repo does not
   re-derive it automatically.

## Notes

- `source.ref` pins each plugin to its repo's `main` branch (or a tag, for
  `hermes-gate`). Pin the rest to tags once those plugins start cutting
  releases.
- `agent-signage`'s source metadata is maintained independently by its owner;
  this repo mirrors it as-is rather than re-deriving it.
