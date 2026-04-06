# رُشد AI — RushdAI

> *أحيِ التراث العربي · Bring Arabic Heritage to Life*

RushdAI transforms scanned Arabic books into beautifully structured, professionally formatted ebooks. Upload a PDF scan, and RushdAI reads every page — recognising prose, poetry, Quranic verses, chapter headings, footnotes and endnotes — then exports a publication-ready EPUB and PDF.

Built for classical Islamic texts. Designed with love for the tradition it serves.

---

## ✨ What It Does

| Feature | Description |
|--------|-------------|
| 🔍 **AI-powered OCR** | Claude AI reads each page and classifies every section by type |
| 📌 **Smart classification** | Detects prose, poetry (شعر), Quran, headings (باب/فصل), basmalas, footnotes, endnotes, and page headers |
| ✍️ **Human review** | Edit, reclassify, reorder, and correct any section before export |
| 📚 **EPUB export** | Fully structured EPUB3 with popup footnotes, navigable TOC, RTL layout, and Amiri font |
| 📄 **PDF export** | Print-ready A4 PDF with running headers, page numbers, and chapter-end footnotes |
| 💾 **Persistent progress** | Auto-saves to browser localStorage. JSON backup for cross-device portability |
| 🔁 **Resumable** | Stop and resume processing anytime. Rate limit auto-retry built in |

---

## 📖 Designed For

- Classical hadith collections (مصنفات الحديث)
- Fiqh texts (كتب الفقه)
- Arabic poetry collections (دواوين الشعر)
- Quranic tafsir (كتب التفسير)
- Any scanned Arabic book with mixed content types

---

## 🚀 Getting Started

### Prerequisites
- A [Claude.ai](https://claude.ai) account (free or Pro)
- A scanned Arabic PDF
- A modern browser (Chrome recommended for PDF export)

### Usage

1. **Open the app** inside Claude.ai as an artifact
2. Click **Test API** to verify connectivity
3. Click **Upload PDF** and select your scanned book
4. Fill in **Book Info** — title, author, disclaimer
5. Click **OCR** on individual pages, or **All▶** to process the entire book
6. Review each page — edit text, fix classifications, reorder sections
7. Click **📤 Export** → choose EPUB or PDF

> **Tip:** Use **All▶** and let it run in the background. It auto-retries rate limits and saves progress as it goes.

---

## 🗂️ Section Types

RushdAI classifies text into 8 types, each with its own visual style and export behaviour:

| Type | Arabic | Description | Export |
|------|--------|-------------|--------|
| `heading` | عنوان | Chapter/section titles (باب، فصل) | H2, drives TOC and running header |
| `basmala` | بسملة | بِسْمِ اللَّهِ الرَّحْمَنِ الرَّحِيمِ | Centered, decorative |
| `prose` | نثر | Flowing paragraphs and hadith chains | Justified text |
| `poetry` | شعر | Verse poetry (`صدر *** عجز` format) | Two-column table layout |
| `quran` | قرآن | Quranic text with full tashkeel | Green framed box |
| `footnote` | حاشية | Inline references like `[م:٢٣٣٧]` | Small gray superscript |
| `endnote` | هامش | Numbered notes `(١)(٢)` at page bottom | Popup in EPUB, chapter-end in PDF |
| `pageheader` | ترويسة | Book's own printed running header | Excluded from export |

---

## 📚 EPUB Format

The exported EPUB3 is built to professional publishing standards:

- **Apple Books popup footnotes** — endnotes in a separate `footnotes.xhtml` file with `linear="no"` in the spine. Tapping a reference number shows the note as a popup.
- **Navigable TOC** — all chapter headings extracted and linked
- **RTL throughout** — `page-progression-direction="rtl"`, Arabic language metadata
- **Amiri font** — full Unicode Arabic coverage including ﷺ ﷻ ؓ
- **Running header** = book title (controlled by EPUB filename, as Apple Books requires)
- **Justified text** on all devices

---

## 📄 PDF Format

The PDF is generated as a print-ready HTML file — open in Chrome and print to save:

- **A4 page size** with professional margins
- **Title page** with disclaimer and RushdAI credit
- **Running chapter header** at top of each page
- **Page numbers** centered at bottom
- **Chapter-end footnotes** — each chapter's endnotes collected after its text, separated by a rule
- **Poetry** in two-column table layout
- **Quran** in a bordered green box

---

## 💾 Saving & Resuming

RushdAI uses browser `localStorage` — your progress is stored directly on your device.

| Action | How |
|--------|-----|
| **Auto-save** | Happens after every page processed |
| **Manual save** | Click 💾 Save |
| **Backup to file** | Click ⬇ Backup → downloads `rushdai-backup.json` |
| **Restore from file** | Click ⬆ Restore → select your JSON backup |
| **Resume same book** | Upload the same PDF — restore modal appears automatically |

> **Best practice:** Download a JSON backup after every session. It's your insurance against browser data being cleared.

---

## ⚙️ Technical Architecture

### Footnote Matching Rule
Endnote numbers restart per hadith, so matching is strictly local:

```
For each prose block:
  collect ONLY the endnote sections that immediately follow it
  stop at the next prose OR heading block
  match (١)(٢) markers by number within that local set only
```

This correctly handles multiple hadiths on the same page each with their own `(١)(٢)` sequence.

### EPUB ZIP Structure
Built with pure JavaScript `DataView` byte-writing (no external dependencies):
- `mimetype` — first file, STORE compression, byte-perfect
- `META-INF/container.xml`
- `OEBPS/package.opf` — metadata, manifest, spine
- `OEBPS/nav.xhtml` — TOC with all chapter links
- `OEBPS/content.xhtml` — main text
- `OEBPS/footnotes.xhtml` — all endnotes as `<aside epub:type="footnote">`, `linear="no"`
- `OEBPS/style.css` — RTL Arabic styling

### Storage
- `localStorage` keyed by `ocr:{filename}_{filesize}`
- JSON backup format: `{results, totalPages, meta, bookKey}`
- No server dependency

---

## 🔑 API & Compatibility

RushdAI uses the Anthropic Claude API through Claude.ai's proxy. The API calls work when the app is opened **inside Claude.ai** as an artifact.

**EPUB readers tested:**
- ✅ Apple Books (iOS + macOS) — full footnote popup support
- ✅ Google Play Books
- ✅ Kobo
- ✅ Moon+ Reader (Android)
- ✅ Thorium Reader (desktop)
- ⚠️ Kindle — requires conversion to MOBI/KFX via Calibre

---

## 🗺️ Roadmap

- [ ] API key input field for standalone deployment (no Claude.ai required)
- [ ] Cover image support
- [ ] Multi-book library management
- [ ] Collaborative review (shared JSON format)
- [ ] Server-side PDF generation with true page-bottom footnotes

---

## 📜 Disclaimer

> أُعِدَّ هذا المشروع خدمةً لكتاب الله وسنة نبيه ﷺ، ولوجه الله تعالى خالصًا.
>
> This project was prepared for the sake of Allah and in service of Islamic knowledge.

---

## 🤝 Contributing

RushdAI is a single-file application (`index.html`). To contribute:

1. Fork the repository
2. Make your changes to `index.html`
3. Test with a real Arabic PDF inside Claude.ai
4. Submit a pull request with a clear description of the change

Please keep the single-file architecture — it keeps deployment simple and the app self-contained.

---

## 📄 License

This project is open source and free to use for non-commercial purposes in service of Islamic knowledge preservation.

---

<div align="center">

**RushdAI** · Built with ❤️ for the Arabic Islamic heritage

*رُشد — guidance, right direction, intellectual maturity*

</div>
