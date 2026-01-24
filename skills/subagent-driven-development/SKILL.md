---
name: subagent-driven-development
description: Use when executing implementation plans with independent tasks in the current session
---

# Subagent-Driven Development

Execute plan by dispatching fresh subagent per task, with two-stage review after each: spec compliance review first, then code quality review.

**Core principle:** Fresh subagent per task + two-stage review (spec then quality) = high quality, fast iteration

**Codex workflow:** Orchestrate subagents with spawn_agent/wait/send_input using prompts in `./references/`.

## When to Use

```dot
digraph when_to_use {
    "Have implementation plan?" [shape=diamond];
    "Tasks mostly independent?" [shape=diamond];
    "Stay in this session?" [shape=diamond];
    "subagent-driven-development" [shape=box];
    "executing-plans" [shape=box];
    "Manual execution or brainstorm first" [shape=box];

    "Have implementation plan?" -> "Tasks mostly independent?" [label="yes"];
    "Have implementation plan?" -> "Manual execution or brainstorm first" [label="no"];
    "Tasks mostly independent?" -> "Stay in this session?" [label="yes"];
    "Tasks mostly independent?" -> "Manual execution or brainstorm first" [label="no - tightly coupled"];
    "Stay in this session?" -> "subagent-driven-development" [label="yes"];
    "Stay in this session?" -> "executing-plans" [label="no - parallel session"];
}
```

**vs. Executing Plans (parallel session):**
- Same session (no context switch)
- Fresh subagent per task (no context pollution)
- Two-stage review after each task: spec compliance first, then code quality
- Faster iteration (no human-in-loop between tasks)

## The Process

```dot
digraph process {
    rankdir=TB;

    subgraph cluster_per_task {
        label="Per Task";
        "Dispatch implementer subagent (./references/implementer-prompt.md)" [shape=box];
        "Implementer subagent asks questions?" [shape=diamond];
        "Answer questions, provide context" [shape=box];
        "Implementer subagent implements, tests, commits, self-reviews" [shape=box];
        "Dispatch spec reviewer subagent (./references/spec-reviewer-prompt.md)" [shape=box];
        "Spec reviewer subagent confirms code matches spec?" [shape=diamond];
        "Implementer subagent fixes spec gaps" [shape=box];
        "Dispatch code quality reviewer subagent (./references/code-quality-reviewer-prompt.md)" [shape=box];
        "Code quality reviewer subagent approves?" [shape=diamond];
        "Implementer subagent fixes quality issues" [shape=box];
        "Mark task complete in update_plan" [shape=box];
    }

    "Read plan, extract all tasks with full text, note context, create update_plan" [shape=box];
    "More tasks remain?" [shape=diamond];
    "Dispatch final code reviewer subagent for entire implementation" [shape=box];
    "Use $finishing-a-development-branch" [shape=box style=filled fillcolor=lightgreen];

    "Read plan, extract all tasks with full text, note context, create update_plan" -> "Dispatch implementer subagent (./references/implementer-prompt.md)";
    "Dispatch implementer subagent (./references/implementer-prompt.md)" -> "Implementer subagent asks questions?";
    "Implementer subagent asks questions?" -> "Answer questions, provide context" [label="yes"];
    "Answer questions, provide context" -> "Dispatch implementer subagent (./references/implementer-prompt.md)";
    "Implementer subagent asks questions?" -> "Implementer subagent implements, tests, commits, self-reviews" [label="no"];
    "Implementer subagent implements, tests, commits, self-reviews" -> "Dispatch spec reviewer subagent (./references/spec-reviewer-prompt.md)";
    "Dispatch spec reviewer subagent (./references/spec-reviewer-prompt.md)" -> "Spec reviewer subagent confirms code matches spec?";
    "Spec reviewer subagent confirms code matches spec?" -> "Implementer subagent fixes spec gaps" [label="no"];
    "Implementer subagent fixes spec gaps" -> "Dispatch spec reviewer subagent (./references/spec-reviewer-prompt.md)" [label="re-review"];
    "Spec reviewer subagent confirms code matches spec?" -> "Dispatch code quality reviewer subagent (./references/code-quality-reviewer-prompt.md)" [label="yes"];
    "Dispatch code quality reviewer subagent (./references/code-quality-reviewer-prompt.md)" -> "Code quality reviewer subagent approves?";
    "Code quality reviewer subagent approves?" -> "Implementer subagent fixes quality issues" [label="no"];
    "Implementer subagent fixes quality issues" -> "Dispatch code quality reviewer subagent (./references/code-quality-reviewer-prompt.md)" [label="re-review"];
    "Code quality reviewer subagent approves?" -> "Mark task complete in update_plan" [label="yes"];
    "Mark task complete in update_plan" -> "More tasks remain?";
    "More tasks remain?" -> "Dispatch implementer subagent (./references/implementer-prompt.md)" [label="yes"];
    "More tasks remain?" -> "Dispatch final code reviewer subagent for entire implementation" [label="no"];
    "Dispatch final code reviewer subagent for entire implementation" -> "Use $finishing-a-development-branch";
}
```

## Codex Subagent Tools

- Use `spawn_agent` to dispatch each subagent (generic agent_type; role defined in prompt).
- Use `wait` to monitor until a subagent finishes.
- Use `send_input` to answer questions or request fixes.
- Close subagents only when their work is complete.

## Prompt Templates

- `./references/implementer-prompt.md` - Dispatch implementer subagent
- `./references/spec-reviewer-prompt.md` - Dispatch spec compliance reviewer subagent
- `./references/code-quality-reviewer-prompt.md` - Dispatch code quality reviewer subagent

## Quick Reference

