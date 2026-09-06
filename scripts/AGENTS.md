# AGENTS: `scripts/` — Thin Orchestrator Scripts

Technical specification for template_sia's script layer.

## Thin-orchestrator contract

- Scripts are orchestration only: argparse, `sys.path` bootstrap, logging, and a
  single delegated call into `src/` entrypoints (which own the Layer 1
  `infrastructure/` imports). No business/data/plot/analysis logic in scripts.
- Each script stays under 150 lines and keeps a `main()` CLI entry point.
- Reusable behavior belongs in `src/` and is tested in `tests/`; subprocess
  smoke coverage lives in `tests/test_scripts.py`.

## Script inventory

| Script | Delegates to | Output |
| --- | --- | --- |
| `run_sia_loop.py` | `src/loop.py::run_sia_loop_project()` | `output/runs/run_<id>/gen_*/`, `output/reports/`, `output/figures/`, `output/data/manuscript_variables.json` |
| `z_generate_manuscript_variables.py` | `src/reports.py::write_manuscript_variables()` + `compute_variables()`; `infrastructure.rendering.manuscript_injection.write_resolved_manuscript_tree()` | `output/data/manuscript_variables.json`, resolved `output/manuscript/*.md` |

## Gotchas

- `z_generate_manuscript_variables.py` fails fast without
  `output/runs/run_1/run_summary.json` — run `run_sia_loop.py` first.
- The LLM model comes from `project_config.sia.llm_model` in the project
  `config.yaml`, not a CLI flag; shipped empty (`llm_model: ""`) means no LLM
  feedback.
- Default mode is deterministic fixture replay from
  `src/fixtures/recorded_generations/`; `--live-sia` is the explicit opt-in for
  bounded subprocess execution and must stay credential-/service-free in
  default tests (`-m "not requires_ollama"`).
- New scripts must copy the `PROJECT_ROOT`/`REPO_ROOT` `sys.path` bootstrap
  block before importing `src.*` or `infrastructure.*`.
- `output/` is disposable — never hand-edit artifacts; regenerate via these
  scripts.
