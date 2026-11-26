# Output conventions — template_sia

| Path | Producer | Consumer |
| --- | --- | --- |
| `output/runs/run_{id}/gen_{n}/target_agent.py` | Loop (live or fixture) | Evaluation, inspection |
| `output/runs/run_{id}/gen_{n}/results.json` | `evaluate.py` | Reports, manuscript vars |
| `output/runs/run_{id}/gen_{n}/agent_execution.json` | Target step | `load_agent_execution()` |
| `output/runs/run_{id}/gen_{n}/improvement.md` | Feedback step | Context ledger |
| `output/runs/run_{id}/context.md` | Context ledger | Agent context (deterministic) |
| `output/runs/run_{id}/run_summary.json` | Loop runner | `z_generate_manuscript_variables.py` |
| `output/reports/sia_loop_report.md` | `src/reports.py` | Human review |
| `output/data/manuscript_variables.json` | Variables script | Debugging, tokens audit |
| `output/manuscript/*.md` | Injection pass | PDF renderer |
| `output/figures/` | Analysis | Four PNG plots and the accessible figure registry |
| `output/web/index.html` | Stage 03 | Combined HTML |
| `output/reports/validation_report.md`, `output/reports/rendered_provenance.json` | Stage 04 | Validation and build provenance |
| `output/pdf/template_sia_combined.pdf` | Stage 03 (`stage_03_render.py`) | Validation, publication |

The tracked `output/` tree is a publication snapshot. Regenerate the outputs
in a disposable checkout using the [build guide](quickstart.md).
