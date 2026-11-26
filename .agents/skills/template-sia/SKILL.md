---
name: template-sia
description: SIA (Self-Improvement Agent) harness exemplar — Meta/Target/Feedback loops, public/private splits, fixture replay, fail-closed validation.
version: 0.1.0
author: docxology
license: MIT
tags: [exemplar, sia, self-improvement, harness]
---

# template-sia

Project-scoped skill for the in-repo exemplar at
`projects/templates/template_sia/`. Load this when working inside the project.

## When to Use

- Working inside the `template_sia` exemplar — running scripts, editing source,
  or regenerating outputs.
- Forking this exemplar as the starting scaffold for a new research project.
- Validating that the exemplar's contracts (thin-orchestrator, layer boundaries,
  no-mocks testing) still hold after changes.

## Quick Reference

Use the [pinned runner and verification guide](../../../docs/quickstart.md).

## Pitfalls

- **Keep scripts thin.** Business logic belongs in `src/` or shared
  `infrastructure/`, not in `scripts/`. `src/loop.py` owns the project adapter;
  `scripts/run_sia_loop.py` only parses flags and presents result paths.
- **No mocks.** All tests must use real data, real files, and real
  computation.
- **Outputs are disposable.** Never hand-edit `output/` — regenerate from
  source and config.
- **Run from the repo root.** Commands assume the template monorepo root
  as working directory unless the child `AGENTS.md` states otherwise.

## Cross-refs

- Project contract: [`AGENTS.md`](../../../AGENTS.md)
- README: [`README.md`](../../../README.md)
- TODO: [`TODO.md`](../../../docs/TODO.md)
- Exemplar roster: [`projects/AGENTS.md`](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/projects/AGENTS.md)
