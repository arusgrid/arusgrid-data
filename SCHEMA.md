# Schema

`data.json` has two top-level keys, `meta` and `facilities`.

## meta

`last_updated` (YYYY-MM-DD). `regions` (rollup above country; a region with `live: false` is scaffolded only). `markets` keyed by market name: `vacancy_pct`, `basis`, `as_of`, `source`, only where a broker published a figure. `tariffs` keyed by country code: `usd_per_kwh`, `note`, `as_of`; null where no single country figure is honest (UAE, Saudi Arabia, India). `deals[]`: the capital layer (country programmes, platform M&A, platform capital, debt, power, anchor leases), each with `type`, `amount_usd_m`, `facilities[]`, `countries[]`, `as_of`, `source`. `chip_controls`: the US export-control frameworks per country and the list of entities headquartered in China or Macau. `checked_empty`: countries researched with nothing above the inclusion bar. `country_notices`: dated country-level conditions. `cost_bands`: build cost per watt by country used for estimated value.

## facilities[]

`id` (stable public slug, also the project URL at arusgrid.com/project/<id>), `name`, `campus`, `country` (ISO code), `region`, `city`, `market`, `lat`, `lng`, `coords_precision` (exact, district, city), `aliases[]`.

`stage`: announced, land_permitting, financing_closed, under_construction, commissioning, operational, expanding, on_hold, cancelled. `stage_history[]`: append-only `{stage, date, source}`, the date the source says the move happened. `phases[]`: `{label, mw, stage, target}`. `fate`: on cancelled or on-hold rows, `{reasons[], summary, date, source}`.

`power`: `planned_mw`, `live_mw` (null when undisclosed), `mw_basis` (it_load default, total_power, grid, derived_it_load), `racks_planned`, `racks_live`, `racks_note` (China). `power_cost`: `usd_per_kwh`, `source` (country_tariff, public_ppa, estimate).

`investment`: `amount`, `currency` (ISO), `unit`, `currency_raw` (the original string), `amount_usd_m`, `scope` (facility, campus, multi_facility, land_only), `basis` (announced, govt_approved, loan_amount, gdv, estimate), `includes_it_hardware`, `as_of`.

`operator`: `{name, type, group}` with type colo, hyperscaler_selfbuild, telco, local_developer, jv. `funders[]`: `{name, type, role}` with role equity, debt, jv_partner. `clients[]`: `{name, type, status, role, mw, scope, as_of, basis, source}` with status confirmed, reported, rumored and role tenant, compute_user, own_use. `clients_checked`: date a search found no named user.

`ai_status`: confirmed, likely, unlikely, unknown, with `ai_evidence`.

Build facts, one row per fact, each with `evidence` (confirmed, reported, inferred), `basis` when inferred, and a dated `source` object: `contractors[]` (role, scope, value, status), `land[]` (counterparty, relationship, area, price, tenure), `power_supply[]` (provider, arrangement, capacity, voltage_kv), `water_cooling[]` (source_type, supplier, volume, cooling, pue_design, wue), `approvals[]` (type, authority).

`sources[]`: `{url, title, date, publisher}`; older rows carry bare URL strings. `last_verified`: the date the row was last checked against sources.

## colo.json

`markets[]` with `points[]`, each a price with `price_class` (wholesale_asking, passing, retail_rack), `band`, `power` (excluded, included, unstated), `as_of`, `source`, `commissioned_by`. `sites[]` for building-level figures. `empty[]` for markets searched with nothing published. `log.held` and `log.rejected` for candidates that failed review, with reasons.

## policy.json

`countries` keyed by country code: `items[]` `{title, text, status (in_force, proposed, paused, lapsed, unclear), effective, authority, sources[]}` grouped by section (approvals, power, incentives, data, water, clock), `authorities[]` ranked by power over whether a project gets built, and `not_confirmed[]`.
