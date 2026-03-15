# OCRify Pro — Professional Browser OCR

> Convert scanned PDFs to searchable, copy-paste-able documents. Runs 100% in your browser — no uploads, no servers, complete privacy.

**[Live Demo →](https://yourusername.github.io/ocrify-pro)**

---

## ✨ Features

- **Word-level text alignment** — OCR text layer precisely positioned to match each word's bounding box
- **5 Quality Modes** — Fast (150 DPI) → Balanced → High Quality → Max Quality → Lossless (PNG)
- **100+ Languages** — Full Tesseract language support including CJK, Arabic, Cyrillic
- **Parallel Processing** — Uses all available CPU cores via Web Workers
- **Large File Support** — Processes 500MB+ PDFs via IndexedDB page buffering
- **Image Preprocessing** — Optional deskew, denoise, binarize, contrast enhance, sharpen
- **AdSense Ready** — Ad slots pre-wired throughout the UI

---

## 🚀 Deploy to GitHub Pages (5 minutes)

### Option A: Upload Files

1. Create a new GitHub repository (e.g. `ocrify-pro`)
2. Upload `index.html` to the root
3. Go to **Settings → Pages → Source → Deploy from branch → main → / (root)**
4. Your app is live at `https://yourusername.github.io/ocrify-pro`

### Option B: Git

```bash
git init
git add index.html README.md
git commit -m "Initial deploy"
git remote add origin https://github.com/YOURUSERNAME/ocrify-pro.git
git push -u origin main
```

Then enable GitHub Pages in repo settings.

---

## 💰 AdSense Integration

AdSense slots are pre-built into the app with clear comments. To activate:

1. Get approved at [Google AdSense](https://adsense.google.com)
2. Get your Publisher ID (`ca-pub-XXXXXXXXXXXXXXXX`)
3. In `index.html`, search for `ADSENSE` comments and uncomment the `<ins>` blocks
4. Replace `ca-pub-XXXXXXXXXXXXXXXX` with your Publisher ID
5. Replace `0000000000` slot numbers with your actual slot IDs

### Ad Slot Locations

| Location | Format | Notes |
|---|---|---|
| Upload screen top | Leaderboard 728×90 | High visibility |
| Upload screen bottom | Banner | After features |
| Config screen | Responsive auto | Mid-page |
| Results screen | Rectangle 336×280 | High CTR spot |

---

## 🔧 How It Works

```
┌─────────────────────────────────────────────────────┐
│  Browser-Native OCR Pipeline                         │
│                                                     │
│  PDF.js          →  Render each page to canvas      │
│  Preprocessor    →  Optional image enhancement      │
│  Tesseract.js    →  OCR with word bounding boxes    │
│  pdf-lib         →  Build output PDF with:          │
│                      - Original page image          │
│                      - Invisible text layer         │
│                        (word-level positioned)      │
│  IndexedDB       →  Buffer pages for large files    │
└─────────────────────────────────────────────────────┘
```

### Text Layer Alignment Algorithm

The key that separates this from basic OCR tools:

1. Render PDF page at target DPI → get pixel dimensions
2. Run Tesseract OCR → get word list with pixel bounding boxes
3. For each word:
   - Scale bbox from canvas pixels → PDF point coordinates
   - Flip Y axis (canvas=top-left, PDF=bottom-left)
   - Place invisible text at exact position
   - Font size = bounding box height (in points)

```javascript
// Core coordinate transformation
const x  = word.bbox.x0 * (pdfWidth  / canvasWidth);
const y  = pdfHeight - (word.bbox.y1 * (pdfHeight / canvasHeight));
const sz = (word.bbox.y1 - word.bbox.y0) * (pdfHeight / canvasHeight);

page.drawText(word.text, { x, y, size: sz, renderingMode: TextRenderingMode.Invisible });
```

---

## 📦 Technologies

| Library | Version | Purpose |
|---|---|---|
| [PDF.js](https://mozilla.github.io/pdf.js/) | 3.11.174 | Render PDF pages to canvas |
| [Tesseract.js](https://tesseract.projectnaptha.com/) | 5.0.4 | Browser-native OCR |
| [pdf-lib](https://pdf-lib.js.org/) | 1.17.1 | Build output PDF with text layer |

All loaded from jsDelivr CDN. No npm, no build step needed.

---

## 🔒 Privacy

- **Zero uploads** — Files never leave your browser
- **No analytics** — No Google Analytics, no tracking
- **No telemetry** — No usage data collected
- **CDN only** — Network requests are only for loading JS libraries and Tesseract language models
- **Memory cleanup** — Canvas buffers explicitly freed after processing

---

## ⚙️ OCR Quality Modes

| Mode | DPI | OEM | PSM | Image Format | Use Case |
|---|---|---|---|---|---|
| Fast | 150 | LSTM | 6 | JPEG 72% | Clean typed documents |
| Balanced | 200 | LSTM | 3 | JPEG 85% | Most documents (default) |
| High Quality | 250 | LSTM | 3 | JPEG 90% | Complex layouts |
| Max Quality | 300 | LSTM | 3 | JPEG 95% | Poor quality scans |
| Lossless | 200 | LSTM | 3 | PNG | When image quality critical |

---

## 🛠️ Preprocessing Options

| Option | Effect |
|---|---|
| Deskew | Detects and corrects page rotation |
| Remove Noise | 3×3 average blur to reduce scan artifacts |
| Binarize | Otsu's thresholding for clean B&W |
| Enhance Contrast | Histogram stretching for washed-out scans |
| Sharpen | Unsharp mask via Laplacian kernel |
| Optimize Output | JPEG compression for smaller file sizes |

---

## 🧠 Large File Handling

For 500MB+ PDFs with 1000+ pages:

1. Pages processed in configurable batches (1–8 pages at once)
2. Each batch processed in parallel across CPU workers
3. Processed page data (image bytes + OCR words) stored in IndexedDB
4. Final PDF assembled from IndexedDB, loaded page by page
5. Canvas memory explicitly freed after each page
6. Peak RAM ≈ `batch_size × page_size_at_target_DPI`

---

## 📋 Browser Compatibility

| Browser | Support |
|---|---|
| Chrome / Edge 90+ | ✅ Full support |
| Firefox 90+ | ✅ Full support |
| Safari 15+ | ✅ Full support |
| Mobile Chrome | ✅ Works (large files may be slow) |

Requires: Web Workers, IndexedDB, Canvas API, Fetch API

---

## 📄 License

MIT License — Free to use, modify, and deploy.

---

*Built with OCRify Pro Engine · Browser-native · Privacy-first*
