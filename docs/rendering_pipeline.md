# Rendering pipeline

Use the [pinned runner commands](quickstart.md) to regenerate these outputs:

1. The project analysis script replays the configured fixture generations and
   writes the run summary, report, and registered figures.
2. Stage 03 invokes `scripts/z_generate_manuscript_variables.py` automatically.
   The hook uses `project_config.sia.run_id` to find the run summary, writes the
   variables JSON, and resolves placeholders from the [authored manuscript](manuscript/README.md)
   into `output/manuscript/`.
3. Stage 03 builds combined PDF and HTML with the bibliography and four figures.
4. Stage 04 validates the outputs and writes the report and provenance receipts.

## Configuration

[Manuscript configuration](manuscript/config.yaml) contains:

| Block | Keys | Effect |
| --- | --- | --- |
| `project_config.sia` | `task_name`, `run_id`, `max_generations` | Selects the task, run directory, and generation count |
| `project_config.sia` | `target_timeout_sec`, `llm_model` | Sets the live subprocess timeout and optional local feedback model |
| `project_config.sia` | `live`, `approval_mode` | Controls the default Python adapter mode and records the approval contract |
| `paper`, `publication`, `render.formats` | Title, version, DOI metadata, format toggles | Sets the paper identity and enabled output formats |

The CLI selects live execution with `--live-sia`. The default command uses
fixture replay. The manuscript reports metrics from the last run at its
configured run ID. Use replay before rendering the paper as shipped.
