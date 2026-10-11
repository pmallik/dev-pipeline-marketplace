# dev-pipeline – reusable 9-agent Claude Code plugin

| # | Component | Type | Output |
|---|-----------|------|--------|
| 1 | brainstormer | agent | docs/solution.md |
| 2 | phase-planner | agent | docs/phases.md |
| 3 | code-generator | agent | source code, README.md, docs/progress.md |
| 4 | env-runner | agent | docs/run-report.md |
| 5 | sample-data-generator | agent | data/, seed script |
| 6 | code-validator | agent | docs/validation-report.md |
| 7 | error-logging-engineer | agent | logging config, docs/error-handling.md |
| 8 | unit-tester | agent | tests/, docs/test-report.md |
| 9 | /build | command (orchestrator) | runs 1–8 with checkpoints and fix loops |

The orchestrator is a slash command, not an agent, because subagents can't call other subagents.

## Install

### Option A – test locally
```bash
claude --plugin-dir ./plugins/dev-pipeline
```

### Option B – reuse across all projects via GitHub
1. Push this whole folder to a GitHub repo, e.g. `yourname/dev-pipeline-marketplace`.
2. In Claude Code (any project):
```
/plugin marketplace add yourname/dev-pipeline-marketplace
/plugin install dev-pipeline@dev-pipeline-marketplace
```
3. Restart Claude Code if prompted. Run `/agents` to confirm the 8 agents appear.

## Use in a project
1. Copy `plugins/dev-pipeline/templates/requirements.md` to `docs/requirements.md` in your project and fill it in.
2. (Recommended) Copy `templates/CLAUDE.md` to your project root and fill in the stack.
3. Run:
```
/build docs/requirements.md
```
or for something small:
```
/build a CLI tool that converts CSV files to JSON
```
4. Approve the design at the brainstorm checkpoint. The rest runs automatically.

Run one step on its own:
```
/pipeline-step unit-tester focus on src/payments
```

## Customising
- Edit any file in `agents/` to change behaviour; bump `version` in plugin.json.
- Set `model: opus` / `model: sonnet` / `model: haiku` in an agent's frontmatter to choose a model per step.
- Keep project-specific rules in each project's CLAUDE.md so the plugin stays generic.

Docs: https://code.claude.com/docs/en/plugins/overview.md · https://docs.claude.com/en/docs/claude-code/sub-agents
