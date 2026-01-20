---
name: requesting-code-review
description: Use when completing tasks, implementing major features, or before merging to verify work meets requirements
---

# Requesting Code Review

Run a structured code review to catch issues before they cascade. Use the template in `references/code-reviewer.md`.

**Core principle:** Review early, review often.

## When to Request Review

**Mandatory:**
- After each task in sequential execution (subagent-driven-development in Codex)
- After completing major feature
- Before merge to main

**Optional but valuable:**
- When stuck (fresh perspective)
- Before refactoring (baseline check)
- After fixing complex bug

## Quick Reference

| Step | Action | Output |
|------|--------|--------|
| Get SHAs | Identify base/head commits | Diff range |
| Review | Use code review template | Strengths + issues |
| Act | Fix Critical/Important issues | Updated code |

## How to Request

**1. Get git SHAs:**
```bash
BASE_SHA=$(git rev-parse HEAD~1)  # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

**2. Run the code review:**

Fill the template at `references/code-reviewer.md` and review the diff yourself (or ask Codex to follow the template in the current session).

**Placeholders:**
- `{WHAT_WAS_IMPLEMENTED}` - What you just built
- `{PLAN_OR_REQUIREMENTS}` - What it should do
- `{BASE_SHA}` - Starting commit
- `{HEAD_SHA}` - Ending commit
- `{DESCRIPTION}` - Brief summary

**3. Act on feedback:**
- Fix Critical issues immediately
- Fix Important issues before proceeding
- Note Minor issues for later
- Push back if reviewer is wrong (with reasoning)

## Example

```
[Just completed Task 2: Add verification function]

You: Let me request code review before proceeding.

BASE_SHA=$(git log --oneline | grep "Task 1" | head -1 | awk '{print $1}')
HEAD_SHA=$(git rev-parse HEAD)

[Run code review using `references/code-reviewer.md`]
  WHAT_WAS_IMPLEMENTED: Verification and repair functions for conversation index
  PLAN_OR_REQUIREMENTS: Task 2 from docs/plans/deployment-plan.md
  BASE_SHA: a7981ec
  HEAD_SHA: 3df7661
  DESCRIPTION: Added verifyIndex() and repairIndex() with 4 issue types

[Self-review output]:
  Strengths: Clean architecture, real tests
  Issues:
    Important: Missing progress indicators
    Minor: Magic number (100) for reporting interval
  Assessment: Ready to proceed

You: [Fix progress indicators]
[Continue to Task 3]
```

## Integration with Workflows

**Subagent-Driven Development (Codex sequential):**
- Review after EACH task
- Catch issues before they compound
- Fix before moving to next task

**Executing Plans:**
- Review after each batch (3 tasks)
- Get feedback, apply, continue

**Ad-Hoc Development:**
- Review before merge
- Review when stuck

## Common Mistakes

- Skipping reviews on "small" changes
- Reviewing without reading the actual diff
- Proceeding with unfixed Important issues

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "It is a tiny change" | Small changes still regress. Review still required. |
| "I already know it works" | Verification beats confidence. Check the diff. |
| "I'll fix issues later" | Issues compound. Fix before moving on. |

## Red Flags

**Never:**
- Skip review because "it's simple"
- Ignore Critical issues
- Proceed with unfixed Important issues
- Argue with valid technical feedback

**If reviewer wrong:**
- Push back with technical reasoning
- Show code/tests that prove it works
- Request clarification

See template at: references/code-reviewer.md
