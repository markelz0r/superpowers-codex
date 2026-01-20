---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** Prefer a dedicated worktree for higher-risk changes; otherwise ask whether to use a worktree or stay in the current workspace (set during brainstorming).

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For Codex:** REQUIRED SUB-SKILL: Use $subagent-driven-development to implement this plan task-by-task.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

```markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

**Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

**Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
```

## Quick Reference

| Item | Must include |
|------|--------------|
| Plan header | Required sub-skill line, goal, architecture, tech stack |
| Each task | Exact file paths, test steps, commands, commit |
| Steps | One action each (2-5 minutes) |

## Common Mistakes

- Skipping exact file paths
- Packing multiple actions into a single step
- Missing test commands or expected output
- Asking for workflow choice in Codex

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Details can go in the implementation" | Plans must be self-contained and executable. |
| "This is obvious, no need for commands" | Explicit commands prevent mistakes and drift. |
| "I'll ask which workflow later" | In Codex, default to sequential execution. |

## Red Flags

- Steps use vague verbs like "handle" or "update" without code/commands
- Tasks cannot be completed in 2-5 minutes each
- Missing a clear testing path for each task

## Remember
- Exact file paths always
- Complete code in plan (not "add validation")
- Exact commands with expected output
- Reference relevant skills with @ syntax
- DRY, YAGNI, TDD, frequent commits

## Execution Handoff

After saving the plan, proceed without asking for a workflow choice in Codex.

**For Codex (single session):**
- **REQUIRED SUB-SKILL:** Use $subagent-driven-development
- Execute tasks sequentially with self-review passes
- Track progress in update_plan

**If a separate session is required:**
- Open a new session in the chosen workspace (worktree if used, otherwise current workspace)
- **REQUIRED SUB-SKILL:** Use $executing-plans in that session
