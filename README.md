# Utils

A self-hosted collection of browser-based utilities for common text and data tasks. No backend, no build step, just pure HTML/CSS/JS.


## Intent

Provide a fast, offline-capable toolbox for day-to-day developer text operations. All processing runs in the browser; nothing is sent to a server.


## Structure

```
index.html          Landing page - card grid linking to each utility
utility/
  text.html         Line-based text operations
  json.html         JSON formatting and validation
  base64.html       Base64 encoding and decoding
  password.html     Random password generator
  colorpicker.html  Color picker with format conversions and tools
Notes.md            This file
```


## Design guidelines

- **Layout**: Whenever applicable, utility page uses a three-column layout — Input | Options | Output - filling the full viewport height. 
- **Preview flow**: All current operations are lightweight and update the output in real time as the user types or changes options.
- **Commit flow**: Once the output looks correct, the **Commit** button replaces the input with the output, allowing operations to be chained.
- **Copy**: A **Copy** button in the output panel writes to the clipboard.
- **Template**: A base template for utility pages can be found at utility/page-template.html

### Styling guidelines
- **Theme**: VS Code-inspired. Fully self-contained - no external stylesheets or scripts.

**Common**
| Variable          | Value     | Role                        |
|-------------------|-----------|-----------------------------|
| `--r`             | `5px`     | Border radius               |
| `--mono`          | `'Cascadia Code', Consolas, 'Courier New', monospace` | Monospace font stack |

**Dark mode**
| Variable          | Value     | Role                        |
|-------------------|-----------|-----------------------------|
| `--bg`            | `#1e1e1e` | Page background             |
| `--surface`       | `#252526` | Panel / card background     |
| `--surface-hover` | `#2a2d2e` | Hover state for surfaces    |
| `--border`        | `#3c3c3c` | Dividers and input borders  |
| `--text`          | `#d4d4d4` | Primary text                |
| `--text-dim`      | `#888888` | Labels, placeholders, hints |
| `--accent`        | `#0078d4` | Buttons, focus rings        |
| `--accent-hover`  | `#106ebe` | Accent hover state          |
| `--success`       | `#4ec9b0` | Valid / positive feedback   |
| `--warn`          | `#dcdcaa` | Warning feedback            |
| `--error`         | `#f44747` | Error / invalid feedback    |

**Light mode**
| Variable          | Value     | Role                        |
|-------------------|-----------|-----------------------------|
| `--bg`            | `#f8f8f8` | Page background             |
| `--surface`       | `#ffffff` | Panel / card background     |
| `--surface-hover` | `#f0f0f0` | Hover state for surfaces    |
| `--border`        | `#d0d0d0` | Dividers and input borders  |
| `--text`          | `#1f1f1f` | Primary text                |
| `--text-dim`      | `#767676` | Labels, placeholders, hints |
| `--accent`        | `#0078d4` | Buttons, focus rings        |
| `--accent-hover`  | `#106ebe` | Accent hover state          |
| `--success`       | `#0e7a6e` | Valid / positive feedback   |
| `--warn`          | `#7a6400` | Warning feedback            |
| `--error`         | `#c72e2e` | Error / invalid feedback    |

## Utilities

### Text (`utility/text.html`)

**Implemented:**
- **Trim whitespace** — strips leading/trailing whitespace from each line
- **Remove empty lines** — filters out blank lines (applied after trimming)
- **Deduplicate lines** — removes duplicate lines, keeping first occurrence; optional case-insensitive matching
- **Sort lines** — ascending or descending alphabetical sort; optional case-insensitive comparison
- **Case conversion** — lowercase, uppercase, title case, or sentence case applied per line
- **Remove accents** — strips diacritical marks, normalising to base characters (é → e, ñ → n)
- **Remove special characters** — removes everything except letters, digits, and whitespace
- **Split on delimiter** — splits each line on a delimiter string into multiple lines
- **Join with separator** — joins all lines into one string using a separator
- **HTML entities** — encode (`<` → `&lt;`, `&` → `&amp;`, etc.) or decode HTML character entities per line
- **URL encode / decode** — encodes or decodes each line with `encodeURIComponent` / `decodeURIComponent`
- **Output details** — live line, word, character, and byte count shown below the output

Processing pipeline order: trim → remove empty → deduplicate → sort → case → remove accents → remove special characters → split → join → HTML entities → URL encode/decode.

