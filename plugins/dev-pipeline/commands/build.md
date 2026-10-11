---
description: Run the full dev-pipeline (brainstorm, plan, code, run, data, validate, logging, tests) for a requirement
argument-hint: <requirement text or path to requirements file>
---
You are the orchestrator of the dev-pipeline. Requirement: $ARGUMENTS

If the requirement is a file path, read that file. If no requirement was given, check docs/requirements.md; if that does not exist, ask the user for the requirement and stop.

Run the following subagents in order. Wait for each to finish and read its output file before starting the next. After each step, give the user a one-line status update.

1. **brainstormer** - produce docs/solution.md.
   CHECKPOINT: Show the user the recommended approach, scope, and open questions. Ask for approval or changes. Do not continue until the user approves. If they request changes, run brainstormer again with their feedback.
2. **phase-planner** - produce docs/phases.md.
3. **code-generator** - implement all phases.
4. **env-runner** - install dependencies and run the app; produces docs/run-report.md.
   If the result is FAIL: send the errors to code-generator to fix, then run env-runner again. Maximum 3 attempts; then stop and report to the user.
5. **sample-data-generator** - create data/ fixtures (and a seed script if applicable).
6. **code-validator** - produce docs/validation-report.md.
   If there are Critical or High findings: send them to code-generator to fix, then re-run code-validator once.
7. **error-logging-engineer** - add error handling and structured logging.
8. **unit-tester** - write and run tests; produces docs/test-report.md.
   If tests fail due to code bugs: send them to code-generator, then re-run unit-tester. Maximum 3 attempts.

When finished, give the user a final summary:
- What was built and where (key files)
- How to install and run it
- Test results and coverage
- Any remaining issues, open questions, or deferred items
