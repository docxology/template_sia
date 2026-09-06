# Scripts — template_sia

Thin orchestrators for the SIA harness exemplar. All computation lives in
`src/` (project adapter) and `infrastructure/` (Layer 1 harness); scripts only
bootstrap import paths, parse flags, and delegate.

## Inventory

| Script | Purpose | Delegates to | Run command |
| --- | --- | --- | --- |
| `run_sia_loop.py` | Run the SIA loop and write all run artifacts | `src/loop.py::run_sia_loop_project()` | `uv run python scripts/run_sia_loop.py [--project-root DIR] [--live-sia]` |
| `z_generate_manuscript_variables.py` | Persist manuscript variables and hydrate `{{SIA_*}}` tokens | `src/reports.py::write_manuscript_variables()` + `compute_variables()`; `infrastructure.rendering.manuscript_injection.write_resolved_manuscript_tree()` | `uv run python scripts/z_generate_manuscript_variables.py` |

## `run_sia_loop.py`

Runs one full loop: loads settings from the `project_config.sia` block of the
project `config.yaml` (defaults apply when absent), replays recorded fixtures
by default, then writes derived artifacts.

```bash
uv run python scripts/run_sia_loop.py             # deterministic fixture replay
uv run python scripts/run_sia_loop.py --live-sia  # bounded subprocess target
```

Flags: `--project-root DIR` (default: this project root), `--live-sia` (run the
reference agent as a bounded subprocess; optional Ollama feedback is configured
by `project_config.sia.llm_model`, never a flag).

Module logic:

- `src/loop.py` — project adapter: settings, `RunConfig` assembly, harness invocation, artifact orchestration
- `src/loop_config.py` — typed `SiaLoopSettings` from the `project_config.sia` block
- `infrastructure/sia/` — Layer 1 generation state machine

Outputs:

- `output/runs/run_<run_id>/gen_*/` — generation records (default `run_1`)
- `output/reports/sia_loop_report.md` — markdown run summary
- `output/reports/artifact_manifest.json` — sha256 manifest of registered artifacts
- `output/figures/sia_metric_progression.png`, `sia_loop_topology.png`, `sia_generation_heatmap.png`, `sia_improvement_delta.png`, `figure_registry.json`
- `output/data/manuscript_variables.json`

## `z_generate_manuscript_variables.py`

Post-analysis token hydration. Fails fast if `output/runs/run_1/run_summary.json`
is missing — run `run_sia_loop.py` first.

```bash
uv run python scripts/z_generate_manuscript_variables.py
```

Module logic:

- `src/reports.py` — `write_manuscript_variables()` / `compute_variables()` entrypoints
- `src/manuscript_variables.py` — core + metrics token computation
- `infrastructure/rendering/manuscript_injection.py` — `{{TOKEN}}` substitution and resolved-tree writer

Outputs:

- `output/data/manuscript_variables.json`
- `output/manuscript/*.md` — resolved copies with tokens substituted; auxiliary files copied verbatim
