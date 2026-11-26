# Troubleshooting

## Import errors or missing test dependencies

Run the commands from the [pinned Template checkout](quickstart.md#set-up-the-runner).
The project imports the shared harness and plotting and rendering dependencies
from that environment.

## Missing run summary

Run the replay script before Stage 03. It writes
`output/runs/run_<run_id>/run_summary.json`. Check that its run ID matches
`project_config.sia.run_id` in the
[manuscript configuration](manuscript/config.yaml).

## Task validation fails

Check that `data/public/task.md`, `data/private/`, and
`reference/reference_target_agent.py` are present. Use the CLI layout check in
the [verification commands](quickstart.md#verify-and-render) for diagnostics.

## PDF figures or tokens do not resolve

Check the four PNGs and their registry under `output/figures/`. The generated
variables and manuscript copy live in `output/data/` and `output/manuscript/`.
Rerun replay and Stage 03 to regenerate them after editing configuration or
authored sections.

## Optional model feedback is absent

The shipped model setting is empty. A configured Ollama model requires a local
service. The offline classifier runs without it and uses the configured
`target_timeout_sec` for the target subprocess.
