# Abstract-Fig

生成可编辑论文图形摘要和概念模型图的 Codex skill：先用 Codex 里的 image2 制作论文主题元素，拆分为独立透明 PNG，嵌入 draw.io，文字框、箭头、边框、分组和标签全部保持可编辑。

目标不是一张不可编辑的 AI 大图，而是一个能在 draw.io 里继续拖拽、改字、换元素、调版面的 `.drawio` 文件。适合投稿论文的 graphical abstract、机制概念图、方法流程图和综合示意图。

> 中文为主，English version below.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-green.svg)](SKILL.md)
[![draw.io](https://img.shields.io/badge/draw.io-editable%20.drawio-orange.svg)](https://app.diagrams.net/)

## 运行要求

只面向 Codex：完整流程依赖 Codex 里的 image2 生图能力。其他 agent 可以参考 `SKILL.md` 的流程思路，但没有 image2 或等价的生图与文件处理能力，就复现不了"生成元素 → 拆分元素 → 嵌入 draw.io"这条链。

## 使用场景

- 已有论文主线、审稿意见或修改建议，要转成正文概念图或 graphical abstract。
- 需要"图片元素 + 可编辑文字框 + 可编辑箭头"的投稿图，而不是整张贴图。
- 普通流程图太像模板，想加入含水介质、河流、农田、采样井、仪器、地貌等论文主题元素。
- 把已有草图或 draw.io 文件改成适合期刊正文的 boxed manuscript style。

## 功能

从论文内容提炼图件主线（如 `setting -> process/media -> evidence -> status/output`），判断图件类型，生图前先给出绘制方案供选择（agent 自主判断、风格菜单或自定义要求）。生成或复用 image2 元素表，把元素拆成多个透明 PNG，逐个作为独立图片对象嵌入 draw.io；科学逻辑用可编辑的文字框、分区框、箭头和标签表达。默认白底、细边框、少量配色、大字号，并检查压盖、箭头穿字、小字号、A4 缩放后不可读等常见排版问题。

交付物：一个 `.drawio` 文件 + `elements/` 透明 PNG 元素文件夹，外加嵌入检查结果和微调建议。默认不导出 PNG/PDF/SVG——先在 draw.io 里调到满意，再按期刊要求导出。

## 边界

- 不把整张 AI 生成图直接当成最终 draw.io 图。
- PNG 元素内部不能像矢量图一样逐笔编辑；可编辑的是元素位置、大小、替换关系，以及所有文字、边框、箭头和标签。
- 不凭空强化论文机制，不把概念假设画成已证实结论。
- 不替代作者对科学术语、图注、投稿格式和最终分辨率的人工复核。
- 元素质量取决于 image2 生成与拆分效果，复杂背景、阴影和细线可能要手动清理；投稿前建议导出 PDF 按 A4 或期刊栏宽再查一遍。

## 快速开始

在 Codex 里发送：

```text
请从 GitHub 安装这个 skill，并在之后需要制作论文 graphical abstract、概念模型图或可编辑 draw.io 投稿图时优先使用它：
https://github.com/keros68/abstract-fig
```

装完重启或新开窗口：

```text
使用 $abstract-fig 根据这篇论文主线做一张可编辑 draw.io 图形摘要，要求先生成主题元素，再拆分嵌入到 draw.io。
```

手动安装：

```bash
git clone https://github.com/keros68/abstract-fig.git ~/.codex/skills/abstract-fig
```

做好的 `.drawio` 文件拖进 [draw.io 官方编辑器](https://app.diagrams.net/) 即可继续编辑；网页询问保存位置时选本地存储。

## 文件结构

- `SKILL.md` - skill 主说明和触发规则。
- `references/figure-types.md` - 图件类型选择规则。
- `references/style-decision-gate.md` - 生图前的方案、风格和版式选择。
- `references/image2-element-workflow.md` - 元素生成、拆分和复用规则。
- `references/drawio-element-workflow.md` - draw.io 嵌入与编辑性要求。
- `references/boxed-manuscript-style.md` - 论文图风格约束。
- `references/research-roadmap.md` - 技术路线图模板与布局经验值。
- `references/qa-checklist.md` - A4 可读性、压盖、箭头和术语检查清单。
- `scripts/inspect_drawio_images.py` - 检查 `.drawio` 是否嵌入了多个独立图片元素。
- `agents/openai.yaml` - 兼容运行时的 UI 元数据。

## Attribution and Redistribution

This project is the original Abstract-Fig skill by keros68:

https://github.com/keros68/abstract-fig

The project is released under the MIT License. Redistribution, forks, modified versions, and repackaged copies must preserve the copyright notice and license text. Please do not present modified copies as the original project or imply endorsement by the original author.

## English

Abstract-Fig is a Codex-only skill for editable draw.io manuscript figures: graphical abstracts, concept models, mechanism diagrams, workflow figures, and synthesis figures. The workflow is element-based: generate subject-matter image elements with image2, split them into separate transparent PNGs, embed each as an individual draw.io image object, and keep all text, boxes, arrows, frames, and labels editable. Other agents can reuse the instructions as reference, but the full pipeline needs image2 or an equivalent image-generation environment.

Quick start:

```text
Install this skill from GitHub and use it for editable draw.io graphical abstracts and manuscript concept figures:
https://github.com/keros68/abstract-fig
```

Open finished files in the official draw.io editor: https://app.diagrams.net/

## License

MIT. See [LICENSE](LICENSE).

---

**同系列 Agent Skills**：[sci-select](https://github.com/keros68/sci-select)（选刊+投稿前审查） · [academic-reference-matcher](https://github.com/keros68/academic-reference-matcher)（文献引用） · [cugb-doctoral-thesis-format](https://github.com/keros68/cugb-doctoral-thesis-format)（学位论文格式） · [ai-cross](https://github.com/keros68/ai-cross)（多模型交叉验证）｜全览见 [keros68](https://github.com/keros68)
