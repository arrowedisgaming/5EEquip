# PHB Vendor — D&D 5e Equipment Shop

## Architecture

Single-file web app (`index.html`). No build tools, no dependencies, no server required. Just open in a browser.

- **HTML** structure: header, sticky controls bar, 2-column grid (catalog + cart sidebar)
- **CSS** (~400 lines): Parchment theme using CSS custom properties. Responsive: desktop grid, mobile single-column with slide-in cart drawer.
- **JavaScript** (~950 lines): Item data array, catalog rendering, cart logic, localStorage persistence.

## Data Model

All 241 SRD v5.2.1 equipment items stored as `const ITEMS = [...]`. Costs in **copper pieces** (integers) to avoid floating-point issues:

| 1 CP = 1 | 1 SP = 10 | 1 GP = 100 | 1 PP = 1000 |

Item shape: `{ id, name, section, category, cost, costDisplay, weight, url, ...typeSpecific }`.

### Sections & Categories

| Section | Categories |
|---------|-----------|
| Weapons | Simple Melee (10), Simple Ranged (4), Martial Melee (18), Martial Ranged (6) |
| Armor | Light (3), Medium (5), Heavy (4), Shield (1) |
| Tools | Artisan's (17), Other (6), Gaming Sets (4), Musical Instruments (10) |
| Gear | Adventuring Gear (~70), Ammunition (5), Arcane/Druidic/Holy Focuses (11), Equipment Packs (7) |
| Mounts & Vehicles | Mounts (8), Tack/Vehicles (10), Ships (7) |
| Services | Food/Drink/Lodging (17), Hirelings (3), Spellcasting (7), Lifestyle (7) |

## D&D Beyond URLs

Items link to `https://www.dndbeyond.com/equipment/{id}-{slug}` where verified. Unverified items use the search fallback: `https://www.dndbeyond.com/equipment?filter-search={name}`.

## Key Functions

- `renderCatalog()` — Builds `<details>` sections with item cards, respects search/filter state
- `renderCart()` — Renders cart items, totals, encumbrance bar
- `cpToDisplay(cp)` — Converts copper pieces to mixed denomination string (e.g., "12 GP, 5 SP")
- `getSearchableText(item)` — Builds searchable text from all item fields (name, damage, properties, mastery, etc.)
- `matchesFilter(item)` — Checks item against active section tab and search query
- `saveCart() / loadCart()` — localStorage persistence for cart and STR score

## Conventions

- Item IDs prefixed by type: `w-` weapons, `a-` armor, `t-` tools, `g-` gear, `m-` mounts/vehicles, `s-` services
- Event delegation: single click listener on `#catalog` and `#cartItems` containers
- CSS uses `var(--property)` custom properties throughout for consistent theming
- Encumbrance: STR x 15 lb. capacity. Green (0-75%), yellow (75-100%), red (over 100%)

## Data Source

Equipment data from `SRD_CC_v5.2.1.pdf` (pages 89-101), licensed CC-BY-4.0 by Wizards of the Coast.
