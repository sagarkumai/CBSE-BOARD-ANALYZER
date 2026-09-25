# CBSE Top-5 Percentage Calculator

A single-page, client-side web app that takes a CBSE result file — either the
official **gazette-style fixed-width `.TXT`** export (school/roll-no wise
gazette) or a plain **CSV** — and:

1. Parses it (auto-detects the format).
2. Auto-detects which columns are subject marks vs. identifier fields
   (roll no, name, school, etc.).
3. Computes each candidate's **percentage from their best 5 subjects**.
4. Lets you download the results as an **Excel (.xlsx)** file or a **CSV**.

Everything runs **entirely in the browser** — no backend, no server upload,
no data leaves the user's machine. It's a static HTML file that can be
hosted anywhere (GitHub Pages, Netlify, a local file, etc.).

## Features

- **Two input formats, auto-detected:**
  - CBSE "school / roll no wise gazette" fixed-width `.TXT` (2 lines per
    candidate: name + subject codes, then marks + grades).
  - Generic CSV with a header row (subject/mark columns detected by
    checking which columns are mostly numeric).
- **Top-5 percentage**: for each candidate, the 5 highest subject marks are
  averaged into a percentage (assumes each subject is scored out of 100).
  Blank/absent marks are ignored rather than treated as zero.
- **Preview** of the parsed data before calculating.
- **Export** to `.xlsx` (via [SheetJS](https://sheetjs.com/)) or `.csv` (via
  [PapaParse](https://www.papaparse.com/)).
- Responsive layout, light/dark theme aware.

## Usage

Just open `index.html` in a browser (or visit the GitHub Pages URL once
enabled — see below). No build step, no install.

1. Click **Choose File** and select your CBSE `.TXT` gazette file or a `.csv`.
2. Review the auto-detected columns in the preview table.
3. Click **Calculate Top-5 %**.
4. Click **Download Excel (.xlsx)** or **Download CSV** to save the results.

## Running locally

```bash
git clone https://github.com/<your-username>/cbse-top5-percentage-calculator.git
cd cbse-top5-percentage-calculator
open index.html   # or just double-click the file / drag it into a browser
```

## Deploying with GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to `Deploy from a branch`,
   branch `main`, folder `/ (root)`.
4. Your app will be live at
   `https://<your-username>.github.io/cbse-top5-percentage-calculator/`.

## How the gazette `.TXT` parser works

The official CBSE gazette export is fixed-width, not delimited. Each
candidate occupies two lines:

```
25622094   M ABHISHEK BARMOLA                                    301     030     054     055     083     048      A2 A2 A2    PASS
                                                                   083  B1 055  C2 059  C2 056  C1 052  D2 072  C1
```

- Line 1: roll no, sex, name, 6 subject codes, 3 overall grades, result,
  compartment subject.
- Line 2: the 6 corresponding (marks, grade) pairs, column-aligned under
  line 1's subject codes.

The parser reads fixed character offsets (roll: `0–8`, sex: `11`, name:
`13–64`, subject slots starting at column `65` in 8-character strides,
marks at `+0..3`, grade at `+5..7`, overall grades at `114/117/120`, result
from `126` onward) to reconstruct each candidate as a structured record.
`SCHOOL :` header lines are tracked so every candidate is tagged with their
school code/name.

## Tech stack

- Vanilla HTML/CSS/JS — no framework, no build tooling.
- [PapaParse](https://www.papaparse.com/) for CSV parsing/export.
- [SheetJS (xlsx)](https://sheetjs.com/) for Excel export.

## License

MIT — see [LICENSE](LICENSE).
