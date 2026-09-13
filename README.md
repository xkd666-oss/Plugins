# 小甘 图像控制与提示词一体化工作台 (XiaoGan AI Canvas & Storyboard Studio)

一个免安装、零外部构建依赖、单文件（HTML + CSS + 原生 JavaScript）的专业级 AI 图像空间控制与结构化提示词生成工作台。核心面向 Stable Diffusion、FLUX、Midjourney、ComfyUI 及各类局部重绘（Inpainting）工作流，构建起从空间标注、多参考图解耦、全视角 3D 相机、物理级多光源布光到分镜故事板批量拆解的全链路闭环。

---

## 核心特性清单

### 1. 多图层与多参考图角色体系 (Multi-Reference Architecture)

* **并行图像载入**：支持批量导入多张参考图像，每张图独立绑定原始分辨率、离屏蒙版画布（Mask Canvas）与局部对焦点标记。


* **精准角色解耦**：
* `Character Reference (人物/角色参考源)`：锁定人物面容特征、发型发色与特定服饰。


* `Background Reference (场景/环境参考源)`：锁定场景建筑空间骨架、室内外材质与环境透视。


* `Base Image (底图修改源)`：作为局部重绘与外扩的渲染基底。


* `Object Source (迁移物件参考源)`：指定特定目标物件的抠图提取来源。


* `Style / Structure Reference`：提炼美学色调或锁定几何线稿骨架。





### 2. 独立分镜故事板与剧本拆解 (Storyboard & Multi-Shot Pipeline)

* **动态网格与画幅切换**：
* 支持自由滑动或数字输入生成 $1 \sim 6$ 阶自由矩阵（如 $2\times2$、$3\times3$、$4\times4$），计算对应独立的渲染镜头总数。
* 内置 `16:9`、`4:3`、`1:1`、`3:4`、`9:16` 等常用画幅比例，预览窗实时等比响应。


* **AI 故事导演拆解引擎**：
* 专设故事剧本输入框，内置面向大语言模型（LLM）的分镜头导演拆解规则。
* 自动将自然语言故事剧本拆解为节奏分明的 $N$ 组连贯镜头清单（`Shot_01.png` ~ `Shot_N.png`），自动映射景别、角色与背景参考。


* **严格的工作流隔离**：分镜工作流设有多层独立同步开关，未启用时完全不干扰单图重绘与局部修饰提示词。

### 3. 三层复合 Canvas 绘图与智能蒙版 (Canvas Masking Engine)

* **图层隔离渲染**：底层渲染原图，中层呈现半透明红色重绘蒙版，顶层渲染笔刷半径、套索与多边形锚点辅助线。


* **多模式选取**：提供动态等比画笔（Brush）、橡皮擦（Eraser）、连续套索（Lasso）与逐点闭合多边形（Polygon）。


* **蒙版算法**：支持 30 步历史撤销（Undo/Redo）、选区一键反相（Invert）以及离屏边缘平滑羽化（Feather）。


* **二值化通道导出**：导出时将彩色蒙版自动转换为纯黑白（255 白重绘 / 0 黑保护）标准通道图。



### 4. 空间视轨与逆矩阵运算 (Viewport & Inverse Matrix)

* **多维视口变换**：中心连续平移（Pan）、平滑缩放（0.1x ~ 8.0x）、90° 步进旋转以及水平/垂直镜像翻转。


* **坐标逆矩阵反算**：在任意旋转、缩放、平移与翻转组合下，点击坐标均能反解回原图真实的像素位置与归一化百分比。



### 5. 空间对焦点标注系统 (Spatial Grounding Annotations)

* **归一化坐标标注**：以原图左上角为原点持久化记录相对百分比坐标（$nx, ny \in [0, 1]$），免受画布尺寸变化影响。


* **自然语言方位映射**：自动将物理坐标量化为摄影构图九宫格自然语言（如中心黄金分割点、左上方区域等），解决大模型难以理解纯数字绝对坐标的痛点。
* **防污染安全指令**：自动在提示词内注入系统级声明，严禁生图模型在输出成品中绘制任何 Pin 针、数字或文本标记。



### 6. 全视角工坊：3D 球面相机 (3D Camera Orbit)

