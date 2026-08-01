# Abstract-Fig

Abstract-Fig 是一个 Codex agent skill，用于把论文内容做成可继续编辑的 draw.io 图件：图形摘要、正文概念模型、机制图、方法流程图、研究/技术路线图和综合示意图。

科学场景由 image2 生成的主题元素承担：元素表拆成独立透明 PNG，逐个作为图片对象嵌入；文字框、箭头、边框、分区框和标签保留为 draw.io 对象。拿到文件后仍然可以拖拽、改字、换元素、调版面。

> 中文为主，English version below.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-SKILL.md-green.svg)](SKILL.md)
[![draw.io](https://img.shields.io/badge/draw.io-editable%20.drawio-orange.svg)](https://app.diagrams.net/)

## 运行要求

完整流程依赖 Codex 里的 image2 生图能力，所以这个 skill 面向 Codex。其他 agent 可以把 `SKILL.md` 和 `references/` 当流程参考，但缺少生图与图片处理能力时，「生成元素 → 拆分元素 → 嵌入 draw.io」这条链跑不下来。遇到这种情况，`SKILL.md` 要求直接说明限制，而不是用 draw.io 基本图形凑数。

校验脚本 `scripts/inspect_drawio_images.py` 只用 Python 3 标准库，无第三方依赖。

## 使用场景

- 已有论文主线、审稿意见或修改建议，要转成正文概念图或 graphical abstract。
- 需要「图片元素 + 可编辑文字框 + 可编辑箭头」的投稿图，而不是整张贴图。
- 普通流程图太像模板，想加入含水层介质、河流、田块、监测井、仪器、地貌等论文主题元素。
- 把已有草图或 draw.io 文件改成适合期刊正文的 boxed manuscript style。

## 功能

**图件类型选型**：在图形摘要、正文概念模型、方法流程图、研究/技术路线图、综合示意图之间判断类型，每类的版式、箭头用法和信息密度要求写在 `references/figure-types.md`。研究/技术路线图按纯图形构建，默认不走 image2 元素流程，`references/research-roadmap.md` 给了阶段模板、mxCell 样式串、画布尺寸和配色的经验值。

**生图前的方案确认**：调用 image2 之前先给出一版绘制方案，包含图件类型、核心信息、阅读路径、拟生成元素、推荐元素风格和推荐版式，再让你在「按方案继续 / 展开风格菜单 / 自己写风格要求」之间选一个。菜单里有 8 种元素风格（干净科学矢量、柔和水彩、扁平示意、技术线稿、半写实对象、剖面切块、极简图标、3D 等距）和 7 种版式（三段式、正文框架、分层概念、方法流程、对比、剖面机制、视觉摘要面板）。已经给出明确风格要求或要求直接开工时跳过这一步。

**元素生成与拆分**：先按场景、介质/过程、方法/证据、输出/状态四类列出 6-12 个可复用元素，生成一张不含文字的元素表，逐个裁成透明 PNG 放进 `elements` 文件夹，再各自嵌入 draw.io。已有的干净元素优先复用。图片以 data URI 内嵌，`.drawio` 不依赖本地图片路径。元素表本身只作为中间产物保留，不整张贴进画布。

**论文风格与措辞约束**：默认 boxed manuscript style，白底、细描边、克制填色、正文倾向衬线字体，配色按角色分配（蓝为参考与补给、红橙为人为输入与超标、绿为还原与衰减、黄为不确定性、灰为背景约束）。措辞上把过强的过程判断降级，例如 `flushing` 改成 `dilution-mixing`、`denitrification confirmed` 改成 `possible NO3 attenuation`；化学式用 HTML 上下标写法；中文、希腊字母和上下标不会为了工具方便降级成 ASCII。

**交付前自检**：按 `references/qa-checklist.md` 检查 A4 页宽下的标签可读性、图压字、箭头穿字、箭头指向、术语强度，以及成图是否像通用 PPT 模板；再用校验脚本确认拆出来的 PNG 确实变成了多个内嵌图片单元。

交付物是一个 `.drawio` 文件加一个透明 PNG 元素文件夹，附带元素来源、所选风格、拆分 PNG 数量和内嵌图片单元数量。默认不导出 PNG、SVG、PDF；先在 draw.io 里调到满意，再按期刊要求导出。

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

## 校验脚本

```bash
python scripts/inspect_drawio_images.py <figure.drawio> --elements-dir <elements_dir> --min-images 3
```

脚本解析 `.drawio` 的 XML，把带 `image=` 样式的单元列成 JSON 报告：图片单元总数、内嵌与非内嵌数量、疑似整页大图的单元 ID，以及 `--elements-dir` 目录下的 PNG 数量。

`--min-images` 默认为 3。通过时退出码 0；图片单元数不足返回 2，存在非 data URI 的外链图片返回 3，单个图片宽高都达到页面尺寸的 55% 返回 4，样式串里出现 element sheet/full figure 字样返回 5；文件不存在返回 1，XML 解析失败返回 6。

## 文件结构

- `SKILL.md` - skill 主说明、默认约定和 12 步工作流。
- `references/figure-types.md` - 图件类型选择规则。
- `references/style-decision-gate.md` - 生图前的方案确认、元素风格与版式菜单。
- `references/image2-element-workflow.md` - 元素规划、生成提示词、拆分与清理规则。
- `references/drawio-element-workflow.md` - draw.io 嵌入与布局要求。
- `references/boxed-manuscript-style.md` - 论文图风格约束与角色配色。
- `references/research-roadmap.md` - 技术路线图模板、样式串与布局经验值。
- `references/qa-checklist.md` - 可读性、压盖、箭头语义、措辞和可编辑性检查清单。
- `scripts/inspect_drawio_images.py` - 检查 `.drawio` 是否嵌入了多个独立图片元素。
- `agents/openai.yaml` - 显示名、简介和默认提示词。

## 边界

- 嵌入的是 PNG 元素，元素内部不能像矢量图那样逐笔修改。可编辑的是元素的位置、大小和替换关系，以及全部文字、边框、箭头和标签。
- 元素质量取决于 image2 的生成和拆分效果，复杂背景、阴影和细线可能要手动清理边缘和残留色晕。
- 图形摘要、机制图和正文概念模型在没有生图能力时不会降级成纯图形版本，而是直接告知元素要求无法满足。
- 措辞约束只做过强表述的降级提示，不判断结论是否成立。科学术语、图注和投稿格式仍需作者复核。

## Attribution and Redistribution

This project is the original Abstract-Fig skill by keros68:

https://github.com/keros68/abstract-fig

The project is released under the MIT License. Redistribution, forks, modified versions, and repackaged copies must preserve the copyright notice and license text. Please do not present modified copies as the original project or imply endorsement by the original author.

## English

Abstract-Fig is a Codex agent skill that turns manuscript content into editable draw.io figures: graphical abstracts, body concept models, mechanism diagrams, workflow figures, research/technical roadmaps, and synthesis figures.

The workflow is element-based. Before generating anything, the skill presents a figure design brief and asks you to pick an element style and a layout style. It then generates an image2 element sheet (or reuses existing project elements), splits it into separate transparent PNGs, and embeds each one as an individual draw.io image object, while text boxes, arrows, frames, and labels stay editable. The deliverable is a `.drawio` file plus an elements folder; PNG/SVG/PDF exports are not produced unless you ask.

The full pipeline needs image2 or an equivalent image-generation setup. Other agents can read `SKILL.md` and `references/` as a process reference, but the skill is instructed to state the limitation rather than substitute plain draw.io shapes.

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
