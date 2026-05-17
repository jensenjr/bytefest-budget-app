# Event Budget Calculator

A single-file, zero-dependency budget planning tool for LAN parties and similar
ticketed events. Open the HTML file in any browser — no build step, no server.

## Features

- **Real-time calculations** — every number updates instantly as you type or drag
- **Multiple ticket types** — configurable names, prices, and per-ticket variable costs
- **Break-even analysis** — with and without sponsor income
- **Contribution margin** — shows how much each average ticket contributes toward fixed costs
- **Goal progress bars** — track tickets sold and sponsor income against your targets
- **Fully in-browser** — React + Babel loaded from CDN; works offline after first load

## How to run

1. Download or clone this repository
2. Open `index.html` in a web browser
3. That's it — no npm, no build, no server required

## How to customize

All customization is done through the **Inställningar** tab inside the app:

| Setting | Where |
|---|---|
| Ticket type names | Inställningar → Biljetttyper |
| Ticket prices | Inställningar → Biljetttyper |
| Per-ticket variable cost (seat rental, goody bag, etc.) | Inställningar → Biljetttyper |
| Count slider range | Inställningar → Biljetttyper |
| Target ticket count | Inställningar → Planeringsmål |
| Target sponsor income | Inställningar → Planeringsmål |
| Fixed overhead costs | Kostnader tab |

To pre-fill the app with your event's data, edit the `initTicketTypes` and
`initFixed` arrays near the top of the `<script>` block in `index.html`.

## Tabs

| Tab | Purpose |
|---|---|
| **Intäkter** | Set ticket counts via sliders, enter sponsor income, add extra revenue lines |
| **Kostnader** | Enter fixed overhead costs (venue, equipment, etc.) |
| **Rapport** | Break-even analysis, contribution margin, goal progress bars |
| **Inställningar** | Configure ticket types, prices, variable costs, and planning targets |

## How break-even is calculated

```
contributionMargin = avgTicketPrice − avgVariableCostPerTicket

breakEvenTickets (with sponsor) = (fixedCosts − sponsorIncome) / contributionMargin
breakEvenTickets (no sponsor)   = fixedCosts / contributionMargin

sponsorNeeded = fixedCosts + targetTickets × avgVarCost − targetTickets × avgTicketPrice
```

The weighted average ticket price and variable cost are computed from the actual
sales mix (how many of each ticket type are entered). When no tickets are entered
yet, a simple mean across all types is used as a fallback.

## Tech stack

- [React 18](https://react.dev/) via CDN (UMD build)
- [Babel Standalone](https://babeljs.io/docs/babel-standalone) for in-browser JSX
- Vanilla CSS — no framework

## License

MIT
