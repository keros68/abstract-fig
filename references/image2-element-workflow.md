# Image2 Element Workflow

Use this reference when a manuscript figure needs pictorial subject matter rather than a plain flowchart. This is the default path for graphical abstracts, mechanism diagrams, and body concept models unless the user asks for a shape-only method workflow.

## Required Gate

For graphical abstracts, mechanism diagrams, and body concept models, do one of these before constructing the draw.io figure:

1. Call image2 or the available image generation tool to create a new element sheet.
2. Reuse an existing project element set that already has publication-style raster elements.

If neither is possible, do not continue with a hand-drawn draw.io substitute. Tell the user that the image-element requirement cannot be met in the current run.

The final response must state the element source: `image2-generated element sheet`, `reused project elements`, or a clear skip reason.

The element sheet is only an intermediate file. It must not be used as the main image in the final draw.io figure.

## When to Use Image2 Elements

Use image2 or the available image generation tool when:

- the figure needs a visible scientific scene, object, medium, process, or status
- a generic icon set would make the figure look like a template slide
- the user says the figure is too plain, AI-like, repeated, or hard to understand
- a graphical abstract must look manuscript-ready but remain editable in draw.io

Skip image2 only when the user explicitly wants a pure vector/shape workflow, or when the figure is a simple methods flowchart where pictorial elements would distract.

Do not skip image2 merely because draw.io shapes can approximate the scene. Hand-drawn mountains, clouds, rivers, factories, trees, wells, or charts are not a substitute for required subject-matter elements in a graphical abstract or mechanism figure.

## Element Plan First

Before calling image generation, list 6-12 reusable elements:

- setting elements: field, village, river, wetland, slope, basin, coast, sampling site
- medium/process elements: porous sediment, fractured rock, soil layer, recharge, flow path, plume-like signal, redox patch
- method/evidence elements: sample bottle, well, isotope pair, ion-ratio symbol, bootstrap chart, map tile
- output/status elements: reference state, disturbance, risk, uncertainty, stable group

Keep the plan tied to the manuscript terms. Do not generate unrelated decorative art.

## Image Prompt Pattern

Generate a single element sheet, not a finished figure. Ask for no text in the image.
The prompt may use the user's manuscript language, but the generated element sheet should normally contain no baked-in text. Add Chinese, Unicode symbols, and chemical notation later as editable draw.io labels.

Template:

```text
Create a clean scientific illustration element sheet for an editable manuscript figure. 
White or flat chroma-key background, no text, no labels, no numbers, no watermark.
Consistent semi-realistic vector-watercolor style, crisp edges, soft natural colors, publication-ready, not cartoonish.
Arrange the following separate elements with generous white space between them:
1. [element]
2. [element]
...
Each element must be isolated, complete, and easy to crop into a transparent PNG.
```

For hydrogeology or environmental geochemistry, useful style words are:

```text
clean scientific cross-section, soft watercolor texture, crisp ink edge, muted colors, white background, no text
```

Avoid:

- text baked into the image
- one big combined scene that cannot be rearranged
- emoji-like icons
- hyper-realistic stock photos
- complex backgrounds
- tiny elements that will blur at A4 width

## Splitting and Cleaning

After generation:

1. Save the image in the manuscript figure working folder.
2. Split or crop each element into its own PNG. Keep these PNGs in an `elements` folder or clearly named equivalent.
3. Remove the white/chroma-key background and save transparent PNGs.
4. Create a quick preview sheet on white background to inspect all elements.
5. Fix cropped edges, colored halos, clipped arrows, or fragments from neighboring cells.

Hard requirements:

- Keep the original element sheet as provenance only.
- Keep separate cropped PNG files for the elements actually used.
- Do not embed the whole element sheet into the draw.io canvas.
- Do not render a complete final figure as one image and place it into draw.io.
- Insert each main pictorial element as a separate image object so it can be moved, resized, replaced, or deleted independently.

If using a script for chroma-key removal, cast RGB arrays to `int32` before squaring color distances.

## Draw.io Assembly

In draw.io:

- embed element PNGs as image objects
- keep all text as editable draw.io text
- keep arrows, frames, and boxes editable
- use large pictorial elements as anchors, with short labels beside them
- avoid placing text directly over detailed illustrations
- align same-role elements to a similar visual size, but allow important mechanism elements to be larger
- embed image data so the `.drawio` file is portable and does not depend on local image paths
- avoid using draw.io primitive shapes as the main visual subjects after the image-element gate has been triggered
- after assembly, inspect the draw.io XML or run `scripts/inspect_drawio_images.py` to confirm that split PNGs became multiple embedded image cells

## Quality Bar

The output should read as a manuscript figure, not a generic presentation slide. If the figure could be mistaken for a template workflow with pasted icons, redesign it with larger subject-matter elements, fewer boxes, and a clearer scientific scene.

Use the user's previous high-quality figures or element folders as style anchors when available. Reuse those elements before generating new ones.

Completion evidence to report:

- element source
- element sheet path, if generated
- split element folder path
- number of split PNG files used
- number of embedded image cells in the draw.io file
