# ArusGrid data

The dataset behind [arusgrid.com](https://arusgrid.com), a map of the AI data center buildout tracked project by project. 522 active projects across Malaysia, Singapore, Indonesia, Thailand, Vietnam, the Philippines, the UAE, Saudi Arabia, Qatar, Bahrain, Kuwait, India and China's national computing hubs. 38.6 GW of planned IT load as of October 2026.

For each project: where it stands in the construction lifecycle (announced, land and permitting, financing closed, under construction, commissioning, operational, expanding, on hold, cancelled), who builds it, who funds it, who operates it, who buys the capacity, how much IT load it carries, the capital behind it, and what power costs. Every stage move carries a dated source. Every project carries an evidence tier computed by fixed public rules, described at https://arusgrid.com/methodology.

## Files

`data.json` is the single source of truth: `meta` (regions, markets with published vacancy, country tariffs, capital deals, chip export-control frameworks) and `facilities[]`. `colo.json` holds colocation prices, kept separate so a price refresh never touches a project record. `policy.json` holds the country rules pages. See `SCHEMA.md` for the fields.

The files update daily from the live site through the workflow in `.github/workflows/sync.yml`. The live copies are always at https://arusgrid.com/data.json, https://arusgrid.com/colo.json and https://arusgrid.com/policy.json.

## Reading the numbers

MW figures are IT load unless `power.mw_basis` says otherwise (`total_power`, `grid`, `derived_it_load`). Unknown figures are omitted, never estimated. `live_mw` null means the operator has not disclosed it, which is most of them. Chinese rows may carry rack counts instead of MW; a rack count is never converted to MW. Market vacancy appears only where a broker published it. Cancelled projects are kept with stage `cancelled`. Sources are objects with url, title, date and publisher; older rows carry bare URLs.

```python
import json, urllib.request
d = json.load(urllib.request.urlopen("https://arusgrid.com/data.json"))
rows = [f for f in d["facilities"] if f["stage"] != "cancelled"]
print(len(rows), sum((f.get("power") or {}).get("planned_mw") or 0 for f in rows))
```

## Licence and contact

CC BY-NC 4.0 (see `LICENSE.md`). Commercial use by arrangement: hello@arusgrid.com. Corrections: corrections@arusgrid.com, or the form on any project page at arusgrid.com.
