# Codex Native Skills Migration Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Convert all Superpowers skills to Codex-native skills under `.codex/skills/`, update references for Codex tooling and `$skill` invocation, remove legacy bootstrap scripts/docs, and refresh documentation for native Codex installation.

**Architecture:** Mirror the existing `skills/` tree into `.codex/skills/` with minimal content changes, then apply targeted edits for Codex-native skill invocation and tool mappings. Remove `.codex/superpowers-codex`, `.codex/INSTALL.md`, and `.codex/superpowers-bootstrap.md` to avoid legacy bootstrap flows. Update repo docs to describe Codex native skills and installation via Codex UI/CLI.

**Tech Stack:** Markdown, shell tooling (`rg`, `cp`, `rm`), Codex skill spec.

### Task 1: Inventory skills and Codex-specific requirements

**Files:**
- Inspect: `skills/`
- Inspect: `docs/README.codex.md`
- Inspect: `README.md`

**Step 1: List skill directories**

Run: `ls skills`
Expected: list of skill folders (e.g., `brainstorming`, `writing-plans`, etc.).

**Step 2: Find tool/invocation references to adjust**

Run: `rg -n "TodoWrite|Task tool|Skill tool|/superpowers:|superpowers:" skills docs README.md .codex -S`
Expected: matches to update for Codex-native `$skill` invocation and tool mappings.

**Step 3: Note Codex docs to update**

Run: `rg -n "Codex" docs README.md -S`
Expected: existing Codex install guidance referencing legacy bootstrap scripts.

**Step 4: Commit**

Skip (planning only).

### Task 2: Create Codex-native skill tree in `.codex/skills`

**Files:**
- Create: `.codex/skills/` (mirroring `skills/`)

**Step 1: Copy skill directories**

Run: `mkdir -p .codex/skills && cp -R skills/* .codex/skills/`
Expected: `.codex/skills/<skill>/SKILL.md` mirrors existing content.

**Step 2: Verify copied files**

Run: `rg --files .codex/skills | head -20`
Expected: `.codex/skills/...` paths listed.

**Step 3: Commit**

Skip (planning only).

### Task 3: Adjust Codex-native skill content (tool mapping + $skill invocations)

**Files:**
- Modify: `.codex/skills/**/SKILL.md`
- Modify: `.codex/skills/**` reference files as needed

**Step 1: Replace slash or colon skill invocations**

Update references like `/superpowers:brainstorm` or `superpowers:writing-plans` to `$brainstorming` or `$writing-plans` in `.codex/skills/**`.

**Step 2: Update tool mappings for Codex**

Replace Claude-specific tool references (e.g., `TodoWrite`, `Task tool`, `Skill tool`) with Codex equivalents or explicit guidance (e.g., `update_plan`, no subagents in Codex).

**Step 3: Keep prompts consistent**

Maintain existing workflows and prompts unless a Codex tool difference requires changes.

**Step 4: Verify no legacy references remain**

Run: `rg -n "/superpowers:|superpowers:|TodoWrite|Task tool|Skill tool|claude" .codex/skills -S`
Expected: only intentional references remain (if any).

**Step 5: Commit**

Skip (planning only).

### Task 4: Remove legacy Codex bootstrap mechanisms

**Files:**
- Delete: `.codex/superpowers-codex`
- Delete: `.codex/INSTALL.md`
- Delete: `.codex/superpowers-bootstrap.md`

**Step 1: Remove files**

Run: `rm .codex/superpowers-codex .codex/INSTALL.md .codex/superpowers-bootstrap.md`
Expected: files removed.

**Step 2: Verify removal**

Run: `ls .codex`
Expected: no legacy bootstrap files listed.

**Step 3: Commit**

Skip (planning only).

### Task 5: Update documentation for Codex-native skills + install

**Files:**
- Modify: `docs/README.codex.md`
- Modify: `README.md`
- Modify: `RELEASE-NOTES.md` (if needed)

**Step 1: Replace legacy bootstrap instructions**

Update Codex docs to explain `.codex/skills` repo-scoped skills and `$skill` invocation in Codex UI/CLI.

**Step 2: Add native install guidance**

Document that cloning the repo enables skills automatically (no bootstrap script), and that `$skill-installer` can be used for additional skills.

**Step 3: Verify docs**

Run: `rg -n "superpowers-codex|bootstrap|INSTALL.md" docs README.md -S`
Expected: no references to removed bootstrap scripts.

**Step 4: Commit**

Skip (planning only).

### Task 6: Final verification

**Files:**
- Verify: `.codex/skills/**`
- Verify: `docs/README.codex.md`
- Verify: `README.md`

**Step 1: Skill discovery sanity check (static)**

Run: `rg --files .codex/skills | head -20`
Expected: Codex-native skills present.

**Step 2: Check for legacy references**

Run: `rg -n "superpowers-codex|superpowers-bootstrap|/superpowers:" -S .`
Expected: no matches.

**Step 3: Commit**

Skip (planning only).