**Planned:**
- Natural/numeric sort
- Prefix / suffix add or remove
- Filter lines by regex
- Count line occurrences
- Remove consecutive duplicate blank lines
- Convert tabs to spaces / spaces to tabs
- Remove lines containing
- Compare input/output (no change, change)
- Search and replace (text and Regex)
- Remove everything before/after x (can be solved using regex search and replace)
- Search count
- Padding (left or right padding with selected character)
- leetspeak encode/decode
- Number format (e.g. from US to NO, thousand separator, decimal separator, number of decimals)
- Move rarely used features (like HTML encoding), into grouped collapsible sections
- Reverse case
---

### JSON (`utility/json.html`)

**Implemented:**
- **Format** — pretty-prints JSON with configurable indent (2 spaces, 4 spaces, tab)
- **Minify** — produces compact single-line JSON
- **Sort keys** — recursively alphabetises object keys (available in Format mode)
- **Inline validation** — shows a live valid/error indicator with the parser error message

**Planned:**
- JSON5 support (comments, trailing commas) via a library
- Tree view with collapsible nodes
- Search / filter by key or value
- Diff two JSON documents
- Generate JSON Schema from a sample
- Convert to/from YAML or XML
- Add JSON5 validation if normal JSON fails, add notice that it is only JSON5 compatilbe if it succeeds

---

### Base64 (`utility/base64.html`)

**Implemented:**
- **Encode** — UTF-8 text → Base64
- **Decode** — Base64 → UTF-8 text
- **Auto-detect** — heuristic (character set + length divisibility) decides whether to encode or decode; shows which operation was chosen
- **Standard encoding** — uses `+`, `/`, and `=` padding (RFC 4648 §4)
- **URL-safe encoding** — uses `-`, `_`, no padding (RFC 4648 §5)

**Planned:**
- File encode/decode (drag-and-drop binary files)
- Line-wrap output at 76 characters (MIME Base64)
- Validate that input is well-formed Base64 before decoding


---

### Password (`utility/password.html`)

**Implemented:**
- **Cryptographically secure** — uses `crypto.getRandomValues()` with rejection sampling to eliminate modulo bias
- **Character sets** — Lowercase a–z, Uppercase A–Z, Digits 0–9, Special `!@#$%^&*…`, and an optional custom characters field
- **Exclude look-alikes** — strips visually ambiguous characters (`I l 1 O o 0 | \``) from all sets
- **Require all sets** — guarantees at least one character from every enabled set before filling remaining positions
- **Configurable length** — slider + number input, range 4–128
- **Batch generation** — generate 1–50 passwords at once
- **Entropy indicator** — shows estimated entropy in bits and a strength label (Weak / Fair / Good / Strong / Very strong)
- **Per-password copy** and **Copy All** buttons
- **Regenerate** button for a fresh batch without changing settings

**Planned:**
- Pronounceable / memorable passwords (word-based)
- Passphrase mode (wordlist-based, e.g. correct-horse-battery-staple)
- Export as CSV or plain text file

---

### Color Picker (`utility/colorpicker.html`)

**Implemented:**
- **Color swatch** — large preview that opens the native OS color picker on click
- **Hex field** — manual entry accepting `#rrggbb` or `#rrggbbaa`; validates input live
- **RGBA sliders** — individual sliders for R, G, B (0–255) and Alpha (0–100%); track gradients update in real time to reflect the current color
- **Format conversions** — HEX, RGB, HSL, HSV, CMYK; RGBA / HSLA variants shown automatically when alpha < 1; copy button on each row
- **Contrast checker** — calculates WCAG contrast ratios against white and black; badges indicate AAA (≥ 7:1), AA (≥ 4.5:1), AA Large (≥ 3:1), or Fail
- **Harmony palette** — Complement, Split-complement (×2), Triadic (×2), Analogous (×2), Square (×2); click any swatch to load that color
- **Tints & shades scale** — 11-step ramp mixing the current color toward black (shades) and white (tints); click any step to load it
- **Color history** — automatically records each committed color (up to 64); click any history swatch to restore it

**Planned:**
- Eyedropper / screen color sampler (EyeDropper API)
- Named CSS color lookup (show closest named color)
- Palette export (CSS custom properties, JSON, or ASE)
- Saved palettes with local-storage persistence
- Color blindness simulation overlays

---

## Planned utilities
- XML
- Regex
- URL, email, phone (validator, formatter and sorter)
- Templater (Apply a template to a given dataset (simple: replace keyword in a text with attributes form input. advanced: create tables from a large dataset))
- Table (create a dynamic table with sortable columns and search filters based on a json input)

---

## Known issues
- base64 does not correctly autodetect base46URL encoded strings, you have to toggle encoding manually