* **对称坐标系**：以**正面直视平拍为绝对原点 $(0^\circ, 0^\circ)$**，水平范围 $[-180^\circ, 180^\circ]$，垂直俯仰范围 $[-90^\circ, 90^\circ]$。
* **3D 视锥与景深渲染**：Canvas 2D 绘制经纬球网格，计算前后景深（Z-sorting），呈现半透明相机投射视锥体与机身锚点。
* **景别预设**：内置中景、特写、远景、鸟瞰、低角度与荷兰角模板，并提供快捷 9 宫格方位跳转。

### 7. 光影工作室：多光源物理布光 (Lighting Studio)

* **多光源并发控制**：同一场景支持创建多盏独立光源，独立控制水平方位角、垂直仰角、光强（0% ~ 100%）与十六进制色温拾取。
* **3D 半透明光锥渲染**：在经纬球上实时渲染向中心底图靶心汇聚的体积光束锥（Cone Light），按深度分层穿插避免覆盖底图。
* **经典布光库**：内置三点布光、伦勃朗光、黄金时刻、赛博朋克双侧光、黑色电影与纯自然天光预设。

### 8. 边缘阔图与扩展 (Outpainting)

* **透明边缘外扩**：可在上下左右四个方向输入扩展像素（px），原图自动居中，扩展出来的画板区域保持**全透明通道**输出。
* **数据坐标重映射**：自动根据新画板长宽比更新蒙版尺寸与标注点的归一化位置。

### 9. 独立分设高精度导出 (Export System)

* **独立单文件导出**：支持独立导出原始底图、黑白二值重绘蒙版图以及原分辨率烧录点位的标注图。
* **项目 ZIP 归档**：一键将所有图像、二值蒙版通道、标注图合成文件、各图层的 `Image_N_Annotations.json` 以及记录全局相机与灯光配置的 `manifest.json` 统一打包。

---

## 快速上手

### 本地运行

由于本项目基于原生前端开发，完全无需配置 Node.js、npm 或 Python 环境：

1. 克隆或直接下载仓库源码：
```bash
git clone https://github.com/your-username/xiaogan-ai-studio.git

```


2. 直接使用现代浏览器（Chrome、Edge、Safari、Firefox）双击打开 HTML 文件即可：
```bash
# 或者使用本地简易静态服务打开
cd xiaogan-ai-studio
python3 -m http.server 8000

```


3. 在浏览器访问 `http://localhost:8000/`。

---

## 快捷键参考指南

在全屏画布编辑模式下，支持以下快捷键：

| 快捷键 | 功能操作 |
| --- | --- |
| `[ / ]` | 逆时针 90° / 顺时针 90° 旋转视图 |
| `\` | 一键重置视角（恢复旋转、翻转与平移） |
| `= / -` | 视图放大 / 缩小 |
| `0` | 视图自适应容器大小居中 |
| `T` | 快捷切换至“打标签（Annotate）”工具 |
| `G` | 快捷打开/收起右侧“图库（Gallery）”面板 |
| `Space + 拖拽` | 按住空格键临时切换为平移抓手模式 |
| `Esc` | 退出全屏编辑模式或关闭当前弹出的抽屉面板 |

---

## 架构概览

```text
XiaoGan AI Studio (Single-File Architecture)
├── UI & Viewport Layer
│    ├── Multi-Layer Containment Canvas (#baseCanvas)
│    ├── Interactive Inpainting Mask (#maskCanvas)
│    └── Auxiliary HUD Overlay (#tempCanvas)
├── Vector Math & Coordinate Core
│    ├── ScreenToImage Matrix Inverse Projection
│    ├── 3D Spherical Orbit (Azimuth [-180, 180], Elevation [-90, 90])
│    └── Normalized Spatial Grounding Mapping ([0, 1])
├── Feature Engine
│    ├── Storyboard Grid & Multi-Shot Batcher
│    ├── Multi-Light Cone Raymarch Preview
│    └── Transparent Canvas Outpainting Extender
└── Prompt Assembly & Exporter
     ├── LLM Narrative Shot Decomposer
     ├── Standardized Masked Transfer Assembly
     └── ZIP & Manifest Packaging (JSZip)

```

---

## 开源协议

本项目采用 [MIT License](https://www.google.com/search?q=LICENSE) 开源协议。欢迎提交 PR 或创建 Issue 提供改进建议。
