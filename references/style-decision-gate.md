# Style Decision Gate

Use this reference after reading the user's manuscript content and before calling image2. The goal is to prevent image2 from defaulting to a fixed 3D/isometric look and to let the user approve the visual direction before generation.

## Core Rule

Do not call image2 immediately after receiving manuscript content unless the user explicitly says to proceed without a design check.

First provide a compact figure design brief, then ask the user to choose one of three paths:

```text
A. Agent decides based on the recommended plan and continues
B. Show style options for the user to choose
C. User provides a custom style instruction
```

If the user already specified the style and layout clearly, show a brief confirmation and proceed. If the user says "you decide", "按你判断", "直接做", or similar, choose the style yourself and continue after the brief design summary.

## Figure Design Brief

The brief should be short and in the user's language. Include:

- figure role: graphical abstract, body concept model, workflow figure, synthesis figure, or other
- core message: one sentence that states what the figure should communicate
- reading path: left-to-right, top-to-bottom, layered, comparison, cross-section, or workflow
- proposed elements: 6-12 image2 elements to generate or reuse
- recommended element style: one item from the element style menu
- recommended layout style: one item from the layout menu
- avoid list: no full-figure raster, no baked-in text, no default 3D/isometric unless selected, no tiny unreadable labels

Chinese compact template:

```text
我先不直接生图，先给你一版绘制方案。

图件类型：...
核心信息：...
阅读路径：...
拟生成元素：...
推荐元素风格：...
推荐版式：...
我会避免：整张 AI 大图、文字烘焙进图片、默认 3D/isometric、小字号。

下一步你选一个：
A. 按这个方案继续
B. 展开风格菜单让我选
C. 我自己输入风格要求
```

English compact template:

```text
Before generating images, here is the proposed figure plan.

Figure role: ...
Core message: ...
Reading path: ...
Planned elements: ...
Recommended element style: ...
Recommended layout style: ...
I will avoid: full-figure raster output, baked-in text, default 3D/isometric styling, and tiny unreadable labels.

Choose one:
A. Continue with this recommended plan
B. Show style options
C. I will provide a custom style instruction
```

## Element Style Menu

Use these options for image2 element generation. These describe visual language, not copyrighted platform assets. Do not say "BioRender style" in the image2 prompt. Use neutral descriptions such as "clean scientific vector illustration".

1. `clean scientific vector`
   - Default general option.
   - White background, clean scientific icons, consistent line weight, restrained colors.
   - Good for graphical abstracts, experimental workflows, environmental mechanisms, and biomedical-style schematic figures.

2. `soft watercolor scientific illustration`
   - Soft texture, crisp ink edge, muted natural colors.
   - Good for geology, hydrology, ecology, soil, landscape, and natural-process figures.

3. `flat schematic vector`
   - Flat vector forms, minimal shadows, clear blocks and arrows.
   - Good for method workflows, model frameworks, data pipelines, and machine-learning figures.

4. `technical line art`
   - Monochrome or low-saturation line drawing, precise outlines, minimal fill.
   - Good for serious body concept models, mechanism diagrams, and figures that should print well in grayscale.

5. `semi-realistic scientific object`
   - More concrete object rendering while keeping a white background and clean edges.
   - Good for instruments, wells, reactors, cores, bottles, field devices, and lab equipment.

6. `cross-section cutaway illustration`
   - Sectional blocks, visible internal layers, process arrows can be added later in draw.io.
   - Good for groundwater, soil, rock, river valleys, aquifers, roots, sediment layers, and subsurface processes.

7. `minimal pictogram / visual abstract icon`
   - Simple icons, low detail, high legibility.
   - Good for medical/public-health visual abstracts, 1-3 panel summaries, and social-media-facing result summaries.

8. `3D / isometric scientific blocks`
   - Optional only; do not choose by default.
   - Good for modular devices, model components, spatial blocks, and physical setups where 3D structure matters.
   - Risk: may look fixed, AI-like, or overly glossy.

Default selection:

- Use `clean scientific vector` for most manuscripts.
- Use `soft watercolor scientific illustration` or `cross-section cutaway illustration` for earth, water, soil, ecology, landscape, and environmental-process topics.
- Use `flat schematic vector` for data/model/method-heavy figures.
- Use `technical line art` when the user wants a restrained, serious, or black-and-white body figure.
- Use `3D / isometric scientific blocks` only when selected by the user or strongly justified by the object.

## Layout Style Menu

Call this "layout" rather than "overall style". It controls reading order and information architecture.

1. `three-panel graphical abstract`
   - Left-center-right story.
   - Good for journal graphical abstracts.

2. `boxed manuscript layout`
   - White canvas, claim boxes, pictorial anchors, restrained borders.
   - Good for body concept models and discussion figures.

3. `layered conceptual model`
   - Top-to-bottom layers such as setting, process, evidence, output.
   - Good for mechanism interpretation and synthesis.

4. `method workflow`
   - Inputs, processing steps, sensitivity checks, outputs, objective.
   - Good for Fig. 2-style study framework figures.

5. `comparison / contrast layout`
   - Parallel panels with same-role elements aligned.
   - Good for treatment vs control, medium A vs medium B, known vs new, before vs after.

6. `cross-section mechanism model`
   - Sectional scene plus process arrows and evidence boxes.
   - Good for hydrogeology, soil, ecology, contaminant transport, and subsurface processes.

7. `visual abstract panels`
   - One to three result panels with concise claims.
   - Good for medical, public-health, and policy-facing visual abstracts.

Default layout selection:

- Use `three-panel graphical abstract` for graphical abstracts.
- Use `boxed manuscript layout` or `layered conceptual model` for body concept models.
- Use `method workflow` for explicit method figures.
- Use `cross-section mechanism model` when the scientific object is spatially layered.
- Use `comparison / contrast layout` when the manuscript's main claim is a contrast.

## Guided Style Menu Reply

When the user chooses B, reply with a compact menu and ask for a combination such as `2 + 6`.

Chinese:

```text
可以。你选“元素风格 + 版式”即可，比如 `2 + 6`。

元素风格：
1 clean scientific vector
2 soft watercolor scientific illustration
3 flat schematic vector
4 technical line art
5 semi-realistic scientific object
6 cross-section cutaway illustration
7 minimal pictogram / visual abstract icon
8 3D / isometric scientific blocks

版式：
1 three-panel graphical abstract
2 boxed manuscript layout
3 layered conceptual model
4 method workflow
5 comparison / contrast layout
6 cross-section mechanism model
7 visual abstract panels
```

English:

```text
Choose an element style plus a layout style, for example `2 + 6`.

Element styles:
1 clean scientific vector
2 soft watercolor scientific illustration
3 flat schematic vector
4 technical line art
5 semi-realistic scientific object
6 cross-section cutaway illustration
7 minimal pictogram / visual abstract icon
8 3D / isometric scientific blocks

Layout styles:
1 three-panel graphical abstract
2 boxed manuscript layout
3 layered conceptual model
4 method workflow
5 comparison / contrast layout
6 cross-section mechanism model
7 visual abstract panels
```

## Prompt Translation

After the user chooses or accepts a style, translate it into the image2 prompt. The image2 prompt must request an element sheet, not a finished figure.

Always include:

- white or transparent-friendly background
- no text, labels, numbers, legends, watermark, or title
- separate elements with generous spacing
- consistent style across all elements
- isolated complete objects, easy to crop

Unless the user selected 3D/isometric, include:

```text
not 3D, not isometric, no glossy plastic render, no mockup lighting
```

Do not ask another style question after the user has selected A, B, or C unless generation fails or the result clearly violates the selected style.
