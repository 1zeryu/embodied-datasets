# 具身智能数据集索引

每天自动整理公开的具身智能数据集，记录数据格式、规模、标定信息、许可证与可训练性。

- 在线浏览：https://1zeryu.github.io/embodied-datasets/
- 更新日志：https://1zeryu.github.io/embodied-datasets/log.html

## 收录内容

每条记录包含：发布机构、机器人型号、模态、任务类型、规模（轨迹数 / 小时数 / 体积）、存储格式与目录结构、相机内外参与深度是否提供、标注情况、许可证与可用性判断。

许可证信息一律来自数据集官方页面或仓库原文，查不到写 `unknown`，不做推测。页面只呈现原文与推导结果，不构成法律意见。

## 更新机制

每天 10:00 由定时任务运行一次，流程见 [TASK.md](./TASK.md)，字段定义见 [schema.md](./schema.md)。任务只修改两个数据文件：

- `docs/data/datasets.json`：数据集主数据
- `docs/data/changelog.json`：每次变更的日志

有变更时才提交，提交信息格式为 `chore: daily update YYYY-MM-DD`。没有变化则不产生提交。

## 目录说明

```
docs/                GitHub Pages 发布目录
├─ index.html        数据集列表
├─ log.html          更新日志
└─ data/             JSON 数据
TASK.md              定时任务说明
schema.md            字段定义与判定规则
```

## 补充数据集

直接提 issue 或 PR，说明数据集名称与官方链接即可。也可以手动编辑 `docs/data/datasets.json`，字段要求参见 `schema.md`。
