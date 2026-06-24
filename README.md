# City Record Money Map

**Making *The City Record* legible.**

[The City Record](https://a856-cityrecord.nyc.gov/) is the official daily journal of the City of
New York. By law (City Charter §1066), every agency must publish its procurement solicitations,
contract awards, public hearings, land-use actions, and personnel changes there. It is, in effect,
NYC's daily newspaper of government — and almost nobody can read it. It arrives as a dense stream of
disconnected legal notices: a Request for Proposals here, an award months later there, a rezoning
written entirely in metes-and-bounds.

This project re-links those notices into something a person can actually follow.

## Why this matters: PASSPort vs. the public record

NYC runs procurement through **[PASSPort](https://www.nyc.gov/site/mocs/passport/about-passport.page)**
(the Mayor's Office of Contract Services vendor portal) — vendors enroll, get matched to opportunities,
and respond there. But PASSPort is a gated workflow tool. The **City Record is the open, legal
record of public notice**: every opportunity at or above $100,000 must be published there.

So the people who depend on the City Record are exactly the ones *not* wired into PASSPort — an
outside consultant sizing up an agency, a journalist, a researcher, a small vendor who missed the
portal alert. For them, a single RFP notice is nearly useless: it doesn't say how much that agency
actually awards for this kind of work, who won last time, or whether an award ever landed.

## v0 — Follow the money (procurement)

The live tool reads the City Record straight from NYC Open Data and, for any RFP, stitches the
scattered notices that share a **Procurement Identification Number (PIN)** back into one chain:

> **Solicitation (RFP) → Intent to Award → Award ($)**

Each box links back to the real notice in the City Record, and an agency context strip shows how
much that agency has awarded on record — turning one isolated notice into a sense of the whole flow.

**It's a single `index.html`** — vanilla JS, no build step, no backend. It fetches the public SODA
API (CORS-open) directly from the browser, so it runs from a double-click or from GitHub Pages.

## Roadmap — the other lenses

The same "make the record legible" move applies across the paper:

- **Follow the people** — *Changes in Personnel* is the single largest section of the City Record
  (~955K notices), each carrying a name, title, action, and salary. Cross-referenced with
  [Citywide Payroll Data](https://data.cityofnewyork.us/City-Government/Citywide-Payroll-Data-Fiscal-Year-/k397-673e),
  it answers "who got appointed, and what does everyone in NYC get paid?"
- **Follow the land** — City Planning rezoning / ULURP notices are unreadable legalese
  (`R7-1 → R8`, MIH options, metes-and-bounds). Cross-referencing them to maps, addresses, and
  applicants makes them intuitive.

## Data sources

| Source | What it gives |
|---|---|
| [City Record Online — `dg92-zbpx`](https://data.cityofnewyork.us/City-Government/City-Record-Online/dg92-zbpx) | Every published notice (structured form of the printed paper) |
| [Recent Contract Awards — `qyyg-4tf5`](https://data.cityofnewyork.us/City-Government/Recent-Contract-Awards/qyyg-4tf5) | Award detail derived from the City Record |
| [Citywide Payroll Data — `k397-673e`](https://data.cityofnewyork.us/City-Government/Citywide-Payroll-Data-Fiscal-Year-/k397-673e) | Per-employee title, salary, and gross pay |
| [Checkbook NYC](https://www.checkbooknyc.com/) | Actual dollars paid against contracts |

## Honest caveats

The underlying data is messy and the tool says so on its face: some notices carry placeholder
PINs that can't be linked; some PINs are "blanket" codes bundling many unrelated awards; and raw
contract amounts contain erroneous mega-values (agency totals exclude rows above $5B). Always
confirm a figure against the [official City Record](https://a856-cityrecord.nyc.gov/) and
[Checkbook NYC](https://www.checkbooknyc.com/).

## Run it

```
open index.html      # or just double-click — no server, no build
```

Live: **https://jimdc.github.io/city-record-money-map**
