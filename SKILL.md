---
name: abstract-fig
description: Codex-only skill for creating editable draw.io manuscript figures in a boxed paper-figure style, using image2 subject-matter visual elements plus editable text boxes, arrows, and frames. Use required image2/generated or reused element sheets for graphical abstracts, concept models, and mechanism diagrams. Use when the user wants a thesis or paper graphical abstract, concept model, mechanism diagram, workflow figure, or synthesis figure as an editable .drawio file; when revising figures from reviewer, GPT, or Claude comments; when converting a dense figure into an element-based draw.io diagram; when a figure looks too generic, AI-like, dashboard-like, or flowchart-template-like; or when checking A4 or journal readability, label overlap, arrow semantics, and terminology strength. The full workflow assumes Codex image2 availability. Default output is a .drawio file only; do not export PNG, SVG, or PDF unless the user asks.
---

# Abstract-Fig

Build an editable draw.io manuscript figure that communicates the paper main scientific story with visual elements, short labels, and defensible arrows. The figure should be editable first: keep text, boxes, arrows, and layout as draw.io objects; use raster elements only for scientific illustrations or icons.

This skill is intended for Codex, because the complete workflow depends on image2 for generating subject-matter elements. Other agents may treat these instructions as a reference, but do not present the full workflow as agent-agnostic unless an equivalent image-generation and file-processing setup is available.

## Default Contract

- Deliver a `.drawio` file by default. Do not create final PNG, SVG, or PDF exports unless the user asks.
- Preserve the original file. Create a versioned copy or a clearly named new `.drawio`.
- Prefer fewer, larger labels over many small annotations.
- Keep scientific claims no stronger than the manuscript evidence supports.
- If the user provides review comments, convert them into visual constraints before editing.
- Use visual elements to carry subject matter, but keep labels and flow editable in draw.io.
- Default to a boxed manuscript style: white canvas, strong editable text boxes, restrained fills, thin borders, and pictorial elements as anchors rather than decoration.
- For graphical abstracts, mechanism diagrams, and body concept models, create or reuse subject-matter raster elements before building the draw.io layout. Use image2 or the available image generation tool for new elements.
- Do not silently replace required image elements with draw.io primitives. If no image generation tool is available and no reusable element set exists, stop and say the image-element requirement cannot be met in this run.
- Preserve the user's manuscript language and scientific notation in outputs. Do not downgrade Chinese labels, Greek letters, subscripts, superscripts, or chemical notation to ASCII solely for tool or platform convenience.

## Workflow

1. **Define the figure role.** Choose graphical abstract, body concept model, workflow figure, or synthesis figure. If unclear, infer from the user's wording and manuscript context; ask only when the choice changes the layout substantially.
2. **Extract the scientific spine.** Reduce the paper to 3-5 blocks such as `setting -> aquifer media/process -> evidence -> status/output`.
3. **Set the canvas.** For A4-facing wide figures, use a wide canvas near 5:2 or A4-landscape proportions. Leave margins for manual edits.
4. **Pass the image-element gate.** For a graphical abstract, mechanism diagram, or body concept model, first generate an image2 element sheet or locate reusable project elements. Record the element source. Shape-only fallback is allowed only for pure workflow figures or explicit user requests.
5. **Plan the boxed manuscript layout.** Load `references/boxed-manuscript-style.md` unless the user explicitly wants a modern infographic/dashboard style. Decide which claims belong in editable text boxes and which pictorial elements anchor them.
6. **Write short labels.** Use one heading line plus at most one supporting line per box. Put detailed explanation in the manuscript, not the figure.
7. **Build in draw.io.** Use editable text boxes, rounded rectangles, arrows, and embedded image elements. Avoid nested cards and dense legends.
8. **Run visual QA.** Check A4 readability, overlap, arrow meaning, chemical notation, unsupported process terms, and whether image2/project elements are actually embedded.
9. **Verify split-image embedding.** For graphical abstracts, mechanism diagrams, and body concept models, run `scripts/inspect_drawio_images.py <drawio> --elements-dir <elements_dir>` or perform an equivalent XML inspection. Do not call the figure done if it only embeds a whole element sheet or a single full-figure raster image.
10. **Report the `.drawio` path.** Also state whether image2 generated new elements or existing elements were reused, the number of split PNG elements, and the number of embedded image cells. Include the final handoff note. Mention remaining manual drag suggestions only if they matter.

## Figure Role Selection

Load `references/figure-types.md` when choosing or changing figure type.

Quick guide:

- **Graphical abstract:** horizontal, fast, visual, 5-10 second reading. Use three broad panels and one bottom takeaway.
- **Body concept model:** more rigorous; can show context, process domains, evidence, and interpreted outputs.
- **Workflow figure:** shows method steps, data inputs, sensitivity checks, outputs, and objective.
- **Synthesis figure:** summarizes result logic or mechanism without all method details.

## Element-Based Draw.io Rules

Load `references/drawio-element-workflow.md` when generating, replacing, embedding, or arranging visual elements.

