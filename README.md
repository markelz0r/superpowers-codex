# Superpowers (Codex Fork)

This repository is a fork of the original **Superpowers** library by obra, fully ported for **Codex**. It keeps the same core workflow and skills system, but ships in a native Codex format and setup.

Credit and original project: https://github.com/obra/superpowers

## What it is

Superpowers is a software development workflow for coding agents, built on composable skills and starter instructions that enforce consistent, high‑quality process.

## Install

```bash
$skill-installer https://github.com/markelz0r/superpowers-codex
```

Restart Codex to pick up the skills.

## Required experimental flags

For `subagent-driven-development`, enable **all** `/experimental` features in Codex:

```
› [x] Background terminal
  [x] Shell snapshot
  [x] Powershell UTF-8 support
  [x] Multi-agents
  [x] Steer conversation
```

## How it works (short)

1. **brainstorming** → clarify intent, explore options, produce a readable design.
2. **writing-plans** → break the design into explicit, testable steps.
3. **subagent-driven-development** or **executing-plans** → implement with reviews.
4. **test-driven-development** → enforce RED‑GREEN‑REFACTOR.
5. **verification-before-completion** → prove it actually works.

## License

MIT. See `LICENSE`.