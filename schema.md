# 数据集记录字段定义

`docs/data/datasets.json` 的唯一数据来源。每次更新必须严格遵循以下字段，不得增删字段名。

## 顶层结构

```json
{
  "meta": { "schema_version": 1, "updated_at": "YYYY-MM-DD", "count": 0 },
  "datasets": []
}
```

## datasets 数组元素

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| id | string | 是 | 唯一标识，小写英文与连字符，如 `droid`、`agibot-world` |
| name | string | 是 | 数据集正式名称 |
| org | string | 是 | 发布机构或团队 |
| source_url | string | 是 | 官方页面或论文页，用于溯源 |
| download_url | string | 否 | 下载入口，可为 HuggingFace 页面或官网 |
| platform | enum | 是 | `single_arm` `dual_arm` `humanoid` `mobile` `dexterous_hand` `multi` |
| robots | string[] | 否 | 具体机器人型号，如 `["Franka Emika Panda"]` |
| modalities | enum[] | 是 | `rgb` `depth` `joint_state` `action` `language` `tactile` `point_cloud` `audio` |
| task_types | string[] | 是 | 自由文本，如 `抓取`、`精细操作`、`导航` |
| scale | object | 是 | 见下方规模字段 |
| data_format | string | 是 | 如 `LeRobot` `RLDS` `HDF5` `MCAP` `rosbag` |
| license | string | 是 | 许可证名称原文，无法确认填 `unknown` |
| license_url | string | 否 | 许可证原文链接 |
| train_use | enum | 是 | `allowed` `research_only` `unknown` |
| commercial_use | enum | 是 | `allowed` `research_only` `unknown` |
| redistribution | enum | 是 | `allowed` `restricted` `unknown` |
| first_seen | string | 是 | 首次入库日期，格式 `YYYY-MM-DD`，已有记录不得修改 |
| last_updated | string | 是 | 数据源最后更新时间，格式 `YYYY-MM-DD` |
| summary | string | 是 | 一句话中文说明，不超过 60 字 |
| notes | string | 否 | 使用限制或注意事项 |

## scale 对象

| 字段 | 类型 | 说明 |
|---|---|---|
| episodes | number | 轨迹条数 |
| hours | number | 累计时长（小时） |
| size_tb | number | 数据量（TB），保留一位小数 |

已知任一字段即填，未知省略该字段，不得填 0 或猜测值。

## 判定规则

- `license` 必须来自数据集官方页面、论文或代码仓库的原文。查不到就写 `unknown`，不得推测。
- `train_use` 依据许可证推导。明确允许任意用途训练填 `allowed`；仅限非商业研究填 `research_only`；无明确声明填 `unknown`。
- `license` 为 `unknown` 时，`train_use` 与 `commercial_use` 必须同时为 `unknown`。
- 同一数据集以 `id` 与 `source_url` 双重去重。已存在记录只允许更新 `last_updated`、`scale`、`license` 等可能变化的事实字段。

## 更新日志 changelog.json

路径 `docs/data/changelog.json`，每次有变更时向 `entries` 数组最前面插入一条记录，格式如下。

```json
{
  "date": "YYYY-MM-DD",
  "added": [{ "id": "droid", "name": "DROID", "summary": "一句话说明" }],
  "updated": [{ "id": "agibot-world", "name": "AgiBot World", "fields": ["scale", "license"] }],
  "total": 12,
  "note": ""
}
```

规则：

- `entries` 按日期倒序，最新的一次运行在最前面。
- `added` 只记录本次新增的数据集，`updated` 只记录本次字段发生变化的数据集，`fields` 写明变了哪些字段。
- `total` 为本次运行结束后 datasets 数组的总长度。
- 本次没有任何新增或变更时，不写日志、不提交。
- `meta.entries` 等于 `entries` 数组长度，`meta.updated_at` 为最近一次产生变更的日期。

