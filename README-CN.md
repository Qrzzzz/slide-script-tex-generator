<div align="center">

# 📝 Slide Script TeX Generator

### 一个把幻灯片 PDF 和演讲稿转换成 LaTeX 讲义源码的轻量 Codex Skill

**Slide PDF / 演讲稿 / LaTeX 源码 / 汇报讲义 / 中英双语友好**

[English](./README.md) · [示例](./examples) · [模板](./assets/templates) · [Skill](./SKILL.md)

![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827)
![LaTeX](https://img.shields.io/badge/Output-PDF%20(via%20Tectonic)-008080?logo=latex&logoColor=white)
![PDF](https://img.shields.io/badge/Input-PDF-FF5722?logo=adobeacrobatreader&logoColor=white)
![License](https://img.shields.io/github/license/Qrzzzz/slide-script-tex-generator)

</div>

---

## 🔍 项目概览

**Slide Script TeX Generator** 是一个极简 Codex Skill，用于把已经导出的幻灯片 PDF 和演讲稿整理成一个可编辑的 LaTeX 讲义源码，并可进一步通过 Tectonic 编译为最终 PDF。

它适合学生汇报、课程展示、小组演讲、英文 presentation、教师讲义整理，以及任何需要把「PPT 页面」和「逐页讲稿」放在一起的人。

![中文概览](./assets/overview-cn.png)

---

## ✨ 默认优先流程

默认版式是 `top-slide-manuscript`。

该默认版式会在每页上方放较大的幻灯片预览，下方放对应演讲稿，适合排练、背稿和逐页对照。

在生成最终 `.tex` 之前，skill 必须先询问（除非用户已明确给出答案）：

1. 是否使用默认版式 `top-slide-manuscript`
2. 演讲稿如何切分（`---`、`Slide 1/第1页` 标题、或需按页数粗略切分）
3. 是否允许按页自适应调整讲稿字号
4. 若存在空白页/过渡页/分隔页，应如何处理

---

## 🛠️ 安装到 Codex

本项目可以作为一个 Codex Skill 使用。Codex Skill 本质上是一个包含 `SKILL.md` 的文件夹；安装后，Codex 就可以在需要把幻灯片 PDF 与演讲稿整理成 LaTeX 讲义时调用这个工作流。

### 🤖 方法一：让 Cursor 自动安装

你可以直接把下面这段话发给 Cursor，让它在你的本机环境中自动完成安装：

```text
请帮我把这个 GitHub 仓库安装为一个 Codex Skill：

https://github.com/Qrzzzz/slide-script-tex-generator

要求：
1. 将整个仓库下载或克隆到 Codex 的 skills 目录中。
2. 如果设置了 CODEX_HOME，则安装到 $CODEX_HOME/skills/slide-script-tex-generator。
3. 如果没有设置 CODEX_HOME，则安装到 ~/.codex/skills/slide-script-tex-generator。
4. 确认目标目录下存在 SKILL.md。
5. 安装完成后提醒我重启 Codex，或在 Codex 中重新打开一个新会话。
6. 不要只复制 README，要保留 SKILL.md、assets、references、examples 等目录结构。
```

安装完成后，在 Codex 中可以这样调用：

```text
使用 slide-script-tex-generator skill。

我有 slides.pdf 和 script.md。
请使用默认的 top-slide-manuscript 布局生成一份 LaTeX 讲义。
只输出最终的 main.tex 源代码。
```

或者：

```text
$slide-script-tex-generator

请根据 slides.pdf 和我的逐页演讲稿生成 top-slide-manuscript 风格的 LaTeX 讲义源码。
```

### ⚙️ 方法二：用 Codex 内置安装器

在 Codex 中输入：

```text
$skill-installer install https://github.com/Qrzzzz/slide-script-tex-generator
```

如果你的 Codex 版本要求使用 GitHub tree URL，也可以尝试：

```text
$skill-installer install https://github.com/Qrzzzz/slide-script-tex-generator/tree/main
```

安装后重启 Codex，或重新打开一个 Codex 会话。

### 💻 方法三：命令行手动安装

macOS / Linux / WSL：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"

rm -rf "${CODEX_HOME:-$HOME/.codex}/skills/slide-script-tex-generator"

git clone https://github.com/Qrzzzz/slide-script-tex-generator.git \
  "${CODEX_HOME:-$HOME/.codex}/skills/slide-script-tex-generator"

test -f "${CODEX_HOME:-$HOME/.codex}/skills/slide-script-tex-generator/SKILL.md" \
  && echo "Installed successfully. Restart Codex to use the skill."
```

Windows PowerShell：

```powershell
$CodexHome = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE ".codex" }
$SkillsDir = Join-Path $CodexHome "skills"
$SkillDir = Join-Path $SkillsDir "slide-script-tex-generator"

New-Item -ItemType Directory -Force -Path $SkillsDir | Out-Null

if (Test-Path $SkillDir) {
    Remove-Item -Recurse -Force $SkillDir
}

git clone https://github.com/Qrzzzz/slide-script-tex-generator.git $SkillDir

if (Test-Path (Join-Path $SkillDir "SKILL.md")) {
    Write-Host "Installed successfully. Restart Codex to use the skill."
} else {
    Write-Host "Installation failed: SKILL.md was not found."
}
```

### ✅ 验证安装

安装完成后，检查下面的文件是否存在：

```text
~/.codex/skills/slide-script-tex-generator/SKILL.md
```

然后重启 Codex，或开启一个新的 Codex 会话。你可以通过 `/skills` 查看 Skill 是否被识别，也可以在提示词中直接写：

```text
Use the slide-script-tex-generator skill.
```


## 🧱 从 TeX 到最终 PDF（Tectonic）

本仓库现在将 **PDF 作为最终交付物**。

典型流程：

1. 用本 skill 生成 `main.tex`
2. 使用 Codex 官方 LaTeX **Tectonic** 工具链编译
3. 交付 `main.pdf` 进行打印/复习

如果当前 Codex 环境还未安装 Tectonic，可先让 Codex 引导你安装/启用官方插件，再执行编译。

命令行兜底：

```bash
tectonic main.tex
```

编译成功后，会在同目录生成 `main.pdf`。

---

## 📦 这个项目能做什么

给定：

```text
slides.pdf
script.md / script.txt / 直接粘贴的演讲稿
```

它会生成：

```text
main.tex
main.pdf（可选，通过 Tectonic 编译）
```

生成的 LaTeX 文件会直接引用 `slides.pdf` 中的单页，例如：

```latex
\includegraphics[page=1,width=0.4\textwidth]{slides.pdf}
```

也就是说，PPT 的视觉内容仍然来自 `slides.pdf`，而演讲稿会被按页整理到对应位置，最终形成一份可编辑、可排版、可编译的 LaTeX 演讲讲义源码。

---

## 🚀 核心特性

- 根据 slide PDF 和演讲稿生成一个独立 `.tex` 文件
- 按 PPT 页面对齐演讲稿内容
- 支持 Markdown、txt、纯文本和直接粘贴的演讲稿
- 支持中文、英文和中英双语内容
- 内置 3 种高兼容性 LaTeX 模板
- 输出结果清晰、可编辑、适合作为 GitHub 开源项目维护
- 适合汇报排练、讲稿整理、打印复习和演讲准备

---

## 🧩 内置版式

### `left-thumbnail-clean`

左侧显示 PPT 缩略图，右侧显示对应页演讲稿。

适合：

- 正式汇报
- 演讲讲义
- 中英双语稿
- 逐页解释型 presentation

---

### `top-slide-manuscript`

上方显示较大的 PPT 页面，下方显示完整演讲稿。

适合：

- 背稿
- 排练
- 完整逐字稿
- 需要清楚看到每页 PPT 内容的场景

---

### `compact-review-notes`

紧凑型复习版式。

适合：

- 打印复习
- 简短讲稿
- 小组排练
- 节省页数
- 快速浏览每页讲稿

---

## 📁 推荐工作目录

建议把 `slides.pdf`、演讲稿和生成的 `main.tex` 放在同一个文件夹中：

```text
my-presentation/
  slides.pdf
  script.md
  main.tex
```

生成的 `.tex` 默认假定 `slides.pdf` 和 `main.tex` 位于同一目录。

---

## 💬 如何在 Codex 中使用

可以这样要求 Codex：

```text
Use the slide-script-tex-generator skill.

我有 slides.pdf 和下面这份演讲稿。
请使用 left-thumbnail-clean 风格生成 LaTeX 讲义。
只输出最终的 main.tex 源码。
```

也可以这样：

```text
Use the slide-script-tex-generator skill.

请根据 slides.pdf 和 script.md 生成 compact-review-notes 风格的紧凑复习版。
我的演讲稿已经用 --- 按页分隔。
```

---

## ✂️ 演讲稿切分规则

Skill 会按以下优先级切分演讲稿：

1. Markdown 分隔符：

```markdown
---
```

2. 页码标题：

```text
Slide 1
Page 1
第1页
第 1 页
P1
```

3. 明显的编号段落

4. 如果没有明确分隔符，则在已知页数的情况下尝试按页数粗略切分

Skill 会尽量保留所有用户内容。

如果演讲稿段落多于 PPT 页数，额外内容会被保留为补充备注。

如果演讲稿段落少于 PPT 页数，会为缺失页生成占位内容。

---

## ✅ 生成后检查

生成 `.tex` 后，skill 会检查一些常见问题，例如：

- PPT 页数和讲稿段落数量是否明显不匹配
- 是否缺少 `\newpage`
- PDF 文件名是否错误
- 是否残留未转换的 Markdown 语法
- LaTeX 特殊字符是否未转义
- 是否出现本地绝对路径
- 没有讲稿的页面是否有占位内容

---

## 🚫 这个项目不做什么

这个 skill 刻意保持很小的范围。

它不负责：

- PPTX 转 PDF
- 编译 LaTeX
- OCR 识别
- 安装依赖
- 创建完整工作项目文件夹
- 把 PDF 页面转换成图片

使用前，请先自己把 PPT / PPTX 导出为 `slides.pdf`。

---

## 🗂️ 项目结构

```text
slide-script-tex-generator/
  SKILL.md
  README.md
  README.cn.md
  assets/
    overview-en.png
    overview-cn.png
    templates/
      left-thumbnail-clean.tex
      top-slide-manuscript.tex
      compact-review-notes.tex
  references/
    template-style-guide.md
    script-splitting-rules.md
    latex-compatibility-notes.md
  examples/
    sample-script.md
    sample-output-left-thumbnail.tex
```

---

## 🧪 编译生成的 LaTeX 文件

Codex 生成 `main.tex` 后，把它和 `slides.pdf` 放在同一目录下，然后使用 XeLaTeX 编译：

```bash
xelatex main.tex
xelatex main.tex
```

LaTeX 编译不属于 skill 本身的工作范围。这个 skill 的职责到生成 `.tex` 源码为止。

---

## 📄 开源许可证

本项目使用 MIT License 开源。
