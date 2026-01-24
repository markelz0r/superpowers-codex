# Subagent-Driven Development: Main-Agent Implementation Design

Date: 2026-01-24

## Goal
Update `subagent-driven-development` so the main agent implements tasks in-thread while subagents are used only for review (spec compliance then code quality). Preserve the two-stage review per task and the final whole-change review.

## Scope
- File: `skills/subagent-driven-development/SKILL.md`
- Update workflow description, process graph, quick reference, prompt template list, and example workflow to remove implementer subagent usage.
- Clarify that `spawn_agent` is used only for reviewer subagents.

## Non-Goals
- No changes to other skills or docs outside `skills/subagent-driven-development/SKILL.md`.
- Do not remove or edit subagent prompt templates; just stop referencing the implementer template in the active workflow.

## Proposed Workflow
1. Main agent reads the plan once, extracts full task text/context, and seeds `update_plan`.
2. For each task, the main agent implements directly in the current thread (edits, tests, self-checks).
3. Spawn spec compliance reviewer subagent; loop fixes until ✅.
4. Spawn code-quality reviewer subagent; loop fixes until ✅.
5. Mark task complete; repeat.
6. After all tasks, spawn final code-quality reviewer for whole-change check.

## Changes to Document
- Title summary and core principle: “main agent implements; reviewers are subagents.”
- Process graph nodes/edges: replace implementer subagent nodes with main-agent implementation steps.
- Codex Subagent Tools: specify review-only subagents.
- Prompt Templates list: remove implementer prompt from active list (or label as legacy).
- Quick Reference: rename implementer pass to main agent implementation.
- Example workflow: replace implementer subagent dialogue with main-agent actions.

## Testing
- Documentation-only change; no automated tests required.
- Spot-check that the document is internally consistent (no leftover “implementer subagent” flow in the active process).
