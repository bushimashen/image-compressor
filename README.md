# Image Compressor · 图片压缩

> Batch-compress images in the browser. Smart format choice, target-size binary search, dimension caps, and automatic white backing for transparent images — nothing is uploaded.

**English** | [简体中文](README.zh-CN.md)

<a href="https://bushimashen.github.io/image-compressor/"><b>▶ Live demo</b></a>

## Features

| | |
|---|---|
| **Smart format** | PNG → WebP (keeps transparency, usually half the size); everything else → JPG |
| **Target file size** | Set e.g. 200 KB and it binary-searches the quality to land just under it |
| **Dimension cap** | Longest edge capped at 4K / 2K / 1920 / 1280 / 800, or left untouched |
| **Transparent → JPG** | Detects alpha and composites onto white instead of letting it turn black |
| **Batch** | Multi-file pick, drag & drop, and Ctrl+V paste all work; queue processes one by one |
| **Before/after compare** | Draggable split view plus a table comparing JPG / WebP / PNG at the current settings |

## Two details that matter

**WebP support is verified, not assumed.** Safari used to accept `toBlob(..., 'image/webp')` and
quietly return a PNG. So the tool encodes a 1×1 test pixel and checks `blob.type`; if it isn't really
WebP, it falls back to JPG instead of silently producing a mislabelled file.

**EXIF orientation is baked in.** `createImageBitmap(file, { imageOrientation: 'from-image' })` applies
the EXIF rotation at decode time, so portrait photos taken on a phone don't come out sideways — the bug
you get when you draw an `<img>` straight to canvas in some browsers.

**Target size uses binary search on quality**, not a single guess: 8 iterations between 0.05 and 0.95,
stopping early once the bracket is narrower than 0.04. If even the minimum quality overshoots, it says
so in the row instead of pretending it hit the target.

## Usage

Download `index.html` and double-click it. No install, no build, no network.

## Notes

- PNG re-encoding barely shrinks files. If a PNG has no alpha, the row will suggest WebP or JPG instead.
- Output is capped by what the browser's encoder can do; quality 75 is a good default for photos.
- Animated GIF only compresses its first frame.

## License

MIT — see [LICENSE](LICENSE).
