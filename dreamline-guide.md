# Misty — Dreamline Corpus Explorer: Guide

**What it is, how it works, where the data comes from, and what it can't do.**

---

## What It Is

The Dreamline Corpus Explorer is a self-contained, interactive HTML file that visualises dream-language density across all 101 issues of *Misty* (IPC, 1978–1980). It allows you to see, at a glance, which issues contain the most dream-related language, which stories were running in any given issue, and which individual terms are distinctive to each issue.

It is built entirely from two data sources: Apple Vision OCR text extracted from the comic pages, and Paul's Scrivener story index. No external services are involved. The file is self-contained and works by double-clicking — no internet connection, no login, no installation required.

A Spellbound version is in preparation.

---

## How to Open It

Double-click `index.html` in Finder. It will open in your default browser (Safari, Chrome, or Firefox — all work). If your browser asks whether to open a local file, say yes.

No server required. It does not phone home. You can email it as an attachment and it will work on the recipient's machine in the same way.

---

## The Interface at a Glance

The tool has five main elements, reading top to bottom:

1. **Search bar and filter chips** — across the top
2. **Dream Density Timeline** — the bar chart showing all 101 issues
3. **Running Stories** — coloured horizontal spans showing each story's run
4. **Issue Detail panel** (left) — appears when you click a bar
5. **Story Detail panel** (right) — appears when you click a story span

---

## Dream Density Timeline

Each vertical bar represents one issue of *Misty*, from issue 1 (left) to issue 101 (right). The height of the bar is proportional to the number of times the word DREAM (in any form: dreams, dreamed, dreaming, dreamt) appears in the OCR text of that issue.

**Bar colour** reflects the first story running in that issue, matching the colour of its lane in the Running Stories section below. Issues with no running story in the index are shown in blue; issues with zero dream mentions are shown as a faint stub.

**Hovering** over any bar shows a tooltip: the issue number, dream mention count, and the names of all stories running that week.

**Clicking** a bar populates the Issue Detail panel on the left with full information about that issue.

---

## Running Stories

Below the timeline is a lane diagram showing every story in the index as a horizontal coloured bar, spanning the issues it ran across. Stories are packed into rows to minimise overlap (a "greedy" layout — it finds the first row each story fits into).

**Clicking** a story span populates the Story Detail panel on the right.

Stories whose titles were not successfully extracted from the Scrivener data are silently omitted from this view. If a story you expect to see is missing, it likely needs its Scrivener entry correcting.

---

## Issue Detail Panel

Clicking any bar opens this panel. It shows:

**Dream mention count** — total occurrences of DREAM in the OCR text of the whole issue, shown as a coloured pill (green = low, amber = moderate, red = high).

**Running story pills** — the names of all stories identified as running in this issue, drawn from the story index. These are clickable — clicking a story pill loads that story's detail in the right-hand panel.

**Top terms** — the twelve most *distinctive* words in this issue's OCR text, ranked by TF-IDF (term frequency–inverse document frequency). TF-IDF rewards words that appear often in *this* issue but rarely across the whole corpus — so it surfaces what is specific to this issue rather than what is common to comics generally. Words that belong to the dream-language set (dream, nightmare, vision, sleep, wake, dark, fear, real, mind, shadow, etc.) are shown in purple; others in grey. The number at the right is the raw count.

**Page heatmap** — a strip of small coloured squares, one per page, where the depth of purple indicates how many DREAM mentions fall on that page. Hover over any square for the page number and count. This lets you see whether dream language is concentrated in one part of an issue (e.g. the opening of a story) or distributed throughout.

---

## Story Detail Panel

Clicking a story span (or a story pill in the Issue panel) opens this panel. It shows:

**Statistics row** — issues in the run; average dream mentions per issue; the peak issue (where dream language is most concentrated); total dream mentions across the whole run.

**Mini dreamline** — a small bar chart showing dream density issue by issue across the story's run, scaled to that story's own maximum. This reveals whether dream language is consistent throughout the serial, concentrated at the start (setup), or builds towards a climax. Each bar is interactive: hover for the issue number and mention count; click to load that issue in the Issue Detail panel and highlight it in the main timeline.

**Issue range and artist** — the span of issues covered, plus the artist name where the Scrivener record has it.

**Tags** — the keyword tags from the research CSV (Dream\*, Nightmare\*, Vision\*, Wak\*, Trance\*, etc.) where the story has been tagged by Julia. Untagged stories show "No tags."