| Pass | Goal | Output |
|------|------|--------|
| Implementer subagent | Implement task, tests, commit | Implementation + report |
| Spec compliance reviewer | Validate requirements, no extras | ✅/❌ with file references |
| Code quality reviewer | Review maintainability, tests | Issues list + assessment |
| Final code reviewer | Whole-change review | Ready-to-merge check |

## Example Workflow

```
You: I'm using Subagent-Driven Development to execute this plan.

[Read plan file once: docs/plans/feature-plan.md]
[Extract all 5 tasks with full text and context]
[Create update_plan with all tasks]

Task 1: Hook installation script

[Get Task 1 text and context (already extracted)]
[Dispatch implementer subagent with full task text + context]

Implementer: "Before I begin - should the hook be installed at user or system level?"

You: "User level (~/.config/superpowers/hooks/)"

Implementer: "Got it. Implementing now..."
[Later] Implementer:
  - Implemented install-hook command
  - Added tests, 5/5 passing
  - Self-review: Found I missed --force flag, added it
  - Committed

[Dispatch spec compliance reviewer]
Spec reviewer: ✅ Spec compliant - all requirements met, nothing extra

[Get git SHAs, dispatch code quality reviewer]
Code reviewer: Strengths: Good test coverage, clean. Issues: None. Approved.

[Mark Task 1 complete]

Task 2: Recovery modes

[Get Task 2 text and context (already extracted)]
[Dispatch implementer subagent with full task text + context]

Implementer: [No questions, proceeds]
Implementer:
  - Added verify/repair modes
  - 8/8 tests passing
  - Self-review: All good
  - Committed

[Dispatch spec compliance reviewer]
Spec reviewer: ❌ Issues:
  - Missing: Progress reporting (spec says "report every 100 items")
  - Extra: Added --json flag (not requested)

[Implementer fixes issues]
Implementer: Removed --json flag, added progress reporting

[Spec reviewer reviews again]
Spec reviewer: ✅ Spec compliant now

[Dispatch code quality reviewer]
Code reviewer: Strengths: Solid. Issues (Important): Magic number (100)

[Implementer fixes]
Implementer: Extracted PROGRESS_INTERVAL constant

[Code reviewer reviews again]
Code reviewer: ✅ Approved

[Mark Task 2 complete]

...

[After all tasks]
[Dispatch final code reviewer]
Final reviewer: All requirements met, ready to merge

Done!
```

## Advantages

**vs. Manual execution:**
- Fresh context per task (no confusion)
- Subagent can ask questions before and during work
- Two-stage review catches spec and quality issues early
- Works entirely in one Codex session

**vs. Executing Plans:**
- Same session (no handoff)
- Continuous progress (no waiting)
- Review checkpoints explicit

**Efficiency gains:**
- No cross-session handoff
- Controller provides full task text (no plan-file reads)
- Issues surfaced before moving on

**Quality gates:**
- Self-review catches issues before handoff
- Two-stage review: spec compliance, then code quality
- Review loops ensure fixes actually work
- Spec compliance prevents over/under-building
- Code quality ensures implementation is well-built

**Cost:**
- More subagent invocations per task
- Controller does more prep work (extracting all tasks upfront)
- Review loops add iterations
- But catches issues early (cheaper than debugging later)

## Common Mistakes

- Skipping spec compliance before code quality review
- Moving to the next task with open review issues
- Starting implementation with unclear requirements
- Dispatching multiple implementer subagents in parallel
- Making subagents read the plan file instead of providing full task text

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "I'll review after all tasks" | Issues compound. Review after each task prevents rework. |
| "Spec is obvious, no need for spec pass" | Implementation drift is common. Verify against requirements. |
| "Code quality can wait" | Later reviews miss context. Fix now while it's fresh. |
| "It's a tiny change" | Small changes still regress. Review still required. |
| "Subagents aren't available, I'll just do it myself" | If subagents are required, enable /experimental or pause. Manual fallback changes the workflow. |
| "The implementer report is enough" | Spec reviewers must verify code, not trust reports. |
| "We can skip the reviewer for this one task" | Skipping a single reviewer breaks the quality gate. |

**Violating the letter of the rules is violating the spirit of the rules.**

## Red Flags

**Never:**
- Skip reviews (spec compliance OR code quality)
- Proceed with unfixed issues
- Skip dispatching required subagents (implementer or reviewers)
- Dispatch multiple implementer subagents in parallel (conflicts)
- Start multiple tasks at once (context bleed)
- Make subagents read the plan file (provide full task text instead)
- Skip scene-setting context (you need to know where the task fits)
- Ignore questions or proceed with ambiguity
- Accept "close enough" on spec compliance (spec compliance pass found issues = not done)
- Skip review loops (review pass found issues = implementer fixes = review again)
- Let implementer self-review replace actual review (both are needed)
- **Start code quality review before spec compliance is ✅** (wrong order)
- Move to next task while either review has open issues

**If you have questions:**
- Answer clearly and completely
- Provide additional context if needed
- Don't rush them into implementation

**If reviewer finds issues:**
- Implementer subagent fixes them
- Reviewer re-checks the fix
- Repeat until approved
- Don't skip the re-review

**If you missed issues after moving on:**
- Return to the task and rerun the review passes

## Integration

**Required workflow skills:**
- **$writing-plans** - Creates the plan this skill executes
- **$requesting-code-review** - Code review template for reviewer subagents
- **$finishing-a-development-branch** - Complete development after all tasks

**Use with:**
- **$test-driven-development** - Follow TDD per task when in scope

**Alternative workflow:**
- **$executing-plans** - Use when you must switch to a separate session
