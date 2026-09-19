# Code Reel · 代码动画生成器

一个纯前端的「代码动画生成器」。粘贴代码 → 自动识别语言并语法着色 → 一键生成多种“代码编写过程”动画并导出视频。

整个项目只有一个 `index.html`，无任何依赖、无构建步骤、无 CDN，双击即可在浏览器中运行。

[English](#english) · [功能](#功能) · [快速开始](#快速开始) · [使用说明](#使用说明) · [浏览器兼容性](#浏览器兼容性) · [许可](#许可)

---

## 功能

- **自动语言识别**：内置词法分析器，通过加权规则自动识别粘贴内容所属语言，也可手动指定。
- **19 种语言语法着色**：JavaScript / TypeScript / Python / Java / Kotlin / C / C++ / C# / Go / Rust / Swift / PHP / Ruby / HTML / CSS / JSON / SQL / Shell / YAML，以及纯文本模式。着色由自研词法分析器完成，不依赖 highlight.js 等外部库。
- **五种动画模式**，可多选并按顺序串联成一条时间轴：
  | 模式 | 说明 |
  | --- | --- |
  | 打字机 | 逐字符输出 |
  | 分段 | 按语法片段（token）输出 |
  | 逐行 | 整行升起 |
  | 3D 视角 | 空间旋转 |
  | 平滑运镜 | 相机滞后跟随光标 |
- **清晰渲染**：使用离屏 Canvas 按屏幕像素 1:1 绘制代码，再通过网格切片仿射映射做透视变换，因此 3D 旋转时文字依然清晰；搭配 3D 网格背景与模糊投影。
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
git clone https://gitee.com/xkx121029/code-reel.git
```

## 使用说明

界面分为三栏：

- **左栏 · 代码输入**：粘贴代码，右上角显示自动识别到的语言。支持「载入示例」「清空」，也可手动指定语言。
- **中栏 · 预览舞台**：展示动画画面，下方是播放 / 暂停、重播与可拖拽的进度条。
  - 快捷键：`空格` 播放 / 暂停，`R` 重播。
- **右栏 · 参数**：
  - 动画模式（可多选，按选择顺序串联）
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
- Five animation modes (typewriter, token-by-token, line-by-line, 3D orbit, smooth camera dolly) that can be chained into a single timeline.
- Crisp text at any rotation: code is drawn 1:1 on an offscreen canvas and perspective-mapped via grid-slice affine transforms.
- 5 color themes, 3 aspect ratios (16:9 / 9:16 / 1:1), adjustable speed and font size.
- Export via `MediaRecorder` to WebM (Chrome / Edge work best).

Just open `index.html` in a modern browser. Licensed under the MIT License.

---

## 更新日志

### 2026-09-19 · 首次开源

- 首次发布 v1.0.0。
- 自研词法分析器与自动语言识别（19 种语言）。
- 五种动画模式、五套主题、三种画幅比例。
- 离屏 Canvas + 网格切片仿射映射透视渲染。
- `MediaRecorder` WebM 视频导出。
- 补充 README、MIT LICENSE 与 .gitignore。