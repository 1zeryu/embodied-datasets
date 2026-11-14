# Embodied AI Dataset Index

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

---

# 中文说明

## 项目简介

每天自动整理公开的具身智能数据集，记录数据格式、规模、标定信息、许可证与可训练性。

- 在线浏览：https://1zeryu.github.io/embodied-datasets/
- 更新日志：https://1zeryu.github.io/embodied-datasets/log.html

## 收录内容

每条记录包含发布机构、机器人型号、模态、任务类型、规模（轨迹数 / 小时数 / 体积）、存储格式与目录结构、相机内外参与深度是否提供、标注情况、许可证与可用性判断。

许可证信息来自数据集官方页面或仓库原文，查不到时写 `unknown`，不做推测。页面只呈现原文与推导结果，不构成法律意见。

## 更新机制

每天 10:00 由定时任务运行一次，流程见 [TASK.md](./TASK.md)，字段定义见 [schema.md](./schema.md)。任务只修改两个文件：`docs/data/datasets.json` 和 `docs/data/changelog.json`。

有变更时才提交，提交信息格式为 `chore: daily update YYYY-MM-DD`。

## 补充数据集

直接提 issue 或 PR，说明数据集名称与官方链接即可。也可以手动编辑 `docs/data/datasets.json`，字段要求参见 `schema.md`。
