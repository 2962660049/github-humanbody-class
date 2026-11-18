# AGENT.md — 给 AI 助手的仓库说明

本仓库包含 12 个 GLB 文件，**共 170 MB**。按需取单个文件即可。

## 文件清单

| 文件 | 部位 | 部件 | 三角面 | 大小 |
|---|---|---:|---:|---:|
| `glb/head.glb` | 头 | 581 | 1,942,276 | 38.6 MB |
| `glb/shoulder_left.glb` | 左肩 | 42 | 195,690 | 3.7 MB |
| `glb/shoulder_right.glb` | 右肩 | 42 | 192,870 | 3.7 MB |
| `glb/hand_elbow_left.glb` | 左手和肘 | 123 | 227,028 | 4.7 MB |
| `glb/hand_elbow_right.glb` | 右手和肘 | 125 | 227,322 | 4.8 MB |
| `glb/spine.glb` | 脊柱 | 139 | 420,936 | 10.1 MB |
| `glb/thorax.glb` | 胸 | 822 | 2,298,674 | 51.7 MB |
| `glb/abdomen.glb` | 腹 | 334 | 914,266 | 19.2 MB |
| `glb/pelvis.glb` | 骨盆 | 147 | 324,228 | 6.6 MB |
| `glb/hip.glb` | 髋 | 137 | 509,430 | 9.7 MB |
| `glb/knee_foot.glb` | 膝和足 | 226 | 657,022 | 12.8 MB |
| `glb/skin_body.glb` | 整体皮肤 | 1 | 203,382 | 4.7 MB |

## 坐标与单位约定（所有文件一致）

- 单位**米**，Y 轴向上，原点在人体足底高度（y≈0），人高约 1.70。
- **同一套人体全局坐标**：任意两个文件可以同时加载并自动对齐，不要各自重新居中。
- 每个 mesh 节点自带 `scale: [0.001,0.001,0.001]`（顶点数据是毫米）。
  因此不要直接读 `accessor.min/max` 当世界坐标，用 `Box3.setFromObject()`。
- 一个解剖结构 = 一个 mesh 节点，节点名是英文解剖名（`left radius`、`abdominal aorta`）。
- 跨部位重复是有意为之（同一块肌肉可能同时属于 `hip` 与 `knee_foot`），不是数据错误。

## 节点 extras

| 字段 | 说明 |
|---|---|
| `bp3d_id` | 原始元素 id，如 `FJ1932` |
| `bp3d_group` | `muscle` / `bone` / `artery` / `vein` / `nerve` / `organ` / `cartilage` / `fascia` / `other` |
| `bp3d_fma` | FMA 本体 id，可能为空 |
| `bp3d_cats` | 所属部位（逗号分隔，可能多个） |
| `bp3d_envelope` | `"1"` 表示区域包络（organ zone / anatomical set），共 126 个，**建议默认隐藏**，否则会包住模型 |

## 加载要点

1. `file://` 下不能读本地 glb → 在 glb 所在目录起本地服务器（`python -m http.server 8000`）。
2. 一次只加载 1–2 个文件；`thorax.glb` 51.7 MB，全部加载会卡死课堂机器。
3. 不要开实时阴影，两盏方向光 + 环境光足够。
4. 相机用包围盒取景，不要写死坐标（各部位尺寸差 10 倍）。

## 许可（必须保留署名）

```
BodyParts3D © ライフサイエンス統合データベースセンター licensed under CC BY 4.0
```
