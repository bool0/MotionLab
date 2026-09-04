# Motion Lab

**A complete data studio in a single HTML file.** Open it in a browser, drop in a file, and you can clean, filter, chart, analyze and report on your data — no installation, no account, no internet connection, no data ever leaving your machine.

Built with sports data in mind (it reads FIT activity files natively), but it works just as well on any CSV, spreadsheet export, or data-science file.

## Why you might like it

- **One file, zero setup.** Everything — the app, the chart engine, the file readers — is packed into `MotionLab.html`. Copy it to a USB stick, email it, put it on an intranet. Double-click and it runs.
- **Fully offline & private.** All processing happens inside your browser. The app makes **zero network requests** — your data never leaves your computer.
- **No expertise needed.** If you can use a spreadsheet, you can use this. Pick a chart type, pick two columns, done.
- **Reads 9 formats, writes 7.** It also works as a free file converter (FIT → CSV, CSV → Parquet, …).
- **Real reports.** Compose multi-page A4 dashboards with charts and rich-text notes, then export a single interactive HTML file or print to PDF — one page per tab.

## Quick start (60 seconds)

1. Download `MotionLab.html` and double-click it (any modern browser: Chrome, Edge, Firefox, Safari).
2. Try it without any file at all — copy the text below, click the page, and press **Ctrl/⌘ + V**:

```
City,Month,Sales,Rating
Berlin,Jan,1200,4.5
Berlin,Feb,1350,4.2
Madrid,Jan,900,3.9
Madrid,Feb,,4.1
Tokyo,Jan,2100,4.8
Tokyo,Feb,1980,4.7
```

3. You now have a table. Click the **Charts** tab → it already drew a bar chart. Change *Chart type*, *X axis* or *Y axis* and watch it update.

That's the whole learning curve.

## What it can do

### Load almost anything

| Read | Write (Export ▾) |
|---|---|
| CSV / TSV (delimiter auto-detected) | CSV |
| **FIT** activities | Feather |
| Feather / Arrow (incl. lz4 / zstd compressed) | **Parquet** |
| **Parquet** (incl. snappy) | JSON (records) |
| JSON (all pandas orients) / JSONL | JSONL |
| HTML tables | XML |
| XML | HTML table |
| GeoJSON (as a map background) | interactive HTML report |
| `.mlab` project files | `.mlab` project, PDF (print) |

Converting between formats is just *load → (optionally edit) → Export*. HDF5 and pickle can't be decoded in a browser; the app tells you the one-line Python command to convert them.

### Clean messy data

- One-click: trim whitespace, remove duplicate rows, drop empty rows
- Per column: rename, delete, convert type, fill missing values (mean / median / mode / custom)
- Find & replace with optional regex, across one column or all
- Filters (`=`, `≠`, `>`, `≥`, `<`, `≤`, contains, is empty …) that flow into every chart and export
- Everything is undoable (Ctrl/⌘ + Z)
- A **Describe** button shows pandas-style summary statistics (count, mean, std, quartiles…) for every column

### Chart without writing code

14 chart types: bar, line, area, donut, scatter, histogram (with box/violin overlays), box plot, heatmap, density heatmap, polar scatter, rose, GPS track, 3D scatter, 3D path, 3D surface — plus free-form **text boxes** with Markdown, HTML and LaTeX math support.

Every chart is interactive (zoom, pan, hover, PNG export) and colorblind-safe by design. Charts on one page can come from **different datasets**.

### Join tables like a database

Load two files, open the *Join datasets* panel, and merge them on one **or several** key columns (inner / left / right / full outer — same semantics as pandas `merge`). The result appears as a new `[merge] …` dataset.

### Sports analysis (FIT files)

Drop a `.fit` file and you get separate tables for per-second records, laps, pool lengths, a session summary — and raw sensor waveforms (e.g. 25 Hz accelerometer) expanded sample-by-sample. Developer fields from third-party sensors are decoded with their names and units.

The **Activity analysis** panel computes, from GPS alone:

- total distance (Haversine), moving time, average speed and pace
- rolling pace / speed / grade over a window you choose (per km, per 500 m, per minute…)
- split tables ("each km took…")
- an uphill/downhill label per point

Color the GPS track by pace to heat-map your route, or by grade to see climbs in red and descents in blue. Want streets under the track? Export any area as GeoJSON (e.g. from overpass-turbo) and load it as an offline basemap.

### Build and share reports

- Pages are fixed **A4 landscape** sheets — what you see is exactly what prints
- Drag charts to resize (with snap guides) and reorder; charts that no longer fit flow to the next page automatically
- Add titles, subtitles, notes and rich-text blocks (Markdown / HTML / `$LaTeX$`)
- **Export HTML**: one self-contained interactive file with a tab per page — send it to anyone, no software needed
- **Print / PDF**: one A4 page per tab
- **Save project** (`.mlab`): all datasets + all pages in one small file — open it on any computer and keep editing

## Example: from watch to report

1. Drop `my_run.fit` on the page → tables appear (records, laps, …)
2. *Activity analysis* → **Add metric columns** → pace, grade, cumulative distance appear as new columns
3. Charts tab → *GPS track*, color by `Pace (min/km)` → your route as a heat map
4. Add a text box: `# Sunday long run` plus your notes
5. **Export HTML** → send the file to your coach

## Practical limits

Tested comfortably up to ~1 million rows (loads in ~1 s); the practical ceiling is 3–5 million rows / a few thousand columns, bounded by browser memory. Tables are paginated and charts aggregate or sample automatically, so the UI stays responsive at any size.

## Third-party libraries (embedded)

This project bundles the following excellent open-source libraries:

| Library | Purpose | License |
|---|---|---|
| [Plotly.js](https://plotly.com/javascript/) | charts | MIT |
| [Apache Arrow JS](https://arrow.apache.org/) | Feather read/write | Apache-2.0 |
| [hyparquet](https://github.com/hyparam/hyparquet) / [hyparquet-writer](https://github.com/hyparam/hyparquet-writer) | Parquet read/write | MIT |
| [fzstd](https://github.com/101arrowz/fzstd) | ZSTD decompression | MIT |
| [marked](https://marked.js.org/) | Markdown | MIT |
| [MathJax](https://www.mathjax.org/) | LaTeX math | Apache-2.0 |

Full copyright notices and license texts are in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

## Disclaimer

- **No warranty.** This software is provided *"as is"*, without warranty of any
  kind, express or implied. Use it at your own risk; always keep a copy of your
  original data files. See the [LICENSE](LICENSE) for the full warranty and
  liability disclaimer.
- **Not medical or training advice.** Charts, statistics and derived metrics
  (pace, grade, heart-rate summaries, …) are computed from the data you load and
  are for informational purposes only. They are not medical advice, and no
  training or health decision should be based on them without consulting a
  qualified professional.
- **Your data, your responsibility.** Everything runs locally and nothing is
  uploaded — but files *you* export and share (reports, converted data, project
  files) may contain personal data such as GPS locations and heart rate. Check
  before sharing. If you import third-party data (e.g. OpenStreetMap extracts
  as a basemap), you are responsible for complying with that data's license.
- **Accuracy of file formats.** The FIT, Parquet, Feather, JSON, XML and HTML
  readers/writers aim for interoperability but are not certified by any format's
  steward. Verify round-trips on data that matters.
- **Trademarks.** All product names and file-format names mentioned are
  trademarks of their respective owners. This independent project is not
  affiliated with or endorsed by any of them; names are used only to describe
  compatibility.

## License

Licensed under the [Apache License, Version 2.0](LICENSE).
