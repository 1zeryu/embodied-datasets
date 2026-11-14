# 每日更新任务说明

本文件是定时任务的权威说明。定时任务每次运行时只需读取本文件，按以下流程执行。

## 目标

维护仓库 `/Users/zhouyukun/work/codes/github/embodied-datasets` 中的具身智能数据集索引。每天检索新公开的数据集，补充字段信息，写回数据文件并推送到 GitHub。

## 涉及文件

| 文件 | 权限 | 说明 |
|---|---|---|
| `docs/data/datasets.json` | 可修改 | 数据集主数据 |
| `docs/data/changelog.json` | 可修改 | 更新日志 |
| `schema.md` | 只读 | 字段定义与判定规则，必须严格遵守 |
| 其他文件 | 禁止修改 | 包括页面、本文件 |

## 执行流程

### 1. 读取现状

读 `schema.md`、`docs/data/datasets.json`、`docs/data/changelog.json`。拿到已有记录的 id、source_url 和字段完整度。

### 2. 检索新数据集

检索近 30 天内新公开或首次发布的数据集。来源优先级从高到低：

1. HuggingFace 上 `lerobot` 组织及带 `embodied`、`robot`、`manipulation` 标签的数据集
2. 已知项目的官方页面与论文：Open X-Embodiment、DROID、AgiBot World、RoboMIND、RH20T、RoboTwin、BridgeData、RT-X
3. GitHub 上具身智能数据相关的开源仓库

### 3. 采集字段

**每一个字段都必须来自可访问的原文**，优先顺序为：数据集卡片的 README → 数据集内的 `meta/info.json` 等元数据文件 → 论文正文或附录 → 项目官网。查不到就填 `unknown`，不得凭常识推断。

除基础字段外，重点补充以下四组信息。

#### 数据格式 `format_detail`

打开数据集文件列表，确认实际存储结构：

- `container`：如 `Parquet + MP4`、`TFRecord`、`HDF5`、`MCAP`、`rosbag`、`Zarr`
- `fields`：数据集中实际存在的键名，如 `observation.images.cam_high`、`observation.state`、`action`、`task`、`next.reward`
- `fps`：采集帧率
- `resolution`：主相机分辨率，如 `640x480`
- `video_codec`：如 `h264`、`hevc`、`raw`

#### 目录结构 `tree`

用官方文件列表生成目录树，格式与限制见 `schema.md`。这一步要排在采集其他字段之前，因为它能直接反映数据真实的组织方式，也决定了后面标定信息能不能查到。

HuggingFace 数据集用 `https://huggingface.co/api/datasets/{repo}/tree/main?recursive=true` 拿到完整文件列表，再按三层裁剪。列目录时重点留意有没有 `camera_info`、`calibration`、`extrinsics`、`depth` 这类目录或文件。

#### 标定信息 `calibration`

判断数据集是否提供相机内外参与深度：

- `intrinsics`：是否提供相机内参（焦距、主点、畸变系数）
- `extrinsics`：是否提供外参（相机与机器人基座或末端之间的变换矩阵）
- `depth`：是否包含深度图
- `eef_pose`：是否提供末端执行器位姿序列

判定依据是数据集里实际存在的标定文件（如 `camera_info.json`、`calibration.json`、`extrinsics.npy`）或 README 明确描述。只有文档提到但文件里没有，填 `not_provided`。完全找不到相关描述，填 `unknown`。

#### 标注信息 `annotation`

- `language`：是否有自然语言任务描述
- `subtask`：是否有子任务分段标注
- `success`：是否标注成功或失败

#### 传感器与本体 `sensors`

- `cameras`：相机数量
- `views`：视角类型，取值 `third_person`、`wrist`、`ego`、`overhead`
- `proprioception`：是否有本体状态（关节角、夹爪开合）
- `tactile`：是否有触觉信号

`robots` 字段要写具体型号，如 `Franka Emika Panda`、`UR5e`、`ALOHA`、`Unitree G1`、`xArm 7`。只知道机器人厂商或类别、不知道具体型号时，写能确认的最细粒度，并在 `notes` 中说明。

### 4. 去重与更新

按 `id` 和 `source_url` 双重去重。已存在的记录不重复新增，`first_seen` 永远不改。只有事实确实变化时才更新字段，并在日志里记录变更了哪些字段。

### 5. 补齐旧记录

对已存在但缺少 `format_detail`、`calibration`、`annotation`、`sensors` 的记录，同样按第 3 步的规则补全已验证的信息。补不到的填 `unknown`，避免反复重试。

### 6. 写日志

有新增或变更时，向 `changelog.json` 的 `entries` 最前面插入一条记录，格式见 `schema.md`。无任何变化则不写日志。

### 7. 提交推送

```bash
git add docs/data/datasets.json docs/data/changelog.json
git commit -m "chore: daily update YYYY-MM-DD"
git push origin main
```

无变化时不提交、不推送。

## 禁止事项

- 不得臆造许可证、标定信息、机器人型号、数据规模。所有内容必须能追溯到原文链接。
- 不得修改除两个数据文件以外的任何文件。
- 不得删除已有记录，除非官方页面已确认下线，并在日志 `note` 中说明。
- `license` 为 `unknown` 时，`train_use`、`commercial_use` 必须同时为 `unknown`。

## 结束报告

用不超过 200 字报告：新增数量、更新数量、补齐字段的旧记录数量、跳过的条目及原因、push 是否成功。若 push 失败，保留本地提交并说明原因，不要回滚。
