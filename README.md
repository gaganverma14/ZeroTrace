# ZEROTRACE

**A metadata redaction utility for images — runs entirely in your browser.**

ZEROTRACE lets you inspect and strip embedded metadata (GPS location, timestamps, camera info, author/copyright, thumbnails, color profiles, and more) from your images before sharing them. Nothing is uploaded anywhere — every byte of parsing, redaction, and file generation happens on-device, in a single self-contained HTML file.

## Features

- **Field-level control, not a blind wipe.** Only the metadata categories actually present in your file are shown — pick exactly what to keep and what to remove.
- **Explicit Keep / Remove decisions** for every detected category, with a clear visual state (no guesswork).
- **Deep format support:**
  - **JPEG** — full EXIF tag-level filtering (0th/Exif/GPS/1st/Interop IFDs) plus XMP, IPTC/Photoshop, ICC, and comment segment removal.
  - **PNG** — chunk-level filtering (tEXt/iTXt/zTXt, tIME, eXIf, iCCP, pHYs, and more), while chunks required for correct rendering (IHDR, PLTE, IDAT, tRNS, APNG frames) are always preserved.
  - **WebP** — RIFF chunk stripping for EXIF/XMP/ICC, including correct VP8X flag-bit patching.
  - **GIF / BMP / TIFF / other** — full re-encode fallback, clearly labeled as such.
- **GPS coordinates decoded to readable lat/long**, not just raw rational tag dumps.
- **Batch support** — load multiple images, process one or all of them, download individually or as a ZIP.
- **Before/after size comparison** and a manifest of exactly what was removed.
- **Dark and light themes.**
- **Zero network calls for image data.** Pixel data is verified byte-identical before and after redaction — only metadata is touched.

## Usage

1. Open `zerotrace.html` in any modern browser (double-click it, or host it as a static file).
2. Drop in one or more images, or click to browse.
3. Review the detected metadata categories for each image and mark each one **Keep** or **Remove**.
4. Click **Strip Metadata**.
5. Download the cleaned image(s).

No build step, no server, no dependencies to install — it's a single HTML file.

## How it works

- **JPEG** EXIF parsing/rewriting is handled with [piexifjs](https://github.com/hMatoba/piexifjs); XMP, IPTC, ICC, and comment segments are located and stripped with a custom JPEG segment scanner operating directly on the byte stream.
- **PNG** chunks are parsed and filtered with a custom chunk-level parser that respects the PNG spec's critical/ancillary chunk convention, so image-breaking chunks are never touched.
- **WebP** is parsed as a RIFF container, with metadata chunks (EXIF/XMP/ICCP) removed and the VP8X feature flags patched to match.
- Unsupported formats fall back to a canvas re-encode, which strips all metadata by nature of the re-encoding process.
- [JSZip](https://github.com/Stuk/jszip) is used for batch ZIP downloads.

## Privacy

Everything runs client-side. Your images never leave your device — there is no backend, no upload endpoint, and no analytics.

## Browser support

Any modern browser with support for the Canvas API, `Blob`, and `ArrayBuffer` (Chrome, Firefox, Safari, Edge — desktop and mobile).

## License

MIT
