# omni-ari-collector

An Omnibenchmark metric collector for the
[omni-pipelines-plan](https://github.com/sta426hs2026/omni-pipelines-plan)
benchmark. It runs once after every `metrics` module has finished, gathers all
`*.metrics.json` files, and works out which dataset, method and parameters
produced each one from the `parameters.json` files along its output path.

## Outputs (in `out/ari-collector/`)

| File | Contents |
|---|---|
| `ari_summary.tsv` | One row per run: label, ARI (Leiden, Louvain), cluster counts, all parameters, path to the source `metrics.json` |
| `all_metrics.tsv` | Every agreement metric (ARI, NMI, ...) in long format |
| `ari_barplot.png` | ARI per run, Leiden and Louvain side by side |

Run labels show only the parameters that differ between runs, so adding
`n_comp: [30, 50]` produces labels like `osca (n_comp=50)`. New methods
(e.g. a random baseline) are picked up automatically.

## Plan stanza

```yaml
metric_collectors:
  - id: ari-collector
    name: ARI summary across all runs
    software_environment: collector
    repository:
      url: https://github.com/<your-user>/omni-ari-collector
      commit: <commit-hash>
    inputs:
      - metrics.metrics
    outputs:
      - id: ari.summary
        path: "{input}/{name}/ari_summary.tsv"
      - id: ari.all_metrics
        path: "{input}/{name}/all_metrics.tsv"
      - id: ari.barplot
        path: "{input}/{name}/ari_barplot.png"
```

It needs a `collector` software environment with pandas and matplotlib
(see `envs/collector.yml` in the plan repo).
