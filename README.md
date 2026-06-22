# Utils

A self-hosted collection of browser-based utilities for common text and data tasks. No backend, no build step, just pure HTML/CSS/JS.

[GitHub](https://github.com/KristofferAlvinDahlgren/SimpleUtils) · [Live preview](https://kristofferalvindahlgren.github.io/SimpleUtils/index.html)


## Intent

Provide a fast, offline-capable toolbox for day-to-day developer text operations. All processing runs in the browser; nothing is sent to a server. All files live in the repository root and are served directly via GitHub Pages.


## Structure

```
index.html          Landing page - card grid linking to each utility
text.html           Line-based text operations
json.html           JSON formatting and validation
base64.html         Base64 encoding and decoding
password.html       Random password generator
colorpicker.html    Color picker with format conversions and tools
regex.html          Regex tester with match visualization and premade patterns
permissions.html    Linux file permission builder and decoder
cron.html           Cron expression validator, explainer, and scheduler
jwt.html            JSON Web Token decoder and inspector
hash.html           Hashing with MD5, SHA-*, and HMAC algorithms
encryption.html     AES-GCM / AES-CBC encrypt and decrypt
README.md           This file
```


## Design guidelines

- **Layout**: Whenever applicable, utility page uses a three-column layout — Input | Options | Output - filling the full viewport height. 
- **Preview flow**: All current operations are lightweight and update the output in real time as the user types or changes options.
- **Commit flow**: Once the output looks correct, the **Commit** button replaces the input with the output, allowing operations to be chained.
- **Copy**: A **Copy** button in the output panel writes to the clipboard.
- **Template**: A base template for utility pages can be found at page-template.html

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

### Text (`text.html`)

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
- Remove any lines (not) containing / matching regex
- Compare input/output (no change, change)
- Search and replace (text and Regex)
- Remove everything before/after x (can be solved using regex search and replace)
- Search count
- Padding (left or right padding with selected character)
- Number format (e.g. from US to NO, thousand separator, decimal separator, number of decimals)
- Move rarely used features (like HTML encoding), into grouped collapsible sections
- Reverse case
- Change order of operations so that split line can be used with sort and unique lines before joining at the end
- Add save to local storage for input, settings, etc. 
---

### JSON (`json.html`)

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

### Base64 (`base64.html`)

**Implemented:**
- **Encode text** — UTF-8 text → Base64
- **Decode text** — Base64 → UTF-8 text
- **Encode file** — any file (image, PDF, binary, …) → Base64 or Data URL; drag-and-drop or file picker; image thumbnail shown in the input panel
- **Decode to image** — Base64 or Data URL → inline image preview with download button
- **Decode to file** — Base64 → downloadable file (specify MIME type or auto-detected from a data URL prefix)
- **Auto-detect** — heuristic (character set + length divisibility) decides whether to encode or decode; shows which operation was chosen
- **Standard encoding** — uses `+`, `/`, and `=` padding (RFC 4648 §4)
- **URL-safe encoding** — uses `-`, `_`, no padding (RFC 4648 §5)
- **Output format** — Raw Base64 or Data URL (`data:mime;base64,…`) when encoding a file
- **MIME presets** — PNG, JPEG, WebP, GIF, SVG, PDF, Binary quick-select buttons when decoding to a file

**Planned:**
- Line-wrap output at 76 characters (MIME Base64)
- Validate that input is well-formed Base64 before decoding


---

### Password (`password.html`)

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

### Color Picker (`colorpicker.html`)

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

### Regex (`regex.html`)

**Implemented:**
- **Pattern input** — `/pattern/flags` style input with live validation; border turns green (valid) or red (invalid) with the parser error shown inline
- **Flags** — checkboxes for `g`, `i`, `m`, `s`, `u`, `d`; stay in sync with a freeform flags text field
- **Visual match highlighting** — full match highlighted in yellow; up to 5 capture groups each tinted a distinct color with an auto-generated legend
- **Match table** — scrollable table listing every match, its character position range, and capture group values; toggleable
- **Match count** — live summary of the number of matches found
- **Copy matches** — copies all matched strings (one per line) to the clipboard
- **25 premade patterns** — Email, URL, IPv4, IPv6, Date (ISO and DMY), Time, Phone, Hex color, UUID, Credit card, Semver, JWT, Hashtag, Mention, Integer, Decimal, Whitespace runs, Empty lines, HTML tag, CSS class, Markdown heading, Markdown link, Unix path, Windows path

**Planned:**
- Named capture group support in the legend and table
- Replace mode (substitute matches with a replacement string)
- Regex explanation / plain-English breakdown
- Match count per line
- Export matches as CSV or plain text

---

### File Permissions (`permissions.html`)

**Implemented:**
- **Interactive permission grid** — 3×3 toggle buttons (Owner/Group/Other × Read/Write/Execute); click to toggle individual bits
- **Special bits** — Setuid (4000), Setgid (2000), Sticky (1000) checkboxes; reflected in the execute column as `s`/`S`/`t`/`T`
- **File type selector** — Regular file, Directory, Symlink, Named pipe, Char device, Block device, Socket
- **Octal input** — type a 3- or 4-digit octal value (e.g. `755`, `2755`) to set the grid
- **Symbolic input** — type a 9- or 10-char symbolic string (e.g. `rwxr-xr-x`, `-rwxr-xr-x`) to set the grid
- **All inputs stay in sync** — grid, octal field, and symbolic field update each other live
- **Colorised display** — large permission string with owner bits in blue, group in teal, other in yellow; inactive bits faded
- **chmod (numeric)** — e.g. `chmod 755 <file>`
- **chmod (symbolic/absolute)** — e.g. `chmod u=rwx,g=rx,o=rx <file>`
- **Copy buttons** on every output row
- **Access summary table** — shows read/write/execute and active special bits per entity
- **20 presets** in four categories: Files, Executables, Directories, Security

**Planned:**
- Umask calculator (derive permissions from a given umask)
- `find` command snippet (`find . -perm 755`)
- `install` command snippet
- Explain mode (plain-English description of what the permissions mean)

---

### Cron (`cron.html`)

**Implemented:**
- **Multi-line crontab input** — each line is a cron entry, variable definition, blank line, or comment (`#`)
- **Live validation** — per-line status badge (Valid / Invalid / Variable / Comment / @reboot)
- **Variable definitions** — lines like `SHELL=/bin/bash`, `MAILTO=root`, `PATH=…` are detected and shown with a Variable badge
- **Commands** — the command after the schedule fields (e.g. `0 * * * * /usr/bin/backup.sh`) is parsed and displayed below the description
- **Plain-English explanation** — human-readable description for each valid expression (e.g. `0 9 * * 1-5` → "At 09:00, on weekdays (Mon–Fri)")
- **Next N run times** — configurable 1–20 upcoming scheduled times in the browser's local timezone
- **5-field and 6-field (with seconds) modes** — toggle between standard and extended cron
- **24h / 12h time display** — toggle for time formatting in explanations and run times
- **Special strings** — `@yearly`, `@annually`, `@monthly`, `@weekly`, `@daily`, `@midnight`, `@hourly`, `@reboot`
- **Named values** — month names (jan–dec) and day names (sun–sat) in any field
- **Full field syntax** — `*`, single values, ranges (`1-5`), steps (`*/5`, `1-6/2`), and comma-separated lists
- **Inline comments** — expressions like `0 2 * * * # nightly backup` are parsed and displayed correctly
- **16 preset patterns** — from "Every minute" to "Yearly (Jan 1st)"; click to append to the input
- **Copy** — copies the full analysis (descriptions + next runs) to the clipboard

**Planned:**
- Replace mode — substitute matches and preview the output crontab
- Timezone selector — show next runs in a chosen timezone
- Export next runs as plain text or CSV
- Umask-style: show what changed vs. default

---

### JWT (`jwt.html`)

**Implemented:**
- **Token input** — paste any JWT; textarea border turns green (valid) or red (invalid)
- **Colored visual breakdown** — header, payload, and signature parts rendered in distinct colors with a legend
- **Header section** — decoded header claims in a table; algorithm shown as a badge
- **Payload section** — all claims in a table with special handling for standard claims:
  - `exp` (Expires), `nbf` (Not Before), `iat` (Issued At) — displayed as formatted date/time with relative time ("Expired 2 hours ago", "in 3 days")
  - Expiry badge on the section: Valid / Expired / No expiry
  - Optional claim description column (iss, sub, aud, exp, nbf, iat, jti)
- **Signature section** — raw signature bytes shown as hex, with bit/byte count
- **JSON syntax highlighting** — keys, strings, numbers, booleans, and null each in distinct colors (VS Code palette)
- **Show raw JSON** — toggle to show the formatted, syntax-highlighted raw JSON below each section
- **HMAC signature verification** — verify HS256/HS384/HS512 tokens with a secret key using the Web Crypto API; asymmetric algorithms (RS256, ES256, etc.) show a note
- **Copy** — copies decoded header, payload, and signature hex to the clipboard

**Planned:**
- Asymmetric signature verification (RS256/ES256 with a PEM public key)
- Token builder / encoder
- Token diff view (compare two tokens side by side)

---

### Hash (`hash.html`)

**Implemented:**
- **Algorithms** — MD5 (pure JS, legacy), SHA-1 (weak), SHA-256, SHA-384, SHA-512, HMAC-SHA-256/384/512
- **HMAC** — conditional secret key input; key is UTF-8 encoded
- **Output formats** — Hex (upper or lower), Base64, Base64url
- **Compare** — paste an expected hash to verify; case-insensitive match badge
- **Bit/byte count** — shown below the output
- **Real-time update** — hash recomputed as the user types

**Planned:**
- SHA-3 family (SHA3-256, SHA3-512) via a pure-JS implementation
- File hashing (drag-and-drop)
- Multi-line / per-line hashing

---

### Encryption (`encryption.html`)

**Implemented:**
- **Algorithms** — AES-GCM-256 (default, authenticated), AES-GCM-128, AES-CBC-256, AES-CBC-128
- **Key sources** — Passphrase (PBKDF2-SHA256, 100 000 iterations, random 16-byte salt), Hex key, Base64 key
- **Passphrase show/hide** — toggle visibility of the passphrase field
- **Generate key** — random key of the correct length for the selected algorithm
- **Encoding** — Hex or Base64 for the encoded ciphertext
- **Self-contained output** — salt (passphrase mode) + IV + ciphertext concatenated in a single encoded string; no separate metadata needed for decryption
- **Session details** — derived/used key (hex), salt (hex), and IV (hex) shown after each operation with individual copy buttons
- **Commit** — after decryption, replace the input with the plaintext and switch to encrypt mode
- **CBC warning** — note shown when AES-CBC is selected (no integrity verification)

**Planned:**
- RSA-OAEP asymmetric encryption with PEM key pair generation
- File encrypt/decrypt (binary, drag-and-drop)
- Key export as PEM or JSON Web Key

---

## Planned utilities
- XML, YAML, TOML, CSV
- URL, email, phone (validator, formatter and sorter)
- Templater (Apply a template to a given dataset (simple: replace keyword in a text with attributes form input. advanced: create tables from a large dataset))
- Table (create a dynamic table with sortable columns and search filters based on a json input)
- Save settings and input to local storage
- Data mapping (input data like json, apply mapping rules, generate output)
- Image/base64 conversion

---

## Known issues
- base64 does not correctly autodetect base46URL encoded strings, you have to toggle encoding manually
