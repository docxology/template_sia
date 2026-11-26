# Run the SIA exemplar

The standalone repository ships the project code, task, fixtures, and authored
manuscript. The harness and renderer come from the pinned Template checkout.

## Set up the runner

Install Python 3.15, uv, Pandoc 3.14, matching pandoc-crossref v0.3.26b, and
XeLaTeX with the [Template TeX packages](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/README.md#L565-L574).
Set `SIA_CHECKOUT` to the absolute path of this repository. From its parent
directory, run:

```sh
git clone https://github.com/docxology/template.git sia-template-runner
cd sia-template-runner
git checkout --detach 87704feae0a4490ef7dad9c931d268e8c30273c2
uv sync --locked --python 3.15
project=projects/working/template_sia
mkdir -p projects/working
git -C "$SIA_CHECKOUT" worktree add --detach "$PWD/$project" HEAD
mv "$project/output" "$(mktemp -d)/published-output"
```

Use the disposable worktree for tests and regeneration. This keeps the
tracked publication snapshot in your original checkout available for comparison.

## Verify and render

From the pinned Template checkout, use the default offline test profile,
validate the public task layout, execute fixture replay, and render.

```sh
uv run pytest "$project/tests" --cov="$project/src" --cov-fail-under=90
uv run python -m infrastructure.sia.cli validate "$project/tasks/mini_classify" --json
uv run python "$project/scripts/run_sia_loop.py"
uv run python scripts/pipeline/stage_03_render.py --project working/template_sia
uv run python scripts/pipeline/stage_04_validate.py --project working/template_sia
```

The replay produces three generations with the same held-out accuracy. The
selected run and task come from the
[configuration](manuscript/config.yaml). Inspect the [output inventory](output_conventions.md)
for run data, figures, manuscript PDF and HTML, and validation receipts.

Check authored links from the original checkout:

```sh
lychee --offline README.md STANDALONE.md AGENTS.md 'docs/**/*.md'
git diff --check
```

## Run the local reference classifier

In the disposable worktree, after retaining any replay outputs you want to
compare, run:

```sh
uv run python "$project/scripts/run_sia_loop.py" --live-sia
```

This runs the bundled reference classifier in a subprocess with the configured
timeout. The default empty `project_config.sia.llm_model` keeps it offline.
A configured model uses an available local Ollama service for feedback notes.
The harness copies the same classifier code into each live generation.
