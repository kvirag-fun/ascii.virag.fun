# 🖼️ Image to ASCII Converter

A fast, 100% client-side web tool that converts a photo — or a typed emoji — into ASCII art text.
Built with React, Vite and Tailwind CSS, deployed to **GitHub Pages** via GitHub Actions. This is a
browser-based rebuild of [img-to-text](https://github.com/kvirag-fun/img-to-text), an earlier
desktop Python version of the same idea.

Live at [ascii.virag.fun](https://ascii.virag.fun).

---

## ✨ Features

* **100% Private & Local:** All image processing runs in the browser via the HTML5 Canvas API. No
  images are uploaded to any server.
* **Two source types:** upload a photo, or just type an emoji — it's rendered onto a canvas and fed
  through the same conversion pipeline.
* **Two character sets:** **Classic** (a long ramp of ASCII characters ordered by how much ink each
  one covers) or **Blocks** (`█▓▒░`, fewer and bolder steps, safe on virtually any monospace font).
* **Light/dark aware:** an **Optimize for** toggle flips which end of the character ramp maps to
  "dense" — dark ink on a light background needs the opposite mapping from light ink on a dark one.
* **Adjustable resolution:** a width slider (30–300 columns) trades detail for output size.
* **Exact aspect-ratio correction:** rather than a rough fudge factor, the output height is scaled
  by IBM Plex Mono's actual measured glyph advance width (verified via canvas `measureText`), so
  the ASCII block's proportions match the source image instead of looking stretched.
* **Instant export:** copy the result to the clipboard (as rich, monospace-styled HTML that survives
  pasting into Word, Teams, etc., with a plain-text fallback) or download it as a `.txt` file.

## How the conversion works

`src/lib/ascii.ts` does the actual work:

1. The source image (or rendered emoji) is drawn onto an off-screen canvas at the target width, with
   height computed from the source's aspect ratio and the measured character-cell aspect ratio.
2. Each pixel's perceived brightness is computed with the same Rec. 709 luma weighting used by the
   original Python tool: `0.2126×R + 0.7152×G + 0.0722×B`.
3. That brightness (or its inverse, in dark mode) is normalized to `0–1` and used to index into the
   selected character set — `chars[Math.floor(normalized * (chars.length - 1))]` — building the
   output row by row.

## Stack

React + Vite + TypeScript + Tailwind CSS v4, styled with the shared
[`blueprint-framework`](https://github.com/kvirag-fun/virag.fun_blueprint_framework) design
system (a git submodule — see that repo's README for setup and update instructions).

## Development

```sh
bun install
bun run dev
```

## Build

```sh
bun run build
```

Deploys automatically to GitHub Pages on push to `main` via
`.github/workflows/deploy.yml`.
