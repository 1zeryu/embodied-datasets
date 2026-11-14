# Embodied AI Dataset Index

English | [简体中文](./README_cn.md)

An automatically maintained index of public embodied AI datasets, tracking data format, scale, calibration, licensing, and training usability.

- Browse: https://1zeryu.github.io/embodied-datasets/
- Changelog: https://1zeryu.github.io/embodied-datasets/log.html

## What's recorded

Each entry covers the publisher, robot models, modalities, task types, scale (episodes / hours / size), storage format and directory tree, whether camera intrinsics, extrinsics, and depth are provided, annotation availability, license, and usability judgement.

License fields are taken from the dataset's official page or repository. When a license cannot be confirmed, it is recorded as `unknown` rather than guessed. The site only shows the original license text and the resulting inference; it is not legal advice.

## Update pipeline

A scheduled task runs daily at 10:00. The procedure lives in [TASK.md](./TASK.md) and the field definitions in [schema.md](./schema.md). Only two files are written:

- `docs/data/datasets.json` — dataset records
- `docs/data/changelog.json` — per-run change log

Commits are created only when something changed, using the message `chore: daily update YYYY-MM-DD`.

## Layout

```
docs/                GitHub Pages root
├─ index.html        dataset table
├─ log.html          changelog
└─ data/             JSON data
TASK.md              task procedure
schema.md            field definitions
```

## Contributing a dataset

Open an issue or PR with the dataset name and official link. You can also edit `docs/data/datasets.json` directly; see `schema.md` for the required fields.
