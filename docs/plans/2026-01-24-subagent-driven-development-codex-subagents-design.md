# Subagent-Driven Development (Codex Subagents) Design

**Date:** 2026-01-24

## Goal
Update the `subagent-driven-development` skill to use Codex subagents explicitly (implementer → spec reviewer → code-quality reviewer per task), and align documentation to reflect subagent support and required experimental features.

## Architecture & Workflow
The main agent is the controller. It reads the plan once, extracts full task text + context, and seeds `update_plan` with all tasks. For each task, the controller spawns a fresh implementer subagent, waits for completion (and answers questions), then spawns a spec reviewer subagent. If spec issues are found, it loops implementer fixes and re-review until ✅. Next it spawns a code-quality reviewer subagent and loops until ✅. Only then is the task marked complete. After all tasks, a final code-quality reviewer subagent runs over the full change, then the workflow hands off to `finishing-a-development-branch`. Subagents must never be asked to read the plan file; the controller provides full task text directly.

## Prompt Templates
Rewrite the prompt templates in `skills/subagent-driven-development/references/` as explicit subagent dispatch templates:
- Implementer: task text + context, questions first, implement/test/commit/self-review, report back.
- Spec reviewer: skeptical validation vs requirements, no trust in report, file:line issues.
- Code-quality reviewer: uses requesting-code-review template with SHAs and task summary.

## Documentation Updates
Update `docs/README.codex.md` to remove “subagents not available” and reflect new subagent orchestration. Update `README.md` (Quick Start/Usage area) to note that all `/experimental` toggles must be enabled for subagent-driven-development to work, including the provided checklist.

## Verification
Prefer running `tests/claude-code/test-subagent-driven-development.sh` and optionally the integration test to validate ordering, self-review, and task-text injection. If the Claude CLI environment is not logged in, document the block and provide follow-up instructions.
