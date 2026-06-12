# Álbum Mundial 2026

## Stack
- **Zero-dependency vanilla HTML/CSS/JS** — no build, no framework, no packages.
- Two files: `index.html` (main app), `pendientes.html` (read-only share view).

## Dev server
Open any HTML file directly in browser — no server needed. Or use any static server:
```bash
python3 -m http.server 8080
```

## Data model
- 48 countries in 12 groups (A–L), 4 countries/group, 20 stickers/country = 960 + 33 extras (FWC 19, Coca-Cola 14) = **993 total**.
- Album state: `{ "A_México": Set([1,3,5]), ... }` stored as `{ stickers, duplicates }` in JSONBin + localStorage.
- Duplicate state: `{ "A_México": { "1": 2, "5": 1 }, ... }` (sticker → count).
- View modes: `'album'` and `'duplicates'` controlled by `viewMode` global.

## Persistence
- **Primary**: JSONBin.io (`BIN_ID=6a1b5856ddf5aa59f779a799`, `MASTER_KEY` hardcoded in both files).
- **Fallback**: `localStorage` keys `mundial2026_album` and `mundial2026_duplicates`.
- Save debounced at 800ms.

## Key conventions
- Password gate: hardcoded `fbb2019`, stored in `localStorage` key `album_auth`.
- All text in Spanish.
- Stickers collected = green (`collected` class), duplicates = amber (`duplicate` class).
- When adding/editing sticker behavior, both `buildGroupCard()` and `buildExtras()` must handle both `viewMode` values.
- Group/country stats read from `getCollected()` (album) or `getDupCount()` (duplicates) depending on `viewMode`.

## Important globals
| Variable | Type | Purpose |
|---|---|---|
| `state` | `{key: Set}` | Album collected stickers |
| `dupState` | `{key: {num: count}}` | Duplicate sticker counts |
| `viewMode` | `'album'|'duplicates'` | Current tab |
| `GROUPS` | `[{id, countries}]` | All group/country data |
| `COUNTRY_CODES` | `{name: code}` | 3-letter codes for text output |
| `EXTRAS` | `[{name, flag, count}]` | FWC (19), Coca-Cola (14) |

## Architecture notes
- All logic is in a single `<script>` block per file — no modules, no imports.
- `pendientes.html` is a near-duplicate of `index.html` without click handlers; keep in sync when adding features.
- UI is built imperatively via `buildUI()`/`buildExtras()` — no reactivity; rebuild on mode switch.
- Modal pattern is consistent: `.modal-overlay` with `onclick="closeModal(event)"` + inner `.modal` with `onclick="event.stopPropagation()"`.

## Common tasks
- **Add a section**: new function reading from `state`/`dupState`, add button in the `actions` div, add modal if needed.
- **Add a country/group**: edit the `GROUPS` and `COUNTRY_CODES` arrays.
- **Change sticker count**: update `STICKERS_PER_COUNTRY` or extras `count`.
