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
  | | | 倾斜 | 斜切入正 |
- **关键帧时间轴**：舞台下方是四通道关键帧编辑器 —— 速度（0.25~3×）、缩放（0.6~1.6×）、水平旋转与垂直旋转（**不设角度上限**，轨道显示范围会随数据自动外扩）。顶部片段条标出每段的「出现方式 · 镜头运动」。
  - **基础版（自动编排）**：一键把当前「出现方式 × 镜头运动」翻译成整套关键帧 —— 速度按各出现方式的打字手感在段内交替「冲刺—放缓」（段越长脉冲越多，段尾收住），缩放按运动语义从段首走到段末（推近就一直推、段与段首尾相接成一次连贯运镜），环绕 / 倾斜段从头扫到段末（倾斜保持「开头即斜、随后归正」）。改代码或改选择即重算，不用手动打点。
  - **拖动画布视角即写入**：把画面拖到满意的角度松手，这段角度就固化成旋转关键帧（同时写入水平与垂直旋转），临时视角随之清零，画面不会有任何跳变。**拖动不设角度上限**，可以一路转到任意角度（包括转过头看背面）。点轨道上的点或空白先选一个时刻，就写到那一刻；基础版没选点时自动落在当前时刻。整片此前没有旋转关键帧的话，会先按段采样一次原运镜基线，避免一个点把整片角度都带跑。
  - **高级版（手动打点）**：双击轨道空白加关键帧、拖动同时改时机与数值（跟浮动数值气泡）、双击关键帧删除、拖标尺定位；从基础版切换过去会保留那批自动关键帧作为起点。手动点（含拖视角写入的）在基础版重算时会被保留，不会被自动编排覆盖。
  - **吸附**：拖动关键帧、点轨道选点、拖标尺都会自动贴到有意义的位置 —— 时间贴片段起点 / 打字结束 / 片段结束、播放头、时间刻度、其他通道的关键帧；数值贴通道基准值（1× / 1× / 0°）与同通道其他关键帧的值。阈值 7px、已吸住后放宽到 12px（不会在边缘抖），吸附时有参考线提示，点「吸附」按钮或按住 `Alt` 可关闭。
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
- Two independent dimensions combined into one playback sequence: reveal style (typewriter / word / line / syntax block) × camera move (still / push in / pull out / orbit / tilt).
- A keyframe timeline under the stage with four channels: speed (0.25–3× time remapping), zoom (0.6–1.6×), yaw and pitch (no angle limit — the track range expands automatically to fit the data). Two modes: **Basic** auto-writes the whole arrangement from the current reveal × camera move selections (recomputed whenever the code or selection changes), **Pro** lets you double-click a track to add a keyframe, drag to retime or revalue it, double-click a keyframe to delete, and drag the ruler to scrub. Switching Basic → Pro keeps the generated keyframes as a starting point.
- **Drag the canvas to write keyframes**: release the mouse and the angle you dragged to is baked into the yaw/pitch keyframes, the temporary view resets, and the picture does not jump. Click a keyframe or empty track first to choose the moment it is written to.
- **Snapping** (on by default, toggle in the timeline header, `Alt` to bypass): times snap to clip start / typing end / clip end, the playhead, ruler ticks and keyframes of the other channels; values snap to the channel baseline (1× / 1× / 0°) and to other keyframes of the same channel. 7px engage radius, 12px release radius, with guide lines while snapped.
- Crisp text at any rotation: code is drawn at device-pixel scale on an offscreen canvas and perspective-mapped by WebGL (no slice seams), with a 2D fast path for unrotated 1:1 shots.
- 5 color themes, 3 aspect ratios (16:9 / 9:16 / 1:1), adjustable speed and font size.
- Export via `MediaRecorder` to WebM (Chrome / Edge work best).

Just open `index.html` in a modern browser. Licensed under the MIT License.

---

## 更新日志

### 2026-09-19 · 删掉扫描擦除与聚焦

- 出现方式收敛为四种：打字机、逐词、逐行、语法块（与镜头运动组合出 20 种片段）。
- 移除对应的绘制分支（横向裁剪擦除、模糊聚焦）与自动编排里这两档的速度骨架；每行「还没开始就跳过」的早退逻辑保留。

### 2026-09-19 · 视角角度不再受限

- 移除画布拖动的角度上限：`rotX / rotY` 都不再夹取，可以一路转到任意角度（过头会看到平面背面），只有滚轮调距离仍受限（距离不能穿过相机）。
- 移除打点时的 ±40° 截断：拖出来的角度原样写进关键帧，不再出现"拖到一半被卡住、松手画面跳一下"。
- 旋转轨道显示范围改为**随数据自动外扩**（基准 ±40°，出现更大角度时按 10° 一档扩到刚好包住 + 余量），所以超大角度依然看得见、曲线照常绘制。
- 轨道拖动不再夹取：指针拖到轨道外会继续取值，配合自动外扩的范围可以一路拖到大角度。
- 拖动期间冻结轨道范围，避免指针位置与数值的对应关系在拖动中跳变。

### 2026-09-19 · 关键帧吸附

- 时间吸附：拖动关键帧、基础版选点、拖标尺定位时，自动贴到片段起点 / 打字结束 / 片段结束、播放头（播放中除外）、时间刻度、其他通道的关键帧；同通道的关键帧不参与，避免两点叠在同一时刻。
- 数值吸附：贴到通道基准值（匀速 1×、不缩放 1×、不旋转 0°）与同通道其他关键帧的值，方便对齐多个点的角度。
- 阈值 7px，已经吸住后放宽到 12px 再脱开，避免在阈值边缘来回抖；吸附时画出参考线（时间竖线 / 数值横线），松手即消失。
- 时间轴头部新增「吸附」开关（默认开启），按住 `Alt` 拖动可临时关闭。
- 不做数值步进网格吸附：轨道只有 17px 高，旋转 1° 只占 0.2px，网格会比手还密、等于强制吸附。

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