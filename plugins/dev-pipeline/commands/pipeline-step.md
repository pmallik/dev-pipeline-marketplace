---
description: Run a single dev-pipeline agent (e.g. /pipeline-step unit-tester focus on src/auth)
argument-hint: <agent-name> [extra instructions]
---
Run only the dev-pipeline subagent named in the first word of: $ARGUMENTS

Valid names: brainstormer, phase-planner, code-generator, env-runner, sample-data-generator, code-validator, error-logging-engineer, unit-tester.

Pass any remaining text to that agent as extra instructions. If the agent needs an input file from an earlier step that doesn't exist, tell the user which step to run first. Report the agent's result when done.
