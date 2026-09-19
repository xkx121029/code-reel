# Code Reel · 代码动画生成器

一个纯前端的「代码动画生成器」。粘贴代码 → 自动识别语言并语法着色 → 一键生成多种“代码编写过程”动画并导出视频。

整个项目只有一个 `index.html`，无任何依赖、无构建步骤、无 CDN，双击即可在浏览器中运行。

[English](#english) · [功能](#功能) · [快速开始](#快速开始) · [使用说明](#使用说明) · [浏览器兼容性](#浏览器兼容性) · [许可](#许可)

---

## 功能

- **自动语言识别**：内置词法分析器，通过加权规则自动识别粘贴内容所属语言，也可手动指定。
- **19 种语言语法着色**：JavaScript / TypeScript / Python / Java / Kotlin / C / C++ / C# / Go / Rust / Swift / PHP / Ruby / HTML / CSS / JSON / SQL / Shell / YAML，以及纯文本模式。着色由自研词法分析器完成，不依赖 highlight.js 等外部库。
- **两个维度自由组合**：出现方式 × 镜头运动，取笛卡尔积串联成播放序列（上限 12 段）。

  | 出现方式 | 说明 | 镜头运动 | 说明 |
  | --- | --- | --- | --- |
  | 打字机 | 逐字符输出 | 静止 | 不运镜 |
  | 逐词 | 按语法片段输出 | 推近 | 缓推 |
  | 逐行 | 整行升起 | 拉远 | 缓收 |
  | 语法块 | 按语法块出现 | 环绕 | 空间旋转 |
  | 扫描擦除 | 横向擦开 | 倾斜 | 斜切入正 |
  | 聚焦 | 由虚到实 | | |
- **关键帧时间轴**：舞台下方是四通道关键帧编辑器 —— 速度（0.25~3×）、缩放（0.6~1.6×）、水平旋转与垂直旋转（±40°）。顶部片段条标出每段的「出现方式 · 镜头运动」。
  - **基础版（自动编排）**：一键把当前「出现方式 × 镜头运动」翻译成整套关键帧 —— 速度按各出现方式的打字手感在段内交替「冲刺—放缓」（段越长脉冲越多，段尾收住），缩放按运动语义从段首走到段末（推近就一直推、段与段首尾相接成一次连贯运镜），环绕 / 倾斜段从头扫到段末（倾斜保持「开头即斜、随后归正」）。改代码或改选择即重算，不用手动打点。
  - **拖动画布视角即写入**：把画面拖到满意的角度松手，这段角度就固化成旋转关键帧（同时写入水平与垂直旋转），临时视角随之清零，画面不会有任何跳变。点轨道上的点或空白先选一个时刻，就写到那一刻；基础版没选点时自动落在当前时刻。整片此前没有旋转关键帧的话，会先按段采样一次原运镜基线，避免一个点把整片角度都带跑。
  - **高级版（手动打点）**：双击轨道空白加关键帧、拖动同时改时机与数值（跟浮动数值气泡）、双击关键帧删除、拖标尺定位；从基础版切换过去会保留那批自动关键帧作为起点。手动点（含拖视角写入的）在基础版重算时会被保留，不会被自动编排覆盖。
  - 速度轨道是全局时间轴曲线，但在每段打字区间内按面积归一化：变速只重新分配快慢，不会让哪段代码写不完。
  - 缩放与旋转通道有关键帧时接管相机，没有关键帧的通道继续走镜头运动预设。
- **清晰渲染**：离屏 Canvas 按设备像素 1:1 绘制代码，再交给 WebGL 做真透视贴图（透视插值由硬件保证，旋转时无拼接接缝）；无旋转且 1:1 时走单次 2D 贴图快路径，像素级锐利。搭配 3D 网格背景与投影轮廓模糊填充的阴影。
- **中日韩文本与表情处理**：按双宽字符计算，光标与列定位不会错位。
- **可调外观**：5 套配色主题（午夜石墨 / 深海回声 / 苔原 / 铅印 / 信号）、三种画幅比例（16:9 / 9:16 / 1:1）、速度与字号可调、行号与窗口边框可开关。
- **时间轴可控**：播放条可拖拽定位；渲染是时间的纯函数，暂停与拖动后画面保持一致。
- **视频导出**：通过 `MediaRecorder` 实时录制画布并下载 WebM 视频。

## 快速开始

无需安装任何东西：

1. 下载或克隆本仓库。
2. 用浏览器打开 `index.html`（双击即可）。
3. 在左侧文本框粘贴代码，页面会自动识别语言并着色。
4. 选择动画模式与主题，点击播放预览。
5. 需要成片时点击「导出视频（WebM）」，录制结束后浏览器会自动下载文件。

```bash
git clone https://github.com/xkx121029/code-reel.git
# 或
git clone https://gitee.com/xkx1029/code-reel.git
```

## 使用说明

界面分为三栏：

- **左栏 · 代码输入**：粘贴代码，右上角显示自动识别到的语言。支持「载入示例」「清空」，也可手动指定语言。
- **中栏 · 预览舞台**：展示动画画面，下方是关键帧时间轴与播放条（播放 / 暂停、重播、可拖拽的进度条）。
  - 快捷键：`空格` 播放 / 暂停，`R` 重播。
  - 时间轴四条轨道自上而下为速度 / 缩放 / 水平旋转 / 垂直旋转；时间轴坐标与播放条共用同一个播放位置。
  - 时间轴左上角可在 **基础版**（自动编排）与 **高级版**（手动打点）之间切换；右侧按钮分别是「重新生成」与「清空关键帧」。
  - 基础版下点轨道上的点或空白即选中一个时刻（播放头同步过去，`Esc` 取消），随后在画面上拖动视角就会把角度写到那一刻；双击已有点可删除。
- **右栏 · 参数**：
  - 出现方式与镜头运动（各自可多选，组合成播放序列）
  - 主题、画幅比例、字号、速度
  - 行号、窗口边框、投影开关
  - 导出按钮

调整参数后画面会自动重建，无需刷新。

## 浏览器兼容性

- 需要支持 Canvas 的现代浏览器（Chrome / Edge / Firefox / Safari 的近期版本均可运行）。
- **视频导出依赖 `MediaRecorder` 与 WebM 编码**：Chrome 与 Edge 支持最好（优先 VP9，其次 VP8）；Firefox 亦可。Safari 对 WebM 录制支持有限，导出功能可能不可用，但预览动画不受影响。
- 页面会优先使用 `video/webm;codecs=vp9`，不支持时自动回退到 `vp8` 或普通 `video/webm`；若浏览器完全不支持 WebM 录制，会给出提示。

## 目录结构

```
code_video/
├── index.html    # 全部功能：词法分析、渲染、动画、导出
├── LICENSE
├── .gitignore
└── README.md
```

## 贡献

欢迎提交 Issue 与 Pull Request：

1. Fork 本仓库并创建分支（如 `feature/xxx`）。
2. 保持单文件、零依赖、零构建的形态。
3. 提交前请在不同浏览器中确认预览与导出功能正常。
4. 提交 PR 并说明改动目的与验证方式。

## 许可

本项目基于 [MIT License](LICENSE) 开源。

## 联系方式

- 作者：xkx121029
- 邮箱：xkx121029@qq.com

---

## English

**Code Reel** is a single-file, dependency-free, build-free front-end tool that turns pasted code into animated "typing" clips.

- Paste code → automatic language detection → syntax highlighting, powered by a small hand-written lexer (no highlight.js / no CDN).
- 19 languages supported, plus plain text.
- Two independent dimensions combined into one playback sequence: reveal style (typewriter / word / line / syntax block / scan wipe / focus) × camera move (still / push in / pull out / orbit / tilt).
- A keyframe timeline under the stage with four channels: speed (0.25–3× time remapping), zoom (0.6–1.6×), yaw and pitch (±40°). Two modes: **Basic** auto-writes the whole arrangement from the current reveal × camera move selections (recomputed whenever the code or selection changes), **Pro** lets you double-click a track to add a keyframe, drag to retime or revalue it, double-click a keyframe to delete, and drag the ruler to scrub. Switching Basic → Pro keeps the generated keyframes as a starting point.
- **Drag the canvas to write keyframes**: release the mouse and the angle you dragged to is baked into the yaw/pitch keyframes, the temporary view resets, and the picture does not jump. Click a keyframe or empty track first to choose the moment it is written to.
- Crisp text at any rotation: code is drawn at device-pixel scale on an offscreen canvas and perspective-mapped by WebGL (no slice seams), with a 2D fast path for unrotated 1:1 shots.
- 5 color themes, 3 aspect ratios (16:9 / 9:16 / 1:1), adjustable speed and font size.
- Export via `MediaRecorder` to WebM (Chrome / Edge work best).

Just open `index.html` in a modern browser. Licensed under the MIT License.

---

## 更新日志

### 2026-09-19 · 拖动画布视角即写入关键帧

- 基础版允许选点：点轨道上的关键帧或空白即选中一个时刻（播放头同步过去，选中的点与时刻有高亮圆环和虚线标记，`Esc` 取消），拖标尺定位会取消选点。
- 拖动画布视角后自动打点：松手即把这段角度固化进水平 / 垂直旋转关键帧，同时写入两个通道，随后临时视角清零 —— 关键帧接过角度，画面不跳变；连续拖动会叠加到同一点上。
- 有选点时写到选点那一刻；基础版没选点时自动落在拖动开始时的播放头时刻，高级版没选点则只保留临时视角不写入。
- 旋转通道原本为空（由镜头运动预设驱动）时，写入前先按段采样一次基线锚点，避免一个点把整片角度都带跑；旋转通道值域放宽到 ±40° 以容纳拖动范围，超出时截断并提示。
- 基础版重算（改代码 / 改选择）会保留用户手动打下的点，只重排自动点；基础版双击已有点可删除。
- 右栏「视角」说明同步更新。

### 2026-09-19 · 基础版自动编排 / 高级版手动打点

- 时间轴分成两种模式：**基础版**（默认）按当前「出现方式 × 镜头运动」自动生成整套四通道关键帧，轨道只读、改代码即重算；**高级版**保留原有手动打点方式，切过去会带着自动生成的那批关键帧继续微调。
- 自动编排规则：速度在段内按周期交替「冲刺—放缓」并收住段尾；缩放从段首走到段末，相邻段首尾取值相接（推近 1.00→1.10 接拉远 1.10→1.00）；环绕 / 倾斜段从头扫到段末，其余段给归位锚点；整片没有空间运动时旋转通道留空，继续走镜头运动预设。
- 时间轴左上角新增模式切换，右侧主按钮随模式变为「重新生成」/「清空关键帧」；基础版下点击轨道会提示切到高级版。
- 手动求值与自动编排共用同一套关键帧求值器与渲染路径，两种模式画面逻辑完全一致。

### 2026-09-19 · 关键帧时间轴

- 新增舞台下方四通道关键帧时间轴：速度 / 缩放 / 水平旋转 / 垂直旋转，带时间标尺、片段条与播放头。
- 交互：双击轨道加关键帧、拖动同时改时机与数值、双击关键帧删除、拖标尺定位并与播放条同步。
- 速度轨道按片段打字区间做面积归一化：变速只重分配快慢，保证每段代码都在段内写完。
- 缩放与旋转关键帧直接接管相机；无关键帧的通道继续走镜头运动预设。
- 移除原先嵌在右栏的小型速度曲线编辑器，速度系统统一到时间轴。
- 新增 3D 视角（拖拽转角度、滚轮调远近）、随光标跟随、自动运镜、曲线变速与投影阴影等能力。

### 2026-09-19 · 首次开源

- 首次发布 v1.0.0。
- 自研词法分析器与自动语言识别（19 种语言）。
- 五种动画模式、五套主题、三种画幅比例。
- 离屏 Canvas + 网格切片仿射映射透视渲染。
- `MediaRecorder` WebM 视频导出。
- 补充 README、MIT LICENSE 与 .gitignore。