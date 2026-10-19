# Overfitters project

Forecasting next-hour PM2.5 (`PM2_5_next_hour`) across 12 Beijing air-quality stations for the inter-university datathon.

> ## ➡️ Main code: the [`Final` branch of overfitters_pt2](https://github.com/Rishardjaf/overfitters_pt2/tree/Final)
>
> Start with the [index](https://github.com/Rishardjaf/overfitters_pt2/blob/Final/00_INDEX.md), then the [reproduction instructions](https://github.com/Rishardjaf/overfitters_pt2/blob/Final/05_README_Reproduction_Instructions.md).

```bash
git clone --branch Final https://github.com/Rishardjaf/overfitters_pt2.git
```

## Repositories

| Folder | Repository | What it is |
| --- | --- | --- |
| `current_data/` | [overfitters_pt2](https://github.com/Rishardjaf/overfitters_pt2) (public) | Revised data. **Main code is on its [`Final`](https://github.com/Rishardjaf/overfitters_pt2/tree/Final) branch.** |
| `legacy_data/` | [overfitters](https://github.com/Rishardjaf/overfitters) (private) | Original data and historical work. Reference only. |

The `current_data/` submodule here is pinned to an earlier snapshot of the repository's `main` branch, not `Final`. For the main code, use the `Final` link or clone command above.

## Why there are two repositories

During the hackathon the organisers removed the `current_PM2_5` input column from the supplied data. The target `PM2_5_next_hour` and all other columns stayed the same. Features, baselines, and results from the original data relied on that column and must be revalidated before reuse. See the [dataset release check](https://github.com/Rishardjaf/overfitters_pt2/blob/ee19b06988c9756b5c135881ed4c715cffad4f66/agent_knowledge/findings/dataset_release.md).

## Team

- [@Rishardjaf](https://github.com/Rishardjaf)
- [@eimirjihad](https://github.com/eimirjihad)
- [@nishywishy666](https://github.com/nishywishy666)
- [Keith Andre (@KeithAndre26)](https://github.com/KeithAndre26)

## Cloning this parent repository (optional)

```bash
git clone https://github.com/Rishardjaf/overfitters_project.git
cd overfitters_project
git submodule update --init current_data
```

`legacy_data` is private. Initialize it with `git submodule update --init legacy_data` only if you have access.
