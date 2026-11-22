# Roadmap

HomeNodes started as one homelab's automation map. These phases turn it into an integration
anyone can install from the Home Assistant Marketplace (HACS before 2026.11).

## 1. Own repo, with no change in behaviour (done)

- [x] Split the single 2,578-line file into ES modules, typed with JSDoc.
- [x] Strict type checking for the pure modules; the panel class under looser rules.
- [x] A made-up house for the harness and tests, replacing captured real data.
- [x] Unit tests for graph extraction, conditions, schedules, layout and formatting.
- [x] CI: forbidden-API check, tests and type check.
- [x] Parity: the old file and the new modules render byte-identical shadow DOM for the same
  house, across the overview, focused nodes, Health, Up next, collapsed areas and heat.

Debt carried forward:

- [ ] Split `homenodes-panel.js` (about 1,600 lines) into modules for the map, the inspector,
  history, trace replay and Health.
- [ ] Bring it under `tsconfig.json`'s strict rules and delete `tsconfig.panel.json`.

## 2. Remove one homelab's assumptions

- [x] **Blueprints.** Blueprint automations and scripts are expanded with
  `blueprint/substitute` (Home Assistant 2024.7, core #119890) before drawing; older versions,
  and blueprints that can't be loaded, are reported on the node and in Health.
- [x] **Grouping.** A View menu choice: area (the default: the automation's area, then a `[Tag]`
  naming an area, then where its targets are), label, category, or name tag. Each grouping
  keeps its own saved layout; layouts from before this keep the name-tag grouping.
- [x] **Time zone and locale.** Conditions and schedules run on Home Assistant's time zone.
  Times are shown in the user's language, 12 or 24 hours, and server or browser time, as their
  profile asks.
- [x] **"Defined in".** YAML or UI, from whether Home Assistant's editor API knows the
  automation, replacing the Nix-specific guess.
- [x] **Strings** through `tr()`, gettext style, with the catalog in `translations/en.json`
  generated and checked in CI. English only so far; output proven byte-identical across 17 views.
- [x] Fake-house cases: blueprint automations and scripts, a missing blueprint, YAML packages,
  script fields, labels and categories.
- [ ] Automations an integration created, if any exist in the wild.

## 3. The integration (done)

- [x] `custom_components/homenodes`: config flow, single instance, `hacs.json` requiring Home
  Assistant 2024.12 (options flows get their entry from it).
- [x] The frontend served from `/homenodes_files/<version>-<hash of its files>/`, so every
  module is cache-busted by a release, or by any change to a file. Not under `/homenodes/`:
  the frontend answers every path below a panel's URL with its own index page. An admin-only
  panel, removed on unload.
- [x] Options: controls (off by default), default grouping, heat window, labels to hide (never
  read, not just not drawn). The panel checks them again, and with no config behaves as before:
  17 views byte-identical.
- [x] Diagnostics with versions, option values and counts; label ids are counted, not listed.
- [x] Tests with `pytest-homeassistant-custom-component`, plus a dev shell in `flake.nix`.
- Dropped: a sidebar title and icon. Home Assistant's own sidebar editing
  (`frontend/update_panel`) overrides what an integration registers, so they would look broken.
- Kept per user: condition status, in the View menu, as it was.

## 4. Safety (done)

- [x] A read-only mode: controls are off unless the integration's options allow them.
- [x] A confirmation before "Run actions", in place, with Cancel focused. Toggling was already
  kept to lights, switches, fans and toggle helpers.
- [x] At most 50 live template subscriptions (the rest are listed in Health), each with Home
  Assistant's 3-second render limit, pruned when unused, and none at all in a background tab.
- [x] History, logbook and trace queries give up after a time limit, show why, and a reply
  overtaken by a newer request is ignored. Home Assistant can't cancel a query, so this is
  as cancellable as it gets.
- [x] Saved layouts parsed by `saved.js`: any version, bounded in size, and a bad value
  costs only itself. Prototype keys can't reach anything.
- [x] A hostile house (`?hostile`): markup, attribute breakers, right-to-left overrides, a
  600-character name, templates, `__proto__` and `constructor` as names. Node tests over the
  graph and layout, and `npm run browser`, which clicks through it in headless Chrome and fails
  on any foreign element, `on…` attribute, script or error. It found an area called
  `__proto__` vanishing into another zone, and an automation variable named like a regular
  expression stopping all condition checks; both fixed.
- [x] Right-to-left names placed in a sentence are wrapped in Unicode isolates, so they can't
  reverse the words around them.

## 5. Scale (done)

- [x] Performance budgets, checked in CI: `scripts/bench.mjs` for graph, layout and conditions at
  2,000 automations, and `scripts/browser-bench.mjs` for loading, state changes, the clock,
  panning, zooming and focusing at 500, in headless Chrome. `--large` adds 2,000 there, which
  needs a GPU; headless software painting can't keep up with that much.
- [x] Templates are split into dotted words in one pass instead of searched for every known
  entity, and zoning reads each node's own wires instead of every wire. Graph building at
  2,000 automations went from 121 ms to 26 ms, and now grows in line with the house.
- [x] State updates are painted once per animation frame. Off-screen nodes and wires get
  `display: none` and catch up when they scroll back. Label measurements are cached, and an
  unchanged label isn't measured at all.
- [x] Below 0.2× zoom, idle wires and node details are hidden: at that size they were a
  hairball, and only wires carrying current or a run are worth seeing.

At 2,000 automations, in a browser with a GPU (before → after): loading 1,269 → 716 ms, a state
change 24 → 5.6 ms, the clock tick 34 → 7 ms, focusing an automation 461 → about 220 ms.

## 6. Releases

- [x] `hassfest` and HACS validation in CI (both untried until the repository is on GitHub).
- [ ] Screenshot tests, a weekly run against Home Assistant beta.
- [ ] Tagged releases with notes and provenance.

## 7. Docs and accessibility

- [x] A documentation site (MkDocs Material on GitHub Pages, built strictly in CI): installing,
  options, a guide to the map, troubleshooting, privacy. Screenshots are taken from the harness
  on every build, so they show the current version and never a real home.
- [ ] Keyboard navigation between nodes, and a list view for screen readers.

## 8. Rollout

- [x] A beta through HACS as a custom repository (0.1.0).
- [x] Install instructions for the Marketplace, which replaced HACS in Home Assistant 2026.11.
- [ ] An icon in `custom_components/homenodes/brand/icon.png`, so CI can stop ignoring the
  brands check.
- [ ] The Marketplace catalog (still a pull request to `hacs/default`).

## Later, maybe

- **Wires between zones.** Automations that act across zones make long, crossing wires. Placing
  connected zones next to each other would shorten them; reroute points, like Blender's, would
  let people route them by hand. Tidying those automatically is hard, so this waits.
