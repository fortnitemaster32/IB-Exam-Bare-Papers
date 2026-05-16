# IB Exam Bare Papers

A fully client-side web tool that strips cover pages, instruction sheets, and blank pages from IB exam PDFs, then adds a metadata label on the first page. Everything runs in the browser — no uploads, no server, no installation.

Use it here: **[fortnitemaster32.github.io/IB-Exam-Bare-Papers](https://fortnitemaster32.github.io/IB-Exam-Bare-Papers)**

## Usage

1. Open `index.html` in any modern browser.
2. Drag and drop (or click to browse) one or more IB exam PDFs.
3. Adjust settings if needed (skip pages, year override, download mode).
4. Click "Process All" (or press Ctrl+Enter / Cmd+Enter).
5. Download the cleaned files individually or as a ZIP archive.

## How it works

The tool reads each PDF twice — once for text extraction (pdf.js) to identify question pages, and once for PDF construction (pdf-lib) to rebuild the document with only the question pages plus a metadata label.

### Page removal logic

1. The first N pages are skipped entirely (default 2: the multilingual copyright page and the instructions/cover page).
2. Remaining pages are scanned for question content using text analysis:
   - Pages with fewer than 30 non-whitespace characters are considered blank and removed.
   - Pages matching copyright or instruction boilerplate (in English, French, or Spanish) are removed.
   - Pages containing numbered questions (`1.`, `12.`), maximum mark brackets, action verbs (find, show, calculate, discuss, explain, analyse, etc.), or letter-coded sub-questions are kept.
3. A small grey label (e.g. "AA | Paper 3 | TZ2 | HL") is drawn at the top-left of the first page.

### Filename parsing

The tool reads metadata from filenames to build the output label and filename. It recognises these patterns:

| Example | Subject | Paper | TZ | Level | Year | Language |
|---|---|---|---|---|---|---|
| `2024_Mathematics_analysis_and_approaches_paper_3__TZ2_HL.pdf` | AA | 3 | TZ2 | HL | 2024 | - |
| `Biology_paper_1__TZ2_HL.pdf` | Biology | 1 | TZ2 | HL | - | - |
| `English_A_Language_and_literature_paper_1__TZ1_SL.pdf` | English A Language and literature | 1 | TZ1 | SL | - | - |
| `Physics_paper_1__HL_French.pdf` | Physics | 1 | - | HL | - | French |
| `Mathematics_analysis_and_approaches_paper_3__[German]_HL.pdf` | AA | 3 | - | HL | - | German |
| `AAHL P1 Zone A.pdf` | AA | 1 | - | HL | - | - |
| `Afrikaans_A1_paper_1_HL.pdf` | Afrikaans A1 | 1 | - | HL | - | - |
| `History_paper_3_history_of_Europe__HL_Spanish.pdf` | History | 3 | - | HL | - | Spanish |
| `2021_Mathematics_applications_and_interpretation_paper_3__TZ1_HL.pdf` | AI | 3 | TZ1 | HL | 2021 | - |

Fields not found in the filename are omitted from the label. The year can also be set manually via the year override field.

### Files that are automatically skipped

- Filenames containing "markscheme"
- Filenames containing "question_booklet", "resource_booklet", or "text_booklet"
- Filenames containing "listening_comprehension"
- Filenames without "paper_N" that do not match the short format (e.g. "AAHL P1 Zone A")

## Subjects supported

Works with all IB subject groups:
- Studies in Language and Literature
- Language Acquisition
- Individuals and Societies (History, Economics, Psychology, Geography, etc.)
- Experimental Sciences (Biology, Chemistry, Physics, etc.)
- Mathematics (all syllabuses)
- The Arts

## Technical details

- **Format**: Single HTML file (38 KB) — no build tools, no dependencies to install.
- **Libraries loaded from CDN**: pdf-lib (PDF construction), pdf.js (text extraction), JSZip (ZIP downloads).
- **Fonts**: Playfair Display (headings), DM Sans (body) — loaded from Google Fonts.
- **No tracking, no telemetry, no network requests** other than the CDN library loads.

## Browser support

Chrome, Firefox, Safari, Edge — any modern browser that supports:
- `ArrayBuffer`
- `async/await`
- `File` and `Blob` APIs
- ES2015+

## Deployment

Push `index.html` to any static host. Works on GitHub Pages, Netlify, Vercel, or any web server.

To test locally:
```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

Direct `file://` opening may work but some browsers block CDN script loads over the file protocol. Using a local HTTP server is recommended.

> Created with heavy assistance of AI
