# Roadmap — City Record, Decoded

The City Record is NYC's official daily journal of legal notices, published by law (Charter §1066)
and unreadable in practice. The project makes it legible through three lenses, all reading NYC Open
Data live (no backend, no key).

## 💵 Money — contracts  *(live)*
- RFP → Intent to Award → Award($) chain, stitched by PIN.
- "How to respond" panel (deadline, contact, where to submit, PASSPort link).
- RFPs ⇄ Awards toggle; deadline + dollar filters; CSV export of any filtered list.
- **Next:** join **Checkbook NYC** (POST API, by `prime_contract_pin`) to show dollars *actually paid* + vendor track record + M/WBE + sub-vendors. Agency×category price benchmarks ("is this normal?").

## 👤 People — pay & titles  *(live)*
- **Demystify a role:** official civil-service title, **competitive (exam) vs non-competitive (no exam)**, salary band, and a career ladder of related titles by pay.
- **Look up a person:** parsed Changes-in-Personnel history (appointments / raises / promotions with dates + salary) + current Citywide Payroll (base, gross, OT).
- **Next:** precompute `data/title_crosswalk.json` (Title Code → official title + exam class + salary band) for fast browse; "biggest raises this month" leaderboard; agency turnover (appointments − departures).

## 🏗 Land — rezonings  *(planned; data spine verified)*
- Rezoning notice → **ZAP** project (open dataset `hgx4-8ukb` + ZAP-BBL `2iga-a6mk`) → applicant, units, MIH affordable housing, ULURP status.
- Geocode (Planning Labs GeoSearch) + MapPLUTO lot polygons → map of the rezoned area.
- **"Is it still standing?"** — DOB demolition permits joined by BBL (the original hook).
- Worked example: **79 Rivington Street / Allen Street Mall** rezoning (Lower East Side, CD 3).
- Filters: **ZIP code, address, or neighborhood** (resolved via Planning Labs GeoSearch → BBL).

## 🔔 Cross-lens — Subscribe / daily digest  *(mock built — live-data preview; real send pending)*
Saved filters that refresh on a schedule and email/text a digest. Useful for **both** lenses:
- Money: "all awards over **$1M**", "open RFPs in my category closing this week".
- Land: "rezonings near a **ZIP, address, or neighborhood**".
- **Demo:** the Alerts tab mocks the email/SMS digest populated with live notices and saves subscriptions to localStorage.
- **Real send (open-source):** [Apprise](https://github.com/caronc/apprise) (one lib, 100+ channels), [Novu](https://novu.co/), [Listmonk](https://listmonk.app/), [ntfy](https://ntfy.sh/); email via [Nodemailer](https://nodemailer.com/)/SMTP; SMS via [Jasmin](https://www.jasminsms.com/)/[Kannel](https://www.kannel.org/).
- Mechanics: a scheduled job (launchd/cron) re-queries the saved filter, diffs against last run, sends new items; optionally an **RSS** feed per filter (regenerate a static feed — no backend).

## 🔎 Search UX — natural-language toggle  *(maybe; may not ship)*
A checkbox flips the procurement search from explicit AND/OR filters to plain English
("construction contracts over $5M in the Bronx closing this month"). Demo path: a no-API-key
heuristic parser maps phrases → SODA filters. Production path: a small model (e.g. Claude Haiku)
does the NL → boolean conversion server-side.

## Verified data spine (all open, no key, CORS-enabled)
| Dataset | ID | Join key |
|---|---|---|
| City Record Online | `dg92-zbpx` | `pin` (procurement), `additional_description_1` (personnel/land) |
| Citywide Payroll | `k397-673e` | name (`last` + `first` prefix) + `agency_name` |
| Civil Service List (competitive titles) | `vx8i-nprf` | `list_title_code` / `list_title_desc` |
| ZAP Projects / ZAP-BBL | `hgx4-8ukb` / `2iga-a6mk` | ULURP number (normalized) / `project_id` |
| Checkbook NYC | API (`checkbooknyc.com/api`) | `prime_contract_pin` |
