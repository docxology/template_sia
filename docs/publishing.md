# Publication and forks

The [canonical configuration](manuscript/config.yaml) contains the paper
metadata, concept DOI, version DOI, and recorded publication locations. The
[example configuration](manuscript/config.yaml.example) supplies fork defaults.
Citation sidecars remain at the project root.

From the pinned runner, regenerate the README status using the explicit path:

```sh
uv run python -m infrastructure.publishing.status_report \
  --project "$project" --config "$project/docs/manuscript/config.yaml" --write
```

At the pinned revision the generator's footer still uses the legacy path.
Use the explicit command above for this checkout. The tracked
[output tree](../output/) holds the publication snapshot and should be updated
when preparing a new release.