Load `references/image2-element-workflow.md` when creating graphical abstracts, mechanism diagrams, body concept models, or any figure where a plain box-and-icon workflow would look generic or AI-like.

Load `references/boxed-manuscript-style.md` when the target is a paper graphical abstract, concept model, synthesis figure, or any figure that should resemble a journal manuscript figure rather than a slide dashboard.

Core rules:

- Keep all scientific text editable in draw.io.
- Use raster/image elements for things like aquifer media, rivers, villages, recharge, wells, redox patches, or evidence icons.
- Reuse existing clean elements when available. If new elements are needed, call image2 or the available image generation tool to create a consistent element sheet, split it into transparent PNGs, then embed them into draw.io.
- Keep the split PNG elements as separate files in an `elements` folder or clearly named equivalent. Do not only keep the original element sheet.
- Insert each useful split PNG as its own draw.io image object. Do not paste the entire generated element sheet or a full rendered figure as the main image.
- Do not settle for generic flowchart icons when the figure needs paper-specific subjects. Use image2/generated pictorial elements to carry the scientific scene, then keep text and arrows editable.
- Do not use hand-drawn draw.io mountains, clouds, rivers, trees, wells, or factories as the main subject elements for graphical abstracts or mechanism figures. Those are acceptable only as minor schematic marks inside a mostly element-based figure.
- If replacing old elements, replace all related elements from the same family so styles match.
- Embed image data in the `.drawio` so the file remains portable.
- Do not leave the final figure dependent on temporary image paths.

## Terminology Guardrails

Use conservative process language unless the manuscript directly proves the process.

Avoid over-strong terms:

- `rapid recharge` -> `recharge-linked dilution/mixing` or `valley recharge`
- `flushing` -> `dilution-mixing` unless seasonal flushing is measured
- `contaminant plume` -> `input-disturbance signal` or `conceptual solute-enrichment signal`
- `source attribution` -> `boundary conditions` or `context`
- `denitrification confirmed` -> `possible NO3 attenuation` or `redox-associated low-NO3 state`
- `diagnostic gain` / `accuracy gain` -> `rule-based separation`

Chemical notation:

- Use HTML notation such as `NO<sub>3</sub><sup>-</sup>` or consistent Unicode notation.
- Use `Cl<sup>-</sup> vs NO<sub>3</sub><sup>-</sup>/Cl<sup>-</sup>`, not a dash expression that looks like subtraction.
- Keep `Fe`, `Mn`, and `NH4` notation consistent with the manuscript.

Language and portability:

- The generated `.drawio` may contain Chinese, Unicode scientific symbols, Greek letters, and formatted chemical notation when appropriate.
- Write files in UTF-8-compatible ways and avoid assuming Windows-only paths or fonts.
- For maximum draw.io editability, prefer HTML subscript/superscript inside labels when chemical notation must survive cross-platform editing.

## A4/Journals QA

Load `references/qa-checklist.md` before calling the figure done.

Minimum QA:

- At A4 page width, all labels are readable.
- No image element covers a label, arrow, or frame.
- Arrows do not pass through text.
- Arrows point to a group when the meaning is group-level, not to one misleading output.
- Legends are optional; if used, they should not look like a note for one panel.
- The figure has a clear left-to-right or top-to-bottom reading path.
- The figure does not look like a generic slide template or AI workflow card. If it does, redesign with larger subject-matter elements and fewer boxes.
- The figure does not look like a dashboard or presentation slide. Text boxes carry the scientific claims, and images support those boxed claims.
- For graphical abstracts, mechanism diagrams, and body concept models, the file contains embedded raster subject elements generated by image2 or reused from a project element set.
- Split-image verification reports multiple embedded image cells and no single large full-figure or element-sheet image.
- The final `.drawio` is the deliverable.

## Output Naming

Use clear names:

- `graphical_abstract_<version>.drawio`
- `concept_model_<version>.drawio`
- `Fig10_ConceptualModel.drawio`
- `Graphical_Abstract.drawio`

When working in a manuscript package, place editable outputs in an `Editable_Figures` or `Graphical_Abstract` folder if present.

## Final Handoff Note

When returning the finished `.drawio`, include a short editing note in the user's language. Infer the language from the user's current request and conversation. If the user writes in Chinese, use Chinese. If the user writes in English, use English. If the conversation is mixed, use the dominant language or the language used in the latest request.

Chinese template:

```text
后续可以在 draw.io 官网继续编辑：https://app.diagrams.net/
打开网页后，如果提示选择存储位置，选本地/设备存储即可；然后把 `.drawio` 文件拖进浏览器窗口。图像元素可以继续移动、缩放和替换，文字框、箭头、分区框和标签也仍然可编辑。
```

English template:

```text
Continue editing in the official draw.io editor: https://app.diagrams.net/
Open the site, choose local/device storage if prompted, then drag the `.drawio` file into the browser window. The image elements can be moved/resized/replaced, and the text boxes, arrows, frames, and labels remain editable.
```
