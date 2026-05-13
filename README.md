# Misty Dreamline

**A corpus explorer for dream-language density across all 101 issues of *Misty* (IPC, 1978–1984)**

🔗 [paulfdavies.github.io/MistyDreamline](https://paulfdavies.github.io/MistyDreamline/)

---

## What this is

*Misty* was a British girls' weekly comic that ran for 101 issues. It was unusual in the genre for its consistent preoccupation with dreams, visions, altered states, and the uncanny. This tool maps that preoccupation across the full run.

The explorer shows:

- **Dream density** — how often dream-language appears in each issue, derived from OCR text
- **Running stories** — each serialised story plotted as a span across its issues
- **Term frequency** — top TF-IDF terms per issue, with dream-family words highlighted
- **Page-level heatmap** — where within an issue dream mentions cluster

Click any bar or story span to explore.

---

## How it was built

The pipeline has three stages:

### 1. OCR — `misty_ocr.py`
Extracts CBR/CBZ comic archives, upscales each page 2×, and runs Apple Vision OCR. Outputs structured JSON (page-level text blocks with bounding boxes) and plain text per issue.

### 2. Story index — `build_story_index.py`
Parses Scrivener RTF files from a research project database to extract story titles, issue ranges, artists, and thematic keywords. Cross-references OCR dream-mention counts against tagged stories.

### 3. HTML build — `build.py`
Reads the OCR text and story index, computes TF-IDF term rankings and page-level dream counts, and generates a single self-contained `index.html` with all data embedded. No server, no dependencies — open the file or deploy anywhere.

---

## Data sources

| Source | Description |
|--------|-------------|
| CBR/CBZ scan archive | Full run of *Misty*, issues 1–101 |
| Scrivener research database | Story-level metadata: titles, issue ranges, artists, thematic tags |
| Apple Vision OCR | Text extracted from scanned pages |

Dream-language detection uses a curated word list covering: *dream, nightmare, vision, sleep, wake, trance, prophecy, omen, curse,* and related forms.

---

## Research context

This tool was built as part of a study of dream rhetoric in British girls' comics of the 1970s–80s with Dr Julia Round.

The Spellbound corpus explorer (forthcoming) extends the same methodology to *Misty*'s sister title.

---

*Paul Fisher Davies — FE educator and comics scholar*
