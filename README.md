# PaperFig

PaperFig is a Codex skill for making editable manuscript figures in draw.io.

It is designed for paper graphical abstracts, concept models, mechanism diagrams, workflow figures, and synthesis figures. The core workflow is:

```text
image2 / generated element sheet
-> split transparent PNG elements
-> embed each element into draw.io
-> keep text boxes, arrows, frames, and labels editable
```

## Install

Copy the `paperfig` folder into your Codex skills directory.

Windows PowerShell:

```powershell
Copy-Item -Recurse .\paperfig "$env:USERPROFILE\.codex\skills\paperfig"
```

macOS/Linux:

```bash
cp -R paperfig ~/.codex/skills/paperfig
```

Then start a new Codex thread and call:

```text
Use $paperfig to turn my manuscript idea into an editable boxed manuscript-style draw.io figure.
```

## What It Produces

- Editable `.drawio` files by default.
- Paper-style boxed layouts rather than slide dashboards.
- Image2/generated or reused raster elements embedded as movable draw.io image objects.
- Editable text boxes, arrows, frames, headings, labels, and chemical notation.
- A QA check for split image elements using `scripts/inspect_drawio_images.py`.

## Editing Output

Open the official draw.io editor:

https://app.diagrams.net/

Choose local/device storage if prompted, then drag the `.drawio` file into the browser window. Image elements can be moved, resized, and replaced; text boxes, arrows, frames, and labels remain editable.

## License

MIT
