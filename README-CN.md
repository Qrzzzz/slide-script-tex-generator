<div align="center">

# 📝 Slide Script TeX Generator

### Codex Skill：从幻灯片 PDF 与讲稿生成可编辑 LaTeX 讲义源码

[English](./README.md) · [示例](./examples) · [模板](./assets/templates) · [Skill 规范](./SKILL.md) · [安装说明](./INSTALL.md)

![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827)
![Core Output](https://img.shields.io/badge/Core%20Output-main.tex-0F766E)
![Input](https://img.shields.io/badge/Input-slides.pdf%20%2B%20script-FF5722)
![License](https://img.shields.io/badge/License-LICENCE-blue)

</div>

## 项目做什么

这个仓库提供一个 Codex Skill，核心职责只有一件事：

- **输入**：已导出的 `slides.pdf` + 逐页演讲稿
- **核心输出**：一个完整、可编辑、可编译的 `main.tex`
- **可选输出**：`main.pdf`，仅在用户或环境后续编译时产生

本 skill 不把 PDF 编译视为核心职责。

![中文概览](./assets/overview-cn.png)

## 快速开始

使用默认设置（`top-slide-manuscript`）调用：

```text
Use the slide-script-tex-generator skill.

I have slides.pdf and a page-by-page script.
Generate main.tex with the default top-slide-manuscript layout.
Only output the full main.tex source.
```

## 输入与输出

**必需输入**
- `slides.pdf`（已从 PPT/Keynote/Google Slides 导出）

**推荐输入**
- `script.md` / `script.txt` / 直接粘贴的讲稿文本

**核心输出**
- `main.tex`

**可选输出（不属于核心 skill 输出）**
- 编译后得到的 `main.pdf`

## 版式（Layouts）

### 1) `top-slide-manuscript`（默认）
- 上方大图展示当前 slide，下方展示讲稿。
- 适合排练、背稿、逐页对照。
- 支持可选的逐页自适应字号。

### 2) `left-thumbnail-clean`
- 左侧 slide 缩略图，右侧讲稿。
- 适合结构化讲义和双语内容。

### 3) `compact-review-notes`
- 更紧凑的复习排版。
- 适合快速浏览与节省打印页数。

## 讲稿切分规则

讲稿标准化优先级：
1. 按 `---` 切分
2. 按 `Slide 1`、`Page 1`、`第1页` 等标题切分
3. 按编号段落切分
4. 若无分隔符，则按 slide 页数近似切分

## 生成行为

- **讲稿段落少于 slide 页数**：为缺失页生成空白占位。
- **讲稿段落多于 slide 页数**：保留全部文本，并把超出部分追加到 `Extra Notes`。
- **空白页/分隔页**：不做静默猜测，先询问用户保留或跳过。
- **自适应字号**：仅在 `top-slide-manuscript` 且用户允许时启用。

## 可选编译（Optional compilation）

编译不属于核心 skill 输出边界。skill 的常规完成形态是 `main.tex`。

- **本地默认编译命令**：
  ```bash
  xelatex main.tex
  xelatex main.tex
  ```
- **可选 Tectonic 命令**：
  ```bash
  tectonic main.tex
  ```

## 在 Codex 中安装

快速安装：

```text
$skill-installer install https://github.com/Qrzzzz/slide-script-tex-generator
```

手动安装命令和排错说明见 [INSTALL.md](./INSTALL.md)。

## 项目结构

```text
slide-script-tex-generator/
├── SKILL.md
├── README.md
├── README-CN.md
├── INSTALL.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── LICENCE
├── assets/
│   ├── overview-en.png
│   ├── overview-cn.png
│   └── templates/
│       ├── top-slide-manuscript.tex
│       ├── left-thumbnail-clean.tex
│       └── compact-review-notes.tex
├── references/
│   ├── template-style-guide.md
│   ├── script-splitting-rules.md
│   ├── latex-compatibility-notes.md
│   └── post-generation-checklist.md
└── examples/
    ├── sample-script.md
    ├── sample-script-bilingual.md
    ├── sample-output-top-slide-manuscript.tex
    ├── sample-output-left-thumbnail.tex
    ├── sample-output-compact-review-notes.tex
    ├── sample-output-extra-notes.tex
    └── sample-output-missing-script.tex
```

## 本 skill 不做什么

- 不负责把 PPTX 转换为 PDF
- 不执行 OCR
- 不要求必须完成 PDF 编译才算任务完成
- 模板中不使用绝对路径

## 示例

查看 [examples](./examples) 目录，包含三种版式与“多段/缺段”场景的 `.tex` 示例。

## 许可证

本项目使用 [LICENCE](./LICENCE)。
