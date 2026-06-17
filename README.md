# Abstract-Fig

一个用于生成可编辑论文图形摘要和概念模型图的 Codex skill：先用 Codex 里的 image2 制作论文主题元素，再拆分为独立透明 PNG，嵌入 draw.io，同时保留文字框、箭头、边框、分组和标签可继续编辑。

它适合做投稿论文里的 graphical abstract、机制概念图、方法流程图和综合示意图。目标不是生成一张不可编辑的 AI 大图，而是生成一个可以在 draw.io 里继续拖拽、改字、换元素、调版面的 `.drawio` 文件。

> 中文为主，English version below.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-green.svg)](SKILL.md)
[![draw.io](https://img.shields.io/badge/draw.io-editable%20.drawio-orange.svg)](https://app.diagrams.net/)

## 使用边界

这个 skill 目前只面向 Codex 使用。

原因很简单：完整流程依赖 Codex 里的 image2 生图能力。它不是一个通用 Claude Code / 本地 agent skill。其他 agent 可以参考 `SKILL.md` 里的流程思路，但如果没有 image2 或等价的生图与文件处理能力，就不能完整复现“生成元素 -> 拆分元素 -> 嵌入 draw.io”的流程。

## 适用场景

- 已经有论文主线、审稿意见、GPT/Claude 修改建议，想转成一张正文概念图或 graphical abstract。
- 需要“图片元素 + 可编辑文字框 + 可编辑箭头”的投稿图，而不是整张贴图。
- 觉得普通流程图太单调、太像模板，想加入含水介质、河流、农田、采样井、模型、仪器、地貌等论文主题元素。
- 需要 A4 版面内可读的白底学术图，便于后续在 draw.io 里手动微调。
- 需要把已有草图、draw.io 文件或图形摘要改成更适合期刊正文的 boxed manuscript style。

## 它做什么

- 从论文内容中提炼图件主线，例如 `setting -> process/media -> evidence -> status/output`。
- 判断图件类型：graphical abstract、正文概念模型、机制图、方法流程图或综合图。
- 生成或复用 image2 元素表，并把元素拆分成多个透明 PNG。
- 把每个元素作为独立图片对象嵌入 draw.io，方便移动、缩放、替换。
- 使用可编辑的文字框、标题、分区框、箭头和说明标签表达科学逻辑。
- 优先使用白底、细边框、少量配色和大字号，避免做成幻灯片仪表盘。
- 检查常见排版问题：压盖、箭头穿字、小字号、A4 缩放后不可读、整张 AI 图被当作唯一图片对象等。
- 完成后提示用 [draw.io 官方编辑器](https://app.diagrams.net/) 打开 `.drawio` 文件继续手动编辑。

## 不做什么

- 不把整张 AI 生成图直接当成最终 draw.io 图。
- 不承诺 PNG 图片元素内部可以像矢量图一样逐笔编辑；可编辑的是元素位置、大小、替换关系，以及所有文字、边框、箭头和标签。
- 不自动导出 PNG、PDF、SVG 或 TIFF，除非用户明确要求。
- 不凭空强化论文机制，不把概念假设画成已证实结论。
- 不替代作者对科学术语、图注、投稿格式和最终分辨率的人工复核。

## 工作流程

```text
论文主线 / 审稿意见 / 现有草图
  ↓
提炼 3-5 个图件逻辑块
  ↓
生成或复用 image2 元素表
  ↓
拆分透明 PNG 元素
  ↓
在 draw.io 中组装：图片元素 + 可编辑文字框 + 箭头 + 分区框
  ↓
检查 A4 可读性、压盖、箭头语义和元素嵌入方式
  ↓
输出可继续编辑的 .drawio 文件
```

## 使用方式

在 Codex 里，可以直接发送：

```text
请从 GitHub 安装这个 skill，并在之后需要制作论文 graphical abstract、概念模型图或可编辑 draw.io 投稿图时优先使用它：
https://github.com/keros68/abstract-fig
```

安装后，重启或新开 agent 窗口测试：

```text
使用 $abstract-fig 根据这篇论文主线做一张可编辑 draw.io 图形摘要，要求先生成主题元素，再拆分嵌入到 draw.io。
```

如果 Codex 不能自动安装 GitHub skill，可以手动 clone 到 Codex skills 目录：

```bash
# Codex
git clone https://github.com/keros68/abstract-fig.git \
  ~/.codex/skills/abstract-fig
```

Windows PowerShell 示例：

```powershell
git clone https://github.com/keros68/abstract-fig.git "$env:USERPROFILE\.codex\skills\abstract-fig"
```

没有 Codex skill loader 的环境，可以把 `SKILL.md` 当作流程参考，但不能保证完整运行，尤其是 image2 生成元素这一步。

## 输出内容

常见输出包括：

- 一个可编辑 `.drawio` 文件；
- 一个 `elements/` 文件夹，保存拆分后的透明 PNG 元素；
- 可选的元素表或元素来源说明；
- draw.io 图片嵌入检查结果；
- 简短的后续手动微调建议。

默认只交付 `.drawio`，不导出最终 PNG/PDF/SVG。这样可以先让作者在 draw.io 中把文字、箭头和位置调到满意，再按期刊要求导出。

## 继续编辑

打开 draw.io 官方编辑器：

https://app.diagrams.net/

如果网页询问保存位置，选择本地或设备存储，然后把 `.drawio` 文件拖进浏览器窗口即可继续编辑。图像元素可以移动、缩放、替换；文字框、箭头、分区框和标签仍然是 draw.io 对象。

## 文件结构

- `SKILL.md` - skill 主说明和触发规则。
- `agents/openai.yaml` - 兼容运行时的 UI 元数据。
- `references/figure-types.md` - graphical abstract、概念模型、workflow 和 synthesis figure 的选择规则。
- `references/image2-element-workflow.md` - image2 元素生成、拆分和复用规则。
- `references/drawio-element-workflow.md` - draw.io 嵌入与编辑性要求。
- `references/boxed-manuscript-style.md` - 白底、文字框、细边框的论文图风格约束。
- `references/qa-checklist.md` - A4 可读性、压盖、箭头和科学术语检查清单。
- `scripts/inspect_drawio_images.py` - 检查 `.drawio` 中是否嵌入了多个独立图片元素。

## 已知局限

- 图像元素质量取决于 Codex 中 image2 生成结果和后续拆分质量。
- 拆分透明 PNG 元素通常仍需要视觉检查；复杂背景、阴影和细线可能需要手动清理。
- draw.io 对字体和 HTML 上下标的渲染在不同系统上可能有细微差异。
- 高水平投稿前仍建议导出最终 PNG/PDF 后按 A4 或期刊栏宽检查一次。

## Attribution and Redistribution

This project is the original Abstract-Fig skill by keros68:

https://github.com/keros68/abstract-fig

The project is released under the MIT License. Redistribution, forks, modified versions, and repackaged copies must preserve the copyright notice and license text. Please do not present modified copies as the original project or imply endorsement by the original author.

## English

Abstract-Fig is a Codex-only skill for creating editable draw.io manuscript figures. It is designed for graphical abstracts, concept models, mechanism diagrams, workflow figures, and synthesis figures.

The full workflow currently depends on image2 inside Codex. Other agents may reuse the instructions as a reference, but they cannot run the complete element-generation workflow unless they provide an equivalent image-generation and file-processing environment.

The key workflow is element-based: generate or reuse subject-matter image elements, split them into separate transparent PNGs, embed each element as an individual draw.io image object, and keep all text, boxes, arrows, frames, and labels editable.

Quick start:

```text
Install this skill from GitHub and use it for editable draw.io graphical abstracts and manuscript concept figures:
https://github.com/keros68/abstract-fig
```

After installation, restart or open a new agent window and call:

```text
Use $abstract-fig to turn my manuscript idea into an editable draw.io graphical abstract with generated visual elements.
```

Open finished files in the official draw.io editor:

https://app.diagrams.net/

## License

MIT. See [LICENSE](LICENSE).
