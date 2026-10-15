# Overfitters project

Parent repository for the Overfitters PM2.5 forecasting work (Beijing multi-site air quality, 12 stations, next-hour target `PM2_5_next_hour`). It holds no code or data of its own. It pins two child repositories as Git submodules:

| Folder | Status | Repository | Pinned commit |
| --- | --- | --- | --- |
| [`current_data/`](current_data/) | **CURRENT — newer release. Start here.** | [Rishardjaf/overfitters_pt2](https://github.com/Rishardjaf/overfitters_pt2) (public) | [`ee19b06`](https://github.com/Rishardjaf/overfitters_pt2/tree/ee19b06988c9756b5c135881ed4c715cffad4f66) |
| [`legacy_data/`](legacy_data/) | **HISTORICAL — old release. Reference only.** | [Rishardjaf/overfitters](https://github.com/Rishardjaf/overfitters) (private) | [`97525ff`](https://github.com/Rishardjaf/overfitters/tree/97525ff208185a706aa69128536ac18b8432ea3c) |

```text
overfitters_project/
├── .gitmodules       Submodule names, paths, and HTTPS URLs
├── README.md         This file
├── current_data/     CURRENT: revised hackathon inputs, agent knowledge, new work
└── legacy_data/      HISTORICAL: original inputs, old EDA, models, and scores
```

## Why there are two repositories

During the hackathon the organisers changed the supplied inputs. The revised `train.csv` and `test.csv` **removed the `current_PM2_5` input column**. The target `PM2_5_next_hour` is still in the training data. The 12 stations, row IDs, timestamps, and all other columns are unchanged.

The current repository records how this was verified (checked 2026-09-06). Each revised file is identical to its old version once `current_PM2_5` is removed. The check compares row order, column names, parsed types, values, and missing values, and it includes SHA-256 fingerprints and a reproducible script:

- [Dataset release finding](https://github.com/Rishardjaf/overfitters_pt2/blob/ee19b06988c9756b5c135881ed4c715cffad4f66/agent_knowledge/findings/dataset_release.md)
- [Migration review: what old knowledge still applies](https://github.com/Rishardjaf/overfitters_pt2/blob/ee19b06988c9756b5c135881ed4c715cffad4f66/agent_knowledge/findings/previous_knowledge_review.md)
- [Causal validation policy](https://github.com/Rishardjaf/overfitters_pt2/blob/ee19b06988c9756b5c135881ed4c715cffad4f66/agent_knowledge/decisions/causal_validation.md)
- [Agent instructions](https://github.com/Rishardjaf/overfitters_pt2/blob/ee19b06988c9756b5c135881ed4c715cffad4f66/AGENTS.md) and [knowledge index](https://github.com/Rishardjaf/overfitters_pt2/blob/ee19b06988c9756b5c135881ed4c715cffad4f66/agent_knowledge/INDEX.md)

### What the change invalidates

The following work in `legacy_data/` depended on `current_PM2_5`. It is **not valid** for the revised inputs and must not be reused as a baseline or starting point:

- the persistence baseline and anything scored against it
- PM2.5 lag, rolling, cross-station, and missing-PM2.5 fallback features
- the persistence-residual model design
- trained models, cached/preprocessed parquet files, feature rankings, RMSE scores, and submissions

Some general policies still apply: the station-hour grain, the no-lookahead rule, training-only preprocessing, and chronological autumn/winter validation. Every score must be re-established on the revised schema. Do not reconstruct `current_PM2_5` from old files, shifted target labels, or external sources. See the migration review for the full applicability table.

`legacy_data/` is kept for provenance only.

## Clone

> **Placeholder:** this parent repository is **not yet hosted**. `<PARENT_REPO_URL>` below stands for its future URL. It does not exist yet.

```bash
git clone --recurse-submodules <PARENT_REPO_URL> overfitters_project
```

**Access:** `legacy_data` is a private repository, so a recursive clone needs a GitHub account with read access to `Rishardjaf/overfitters`. Without it, the `legacy_data` step fails. `current_data` is public. The parent therefore effectively needs private access.

If you cloned without `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

To get only the current release:

```bash
git submodule update --init current_data
```

After pulling parent changes that move a pin, run `git submodule update --init --recursive` again. This checks out the newly pinned commits.

## Working in a child repository

Each submodule is a normal, full Git repository with its own history, branches, and pull requests. Submodules check out a specific commit (detached HEAD), so create a branch before making changes. First confirm the submodule has no uncommitted work:

```bash
cd current_data
git status --short --branch         # must be clean before switching
git fetch origin main
git switch -c feat/your_change_name origin/main
# edit, then stage explicit paths, review git diff --cached, and use a Conventional Commit
git push --dry-run --set-upstream origin HEAD:refs/heads/feat/your_change_name
git push --set-upstream origin HEAD:refs/heads/feat/your_change_name
git rev-parse --abbrev-ref --symbolic-full-name @{upstream}
```

Inspect the dry-run output before running the real push. The destination must be `refs/heads/feat/your_change_name` on the intended remote, never `main` or `master`. After the first push, the last command must print `origin/feat/your_change_name`.

Open the pull request in the **child** repository. Follow its own [AGENTS.md](https://github.com/Rishardjaf/overfitters_pt2/blob/ee19b06988c9756b5c135881ed4c715cffad4f66/AGENTS.md) and branch conventions (`type/change_name`, underscores).

## Updating the pinned commits

The parent records one exact commit per child (a gitlink). Child pull requests don't update it automatically. Once a child change is merged to that child's `main`:

```bash
git status --short --branch         # parent must be clean
git switch -c chore/update_current_data
cd current_data
git status --short --branch         # submodule must be clean
git fetch origin main
git switch --detach origin/main
cd ..
git add current_data
git diff --cached --submodule      # confirm old -> new commit
git commit -m "chore(submodules): bump current_data to <short_sha>"
```

Make pin updates on a feature branch of the parent, not directly on `main`. Pin only commits that are published on the child's remote. Otherwise other people cannot fetch them.

## What the pins represent

The pins are the children's **published `main` commits**, verified against GitHub when this parent was created. Local edits, uncommitted work, and unpublished branches in the contributors' original checkouts were not included. That includes an in-progress merge and new modelling scripts in the old repository, and an unpublished seasonal-model branch in the new one. They remain in those original folders and reach this parent only after being published and merged into the child repositories.
