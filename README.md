# PDFyre

Free, privacy-first PDF tools that run 100% in your browser — no uploads, no servers, no account.

## Tools
- OCR PDF → searchable PDF (parallel workers, 100+ languages, word-level text layer)
- Merge, Split, Compress, Rotate
- PDF to Images, Images to PDF

## Tech
- Vanilla JS single-page app (`index.html`)
- PDF.js (render/read), pdf-lib (build/modify), Tesseract.js (OCR via WebAssembly)
- IndexedDB for chunked on-device storage of large files
- GitHub Pages via `.github/workflows/static.yml`

## Privacy
All processing happens on the user's device. Files are never uploaded.

## Deploy
Push to `main` — the workflow stages only production files to GitHub Pages.
