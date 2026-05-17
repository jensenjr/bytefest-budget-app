# Event Budget Calculator

A single-file, zero-dependency budget planning tool for LAN parties and similar
ticketed events. Open the HTML file in any browser — no build step, no server.

## Features

- **Flexible income and cost posts** — each post is either fixed (flat amount) or per-unit (amount × quantity)
- **Linked quantities** — per-unit cost posts can follow the quantity of any per-unit income post automatically
- **Real-time break-even analysis** — contribution margin table and break-even unit count update as you type
- **Goal tracking** — progress bars for unit sales and revenue against configurable targets
- **Fully in-browser** — React + Babel loaded from CDN; works offline after first load

## How to run

1. Download or clone this repository
2. Open `index.html` in any web browser
3. That's it — no npm, no build, no server required

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
- Vanilla CSS — no framework

## License

MIT
