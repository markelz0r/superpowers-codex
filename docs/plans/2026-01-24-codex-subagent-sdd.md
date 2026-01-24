# Codex Subagent-Driven Development Update Implementation Plan

> **For Codex:** REQUIRED SUB-SKILL: Use $subagent-driven-development to implement this plan task-by-task.

**Goal:** Update subagent-driven-development to explicitly use Codex subagents and align docs with subagent support and /experimental prerequisites.

**Architecture:** Convert the workflow from self-review passes to explicit subagent orchestration (implementer → spec reviewer → code-quality reviewer). Update prompt templates and docs to reflect Codex tool usage and prerequisites.

**Tech Stack:** Markdown skill docs, repo documentation, Codex subagent tooling.

---

### Task 1: Update core skill workflow (SKILL.md)

**Files:**
- Modify: `skills/subagent-driven-development/SKILL.md`

**Step 1: Edit workflow language to explicit subagent orchestration**
- Replace self-review wording with spawn/wait loops and subagent roles.

**Step 2: Update flowchart labels and update_plan references**
- Replace TodoWrite with update_plan, add spawn/wait steps, keep spec→quality order.

**Step 3: Update example workflow to show subagent dispatch**
- Ensure example mentions provide full task text, no plan file reads by subagents.

**Step 4: Commit**
```bash
git add skills/subagent-driven-development/SKILL.md
git commit -m "docs: orchestrate subagents in sdd skill"
```

### Task 2: Update subagent prompt templates

**Files:**
- Modify: `skills/subagent-driven-development/references/implementer-prompt.md`
- Modify: `skills/subagent-driven-development/references/spec-reviewer-prompt.md`
- Modify: `skills/subagent-driven-development/references/code-quality-reviewer-prompt.md`

**Step 1: Convert implementer template to subagent dispatch prompt**
- Include full task text, context, questions-first, implement/test/commit/self-review/report.

**Step 2: Convert spec reviewer template to skeptical subagent prompt**
- Require code inspection and explicit ✅/❌ with file:line references.

**Step 3: Convert code-quality reviewer template to subagent prompt**
- Use requesting-code-review template + SHAs.

**Step 4: Commit**
```bash
git add skills/subagent-driven-development/references/implementer-prompt.md \
  skills/subagent-driven-development/references/spec-reviewer-prompt.md \
  skills/subagent-driven-development/references/code-quality-reviewer-prompt.md
git commit -m "docs: refresh sdd subagent prompt templates"
```

### Task 3: Update docs for subagent support + /experimental prerequisites

**Files:**
- Modify: `docs/README.codex.md`
- Modify: `README.md`
- (Optional) Modify: `RELEASE-NOTES.md`

**Step 1: Update Codex README to reflect subagent support**
- Remove “subagents not available” and replace with subagent-enabled guidance.

**Step 2: Add /experimental prerequisite note in README**
- Add short note under Quick Start/Usage with full checklist.

**Step 3: If needed, add a short release note about subagent support**
- Add to top of `RELEASE-NOTES.md` with current date.

**Step 4: Commit**
```bash
git add docs/README.codex.md README.md RELEASE-NOTES.md
git commit -m "docs: update codex subagent guidance"
```

### Task 4: Verification

**Files:**
- None (commands only)

**Step 1: Re-scan for stale wording**
```bash
rg -n "subagents are not available|manual fallback" -S README.md docs RELEASE-NOTES.md
```
Expected: no matches that contradict subagent support.

**Step 2: Run skill tests (if Claude CLI is logged in)**
```bash
bash tests/claude-code/test-subagent-driven-development.sh
```
Expected: "=== All subagent-driven-development skill tests passed ==="

**Step 3: Commit (if changes made after verification)**
```bash
git status -sb
```
