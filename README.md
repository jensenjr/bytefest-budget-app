# Event-budgetizer

> ⚠️ **Early development — v0.1.0.** See the [changelog](CHANGELOG.md); things may change between versions.
> The app UI is in Swedish for now (an English version is planned).

A single-HTML budget planning tool for LAN parties and similar ticketed events.

It works like draw.io: the app is just an **editor**, and your budget is a **real `.xlsx`
file** that you own and store wherever you like — on your computer or in Google Drive. Share
that file (read-only) and anyone can read the budget in Excel, Google Sheets, or Drive's
preview, without needing your app.

## Features

- **Flexible income and cost posts** — each post is either fixed (flat amount) or per-unit (amount × quantity)
- **Linked quantities** — per-unit cost posts can follow the quantity of any per-unit income post automatically
- **Real-time break-even analysis** — contribution margin table and break-even unit count update as you type
- **Goal tracking** — progress bars for unit sales and revenue against configurable targets
- **Your data is a real `.xlsx` file** — Open / Save it on your computer or in Google Drive; the file has readable `Income`, `Costs`, `Settings` and `Summary` tabs
- **Mostly in-browser** — React + Babel + SheetJS loaded from CDN; Google Drive is optional

## How to use

The app opens to an empty budget. Use the toolbar:

- **📂 Öppna (Open)** → *Från den här datorn* (this computer) or *Från Google Drive*
- **💾 Spara (Save)** → saves back to the file you have open
- **Spara som (Save As)** → save a new file *to this computer* or *to Google Drive*
- **✨ Ny (New)** → start a fresh, empty budget

### Where your budget is stored

There's no database and no server holding your data — your budget *is* the `.xlsx` file.

| Tab | Contents |
|---|---|
| `Income` | one row per income post (`id, name, type, amount, quantity`) |
| `Costs` | one row per cost post (`id, name, type, amount, quantity, linkedTo`) |
| `Settings` | event name, goals, and bookkeeping (`nextId`, `savedAt`) |
| `Summary` | computed totals & break-even as plain values, for anyone viewing the file |

To **let people look at it**: in Google Drive, *Share → Anyone with the link → Viewer* on the
`.xlsx`. They'll see the budget; only you (with edit access) can change it.

### Hosting

Deploy the single `index.html` to any static host (Netlify, Cloudflare Pages, GitHub Pages,
Vercel). For local testing, serve it over `http://localhost` (e.g. `python -m http.server 5050`)
— the file pickers need a served page, not a `file://` URL.

### Connect Google Drive (one-time setup)

Local Open/Save needs no setup. To enable the Google Drive buttons:

1. In the [Google Cloud Console](https://console.cloud.google.com/), create a project and
   **enable the *Google Drive API* and *Google Picker API***.
2. **Create an OAuth Client ID** (type: *Web application*). Under *Authorized JavaScript
   origins*, add your host URL (and `http://localhost:5050` for local testing).
3. **Create an API key**, then restrict it by *HTTP referrer* (your host) and to the Drive +
   Picker APIs.
4. Configure the **OAuth consent screen** (*External*); add yourself as a *test user* (or
   publish the app).
5. Open `index.html` and paste your values into the config constants near the top of the
   `<script type="text/babel">` block:
   ```js
   const GOOGLE_CLIENT_ID = "....apps.googleusercontent.com";
   const GOOGLE_API_KEY   = "....";
   ```
   These are *public* values (not secrets); the API-key referrer restriction is what protects
   them. The app requests only the `drive.file` scope, so it can touch **only** files it
   creates or that you explicitly pick — never your whole Drive.

## Data model

### Income posts
Each income post has a **name**, an **amount**, and a **type**:
- **Fast** — a fixed amount regardless of units sold (e.g. a sponsorship: always 10 000 kr)
- **Per enhet** — amount × quantity (e.g. a ticket type: 495 kr × 200 sold = 99 000 kr)

### Cost posts
Each cost post has a **name**, an **amount**, and a **type**:
- **Fast** — a fixed overhead cost (e.g. venue rental: 13 000 kr)
- **Per enhet** — amount × quantity, where the quantity is either:
  - **Linked** to a per-unit income post — the cost automatically follows that post's quantity (e.g. table rental linked to ticket sales)
  - **Manual** — a standalone quantity you set yourself

## Tabs

| Tab | Purpose |
|---|---|
| **Intäkter** | Add and manage income posts (fixed or per-unit) |
| **Kostnader** | Add and manage cost posts; link per-unit costs to income posts |
| **Rapport** | Break-even analysis, contribution margin table, goal progress bars |
| **Inställningar** | Set the event name, unit sales goal, and revenue goal |

## How break-even is calculated

```
contributionMargin(post) = pricePerUnit − sum(linkedCostPostsPerUnit)

weightedMargin = sum(margin × qty) / sum(qty)   ← weighted by actual quantities

uncoveredFixed = fixedCosts + standalonePerUnitCosts − fixedIncome

breakEvenUnits = uncoveredFixed / weightedMargin
```

Standalone per-unit costs (not linked to an income post) are treated as fixed for
break-even purposes, since their quantity doesn't scale with unit sales.

## Tech stack

- [React 18](https://react.dev/) via CDN (UMD build)
- [Babel Standalone](https://babeljs.io/docs/babel-standalone) for in-browser JSX
- [SheetJS](https://sheetjs.com/) for reading/writing `.xlsx` in the browser
- [Google Identity Services](https://developers.google.com/identity), [Drive API v3](https://developers.google.com/drive/api) & [Picker](https://developers.google.com/drive/picker) for optional Google Drive storage
- File System Access API for local Open/Save (with a download/upload fallback)
- Vanilla CSS — no framework

## License

MIT
