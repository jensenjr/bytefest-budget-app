# Changelog

All notable changes to **Event-budgetizer** are documented here.

The project follows [Semantic Versioning](https://semver.org/) (`MAJOR.MINOR.PATCH`).
It is in early development (`0.x`) — anything may change until `1.0.0`.

## [0.1.0] — 2026-06-02

First named release. Renamed from "Event Budget Calculator" / ByteFest to **Event-budgetizer**.

### Added
- **File-based budgets** — open and save your budget as a real `.xlsx`, either on your
  computer (File System Access API, with a download/upload fallback) or in **Google Drive**
  (Google Identity Services + Picker + Drive REST v3). draw.io-style: the app is the editor,
  the file is your data.
- **Readable workbook** — `Income`, `Costs`, `Settings` and a computed `Summary` tab, so the
  file makes sense to anyone you share it with, no app required.
- **Browser draft autosave** — the working budget is kept in `localStorage`, so a page
  refresh never loses your work. The `.xlsx` remains the durable, explicit copy.
- **Slider settings** — a setting to control the quantity-slider maximum (`0` = automatic),
  so the sliders are meaningful for your event size.
- **Timestamped filenames** — new files are named `<name>_YYYY-MM-DD_HH-mm.xlsx` to avoid
  silently overwriting an existing file.
- **Swedish UI** — all in-app text, buttons, and guidance are in Swedish.
- **Version footer** showing the running version.

### Notes
- Google Drive requires a one-time Google Cloud setup (OAuth client ID + API key); see the
  README. Local open/save needs no setup.