**Plot summary** — for corpora where story summaries have been written (currently Spellbound; Misty summaries in progress), a collapsible summary is shown here. Click "Plot summary" to expand it.

---

## Search and Filter Chips

**Search bar** — type any word to highlight only the issues where that word is among the forty most TF-IDF-distinctive terms. Issues where it doesn't appear distinctively are dimmed. Works for dream-language terms ("nightmare", "vision", "shadow") as well as character names, settings, or any other vocabulary that might be distinctive. Clear the box to restore all bars.

*Note:* the search uses two different mechanisms depending on what you type. Dream-language words (dream, nightmare, vision, sleep, wake, dark, fear, shadow, etc.) match against raw mention counts — any issue where the word appears at all is highlighted. Other words match against pre-computed TF-IDF distinctive terms — only issues where the word is *unusually prominent* are highlighted. In both cases the filter chips interact with the search: "Tagged stories" + "nightmare" shows only the issues where a tagged story was running and nightmare appears.

**All issues** — shows everything (default).

**Dream mentions only** — dims issues with zero DREAM occurrences, leaving only issues where the word appears at least once.

**Tagged stories** — dims issues where no tagged story is running, leaving only issues covered by Julia's keyword taxonomy.

---

## Data Sources

**OCR text** is generated by Apple Vision's text recognition framework, run on page images extracted from the comic CBR archives at 2× upscale. The text files live at `~/Documents/Misty/OCR/text/Misty_NNN.txt`, one per issue. Dream counts and TF-IDF terms are computed fresh each time `build.py` is run.

**Story index** (`story_index.json`) is built by `build_story_index.py` from Paul's Scrivener project *Dreams in Comics*. It records each story's title, issue span, artist, and any keyword tags matched from the research CSV. The index maps story titles to issue numbers via `issue_map.json`.

**Keyword tags** come from `Dreams in Comics Stories May26.csv` — Julia's annotation layer identifying which stories contain dream, nightmare, vision, waking, trance, and related content.

**Corpus registry** — the Scrivener project covers both *Misty* and *Spellbound*. Stories from other comics are filtered out using `OutlinerMistySpellboundSummaries.csv`, which labels every story by comic. Only stories labelled *Misty* (or unlabelled, for uncatalogued stories) appear in the *Misty* tool.

---

## Limitations and Caveats

**OCR quality varies.** Apple Vision is very good but not perfect. Comic lettering — hand-drawn, styled, set in panels with complex backgrounds — generates errors. The most common are: missing spaces (NIGHTMARE → NIGHTMAR E), merged words, and misread letterforms (I/1 confusion, O/0, S/5). Rare words, handwritten captions, and very small text are more likely to be missed. DREAM is a short, clear word that OCR handles well; counts should be broadly reliable. Less common terms may be undercounted.

**The DREAM count is a single-word count.** It counts the exact string DREAM (case-insensitive) in its various inflected forms. It does not count synonyms (reverie, slumber, vision) or conceptually related terms. Use the search bar to check individual synonyms issue by issue.

**TF-IDF top terms reflect distinctiveness, not importance.** A word scoring highly means it appears unusually often in this issue relative to the corpus average. This is analytically useful but can surface proper nouns, unusual sound effects, or OCR artefacts alongside genuinely interesting vocabulary.

**The story index is incomplete.** Not all Misty stories have been successfully parsed from the Scrivener data. Stories where the RTF structure doesn't match the expected pattern fall back to a less reliable parser, and some fail entirely. The 37 stories currently indexed represent the stories Paul's Scrivener records cover — there may be short fillers, single-episode stories, or stories not yet entered in Scrivener that are absent from the tool.

**Issue-to-story mapping is approximate.** The issue_map records which stories were *identified as running* in each issue — it is derived from the story index issue ranges, not from page-level story-start identification. If the issue range in Scrivener is wrong by an issue or two, the mapping will be slightly off.

**The pipeline ceiling.** The tool captures verbal language only. Panel border type, colour register, visual abstraction, page layout, and the full formal vocabulary of comics are invisible to it. It is a verbal index to a visual medium — a starting point for close reading, not a substitute for it.

---

## Regenerating the Tool

When the data changes — new OCR, updated story index, new tags in the CSV — run:

```bash
python3 build.py
```

from the `MistyPy/` directory. This regenerates `index.html` in about ten seconds. The file can then be emailed or opened directly.

For Spellbound:
```bash
python3 build.py --ocr ~/Documents/Spellbound/OCR --out spellbound.html
```

---

*Guide written May 2026. Tool version: build.py (corpus-agnostic, v2).*
