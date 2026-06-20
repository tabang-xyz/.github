# Handoff: Tabang Design System & Service Consoles

## Overview
**Tabang** (Tagalog: *to help, to aid*) is a parent brand for a stack of AI‑native
support products aimed at Filipino businesses. Four services live under it —
**Tugon** (support), **Daloy** (integrations), **Hatid** (licensing),
**Bayad** (billing) — plus **Susi** (access/keys). This package contains the
brand system, a marketing landing page (light + dark), and the **Tugon support
console** — a reusable admin shell (sidebar · top bar · table · detail panel)
intended to be the template for *every* service's back‑office UI.

## About the Design Files
The `.dc.html` files in this bundle are **design references created in HTML** —
prototypes that show intended look, layout, and behavior. They are **not
production code to ship directly.** The task is to **recreate these designs in
the target codebase's environment** (the Tabang services use **React + Vite +
Tailwind + shadcn/ui**) following its established patterns, components, and
tokens. If a component library exists, map the primitives below onto it
(e.g. shadcn `Badge`, `Button`, `Avatar`, `Table`) rather than re‑implementing.

> The `.dc.html` files use a small in‑house runtime (`support.js`). To preview a
> file, open it in a browser — they render standalone. Ignore `support.js` and
> the `<x-dc>`/`data-props` wrappers when porting; only the markup, styles, and
> structure matter.

## Fidelity
**High‑fidelity.** Colors, typography, spacing, and component states are final.
Recreate pixel‑for‑pixel using the codebase's libraries. All hex values,
dimensions, and copy below are authoritative.

---

## Brand foundation

### Logo system — Baybayin
Each product's mark is the first syllable of its name written in **Baybayin**
(pre‑colonial Philippine script), set in a rounded‑square ("squircle") tile.
The glyph is always the near‑white **cream** on a colored tile.

| Product | Syllable | Unicode | Accent tile |
|---|---|---|---|
| **tabang** (parent) | Ta — ᜆ | U+1706 | Mustard Gold |
| tugon | Tu — ᜆᜓ | U+1706 U+1713 | Violet |
| daloy | Da — ᜇ | U+1707 | Teal |
| hatid | Ha — ᜑ | U+1711 | Green |
| bayad | Ba — ᜊ | U+170A | Caramel |
| susi | Su — ᜐᜓ | U+1710 U+1713 | Green (shares Hatid) |

- Ready‑made assets are in **`tabang-brand/`**:
  - `logos/<name>.svg` — colored tile + white glyph (512×512, scalable). Plus `logos/tabang-reversed.svg` (white tile, gold glyph) for dark/photo backgrounds.
  - `glyphs/<x>-<name>.svg` — bare glyph, no background, `fill: currentColor`.
  - **All glyphs are pure vector outlines — no font dependency.** Use these SVGs directly for app icons / favicons.
- Tile geometry: `border-radius: 22%` of the tile size. Glyph optically centered.
- The Latin wordmark is always lowercase (`tugon`, `tabang`), weight 800, `letter-spacing: -0.02em`.

### Typography
- **Hanken Grotesk** (Google Fonts, weights 400/500/600/700/800) — the *only*
  typeface in the system; used for wordmarks, UI, and body. Load via
  `tabang-brand/fonts.css`-style `@import`, or self‑host.
- Baybayin glyphs are vector SVGs (above), **not** a runtime font.

### Color — foundation (warm stone)
The neutral skeleton is shared by every product. Light mode uses the low end as
surfaces; dark mode inverts.

| Token | Hex | Role |
|---|---|---|
| `--stone-50`  | `#faf9f9` | Canvas (light bg) |
| `--stone-100` | `#f5f5f4` | Surface / sidebar |
| `--stone-200` | `#e7e5e4` | Sunken / hairline border |
| `--stone-300` | `#d6d3d1` | Border‑strong |
| `--stone-400` | `#a8a29e` | Muted / faint text |
| `--stone-500` | `#78716c` | Muted text |
| `--stone-600` | `#57534e` | Body text |
| `--stone-700` | `#44403c` | Heading / dark border |
| `--stone-800` | `#292524` | Ink / dark surface |
| `--stone-900` | `#1c1917` | Night / dark canvas |
| `--cream` | `#f6f2ec` | Warm off‑white text on accents |

### Color — accents (one per product = its logo color)
| Token | Hex | oklch | Text on accent |
|---|---|---|---|
| `--accent-tabang` | `#dcb13b` | `oklch(.78 .14 88)` | **BLACK** `#241e1a` |
| `--accent-tugon`  | `#7f66bc` | `oklch(.575 .13 295)` | white/cream |
| `--accent-daloy`  | `#2198a4` | `oklch(.625 .10 205)` | white/cream |
| `--accent-hatid`  | `#479c6b` | `oklch(.63 .115 155)` | white/cream |
| `--accent-bayad`  | `#986539` | `oklch(.55 .09 60)` | white/cream |
| `--accent-susi`   | `#479c6b` | (shares Hatid) | white/cream |

> ### ⚠️ The one text rule — must be preserved
> **Tabang is the ONLY brand with BLACK text/CTA labels on its accent** (the gold
> is light). **Every other product uses WHITE/cream text on its accent, in BOTH
> light and dark mode.** This is encoded in `tabang-brand/tokens.css` as the
> `--on-*` variables; always read `--on-accent`, never hardcode.

### Theming mechanism
`tabang-brand/tokens.css` sets `--accent` / `--on-accent` from a
`data-brand="tugon|daloy|hatid|bayad|tabang"` attribute on a root container, and
inverts the stone scale under `data-theme="dark"`. Port this to the codebase's
theming approach (CSS vars, Tailwind theme, or context).

---

## Screens / Views

### 1. Marketing landing — `Tabang Web Example.dc.html`
Parent‑brand landing, shown in **light and dark**.
- **Layout**: centered single column inside a browser frame (680px content).
  Nav → hero (centered) → products grid → footer.
- **Nav**: logo + `tabang` wordmark, links (Products, Pricing, Docs), `Sign in`
  text, **Get started** button (gold `#dcb13b`, black text, radius 9px, 9×16 pad).
- **Hero**: eyebrow (uppercase, gold, 12px/700/0.16em), `h1` 46px/800/‑0.03em
  line‑height 1.05 max‑width 520, sub paragraph 16.5px stone‑600/muted,
  two CTAs: primary gold/black `Start free`, secondary ghost `Book a demo`.
- **Products grid**: 5 columns, each a card (surface, 1px border, radius 12,
  18×10 pad) with the service logo SVG (38px), name (13px/700), tag (11px muted).
- **Footer**: small logo + `© 2026 Tabang · Made in the Philippines`.
- Light surfaces: bg `#faf9f9`, cards `#faf9f9`/white, borders `#e7e5e4`.
  Dark: bg `#1c1917`, products band `#211d1a`, cards `#292524`, borders `#44403c`.

### 2. Tugon support console — `Tugon Console.dc.html`  (the reusable shell)
A full‑screen back‑office layout, shown in **light and dark**. **This is the
template for all service admin pages** — only the table columns, pills, and
detail‑panel contents change per service.

**Frame**: app window, radius 14, border `#d6d3d1`(L)/`#44403c`(D),
shadow `0 22px 60px rgba(40,32,28,.16)`(L) / `rgba(0,0,0,.4)`(D). Optional
traffic‑light window chrome on top (cosmetic).

**Global top bar** — one full‑width strip, **height 62px**, split into 3 segments
by vertical dividers that line up with the columns beneath:
- **Left segment** (width **236px**, border‑right): product logo SVG (28px) + wordmark (17px/800).
- **Middle segment** (flex): filter tab group on the left — pill container
  (bg `#f0efed`(L)/`#2a2521`(D), radius 10, 4px pad) with `All` active
  (white/`#38332e` chip, 1px border, radius 7, 5×12) and `Open 24` / `Pending 8`
  / `Resolved` inactive (13px, 600). Then a spacer, a search field
  (34px tall, radius 9, width 220, placeholder "Search conversations…"), and the
  violet **Compose** button (`#7f66bc`, white text, radius 9, height 34).
- **Right segment** (width **372px**, border‑left, right‑aligned): theme toggle
  (segmented sun ☀ / moon ☾ pill, active half filled), a 1px divider, the agent
  avatar (32px violet circle "JR"), and a **Log out** ghost button
  (radius 9, height 34, 1px border).
- Bar background: `#ffffff`(L) / `#211d1a`(D); bottom border `#e7e5e4`/`#44403c`.

**Body** — flex row, **height 680px**, three columns:

**a) Sidebar** (width **236px**, bg `#f5f5f4`(L)/`#211d1a`(D), border‑right):
- Section label "Workspace" (10.5px/700/uppercase/0.1em, stone‑400/500).
- Nav items (9×10 pad, radius 9, gap 10, 13.5px): **Inbox** active
  (bg `#efecf8`(L)/`#2c2542`(D), text `#5b46a0`/`#cbbef0`, 7px violet square
  marker, count badge `42` violet pill) + Assigned to me `7`, Mentions `2`.
- Section label "Manage": Contacts, Insights, Macros (inactive, grey square markers).
- Bottom: agent card — 30px violet avatar "JR" + name `Jenny Robles` /
  `Surseco · Agent`, in a bordered surface box.

**b) Main column** (flex, min‑width 0):
- **Page heading row** (18×22 pad): `Inbox` h2 (22px/800/‑0.02em) + muted
  "42 conversations · 8 awaiting reply", spacer, `Newest` sort chip (bordered).
- **Table header** (8×22 pad, top+bottom 1px border, bg `#f5f5f4`/`#211d1a`,
  10.5px/700/uppercase/0.08em muted): columns aligned to row cells —
  `[16px checkbox]`, **Conversation** (flex), **Status** (104px),
  **Priority** (78px), **Agent** (48px center), **Updated** (42px right).
- **Rows** (`.tg-row`, 12×22 pad, gap 12, bottom border `#f0efed`/`#2a2521`,
  `:hover { filter: brightness(.985) }`):
  - 16px checkbox (4px radius, 1px border; checked = filled violet).
  - 34px circle avatar with initials (tinted bg + matching text per contact).
  - Conversation cell (flex, min‑width 0): name (14px/700, `white-space:nowrap`)
    + a "Messenger" channel chip (10.5px/600, violet tint pill); below, a
    one‑line subject preview (13px muted, `text-overflow: ellipsis`).
  - Status pill (104px col), Priority chip (78px col) — see token tables below.
  - Agent: 26px initials circle (grey), or `—` if unassigned, or violet **AI**.
  - Updated: relative time (12px faint, right‑aligned).
  - **Selected row**: bg `#f6f3fc`(L)/`#241f33`(D) + 3px left border in accent
    (`#7f66bc`/`#9a86cf`); reduce left padding by 3px to compensate.

**c) Detail panel** (width **372px**, bg white/`#211d1a`, border‑left):
- **Header** (18×20 pad, bottom border): 40px violet‑tint avatar, customer name
  (16px/800), `Acct #SUR‑104882 · San Isidro` (12px muted), a Status pill.
  Below: two buttons — **Assign to me** (violet fill, flex 1) + **Resolve**
  (ghost, flex 1), radius 8.
- **Thread** (flex 1, 18×20 pad, gap 14, bg `#faf9f9`/`#1c1917`): centered
  day divider (11px faint); inbound bubbles left‑aligned, max‑width 84%,
  bg white/`#2c2825`, 1px border, radius `4px 14px 14px 14px`, 11×13 pad,
  13.5px/1.5; timestamp 10.5px faint under each.
- **AI suggested reply** card (pinned to bottom of thread, `margin-top:auto`):
  bg `#f6f3fc`(L)/`#241f33`(D), 1px border `#ddd3f4`/`#3d3470`, radius 12, 13 pad.
  Header row: ✦ sparkle + "Tugon AI · suggested reply"
  (11px/800/uppercase/0.06em, `#5b46a0`/`#b6a6e0`). Body 13px/1.55 with bold
  figures. Buttons: **Use reply** (violet fill) + **Regenerate** (ghost), radius 8.
- **Composer** (12×16 pad, top border): faux input (38px, radius 9, "Type a
  reply…") + 38px square violet send button (➜).

---

## Design tokens (component specifics)

### Status pills  (11.5px / 700, radius 99px, 3×9 pad, 6px leading dot)
| Status | Light text / bg / dot | Dark text / bg / dot |
|---|---|---|
| New | `#5b46a0` / `#efecf8` / `#7f66bc` | `#b6a6e0` / `#2c2542` / `#9a86cf` |
| Open | `#9a6a12` / `#f6edd7` / `#c2841d` | `#e0b252` / `#3a2f17` / `#d6a23f` |
| Pending | `#6b655f` / `#eceae8` / `#a8a29e` | `#bbb4ac` / `#2e2a26` / `#8a847e` |
| Resolved | `#3c7a57` / `#dcece3` / `#479c6b` | `#7bbd96` / `#1f3328` / `#479c6b` |

### Priority chips  (11.5px / 700, radius 99px, 3×9 pad, no dot)
| Priority | Light text / bg | Dark text / bg |
|---|---|---|
| High | `#a83b27` / `#f6e2dd` | `#e08a72` / `#3d251f` |
| Medium | `#9a6a12` / `#f6edd7` | `#e0b252` / `#3a2f17` |
| Low | `#6b655f` / `#eceae8` | `#bbb4ac` / `#2e2a26` |

### Channel chip / AI accent
- Messenger chip: text `#5b46a0`(L)/`#cbbef0`(D), bg `#efecf8`/`#2c2542`, radius 99px.
- AI elements use **violet** throughout (it is Tugon's accent *and* the system's
  "AI" signal). On dark, lift violet to `#9a86cf`/`#b6a6e0` for text.

### Geometry & elevation
| Token | Value |
|---|---|
| Sidebar width | 236px |
| Detail panel width | 372px |
| Top bar height | 62px · Body height | 680px |
| Logo/icon tile radius | 22% (squircle) |
| Cards / windows radius | 14–16px |
| Buttons / inputs radius | 8–10px |
| Pills / badges radius | 999px |
| Avatars | circle; 26 / 30 / 34 / 40px |
| Window shadow (light) | `0 22px 60px rgba(40,32,28,.16)` |
| Window shadow (dark) | `0 22px 60px rgba(0,0,0,.4)` |
| Card shadow (web) | `0 18px 48px rgba(40,32,28,.14)` |

### Type scale (observed)
| Use | Size / weight / tracking |
|---|---|
| Hero h1 | 46 / 800 / ‑0.03em |
| Page title (h2) | 22 / 800 / ‑0.02em |
| Wordmark | 17 / 800 / ‑0.02em |
| Row name / detail name | 14–16 / 700–800 |
| Nav / body | 13.5 / 600 |
| Pills & chips | 11.5 / 700 |
| Section labels | 10.5 / 700 / 0.1em / uppercase |
| Meta / timestamps | 11–12 / 400 |

---

## Interactions & Behavior
The prototypes are static; implement these behaviors:
- **Row select** → loads that conversation into the detail panel; apply the
  selected‑row treatment (accent left border + tint). Keyboard up/down to move.
- **Filter tabs** (All/Open/Pending/Resolved) filter the table; counts reflect
  current data.
- **Search** filters conversations live by customer/subject.
- **Theme toggle** switches light/dark (persist to localStorage / system pref).
- **Compose / Assign to me / Resolve / Use reply / Regenerate / Log out** →
  wire to the real handlers; "Use reply" drops the AI text into the composer.
- **Hover** on rows: `brightness(.985)` (or the codebase's row‑hover token).
- **Responsive**: below ~1100px, collapse the detail panel to a drawer/route and
  let the sidebar become an icon rail or off‑canvas menu.

## State Management
- `tickets[]` (id, customer, accountNo, subject, channel, status, priority,
  assignee, updatedAt, thread[]), `selectedTicketId`, `activeFilter`,
  `searchQuery`, `theme`, `currentAgent`.
- AI reply: async fetch of a suggestion for the selected ticket; `loading`,
  `suggestion`, `error`; "Regenerate" re‑requests.

## Generalizing the shell to other services
Keep top bar + sidebar + table + detail identical; swap per service:
- **Daloy** (teal): rows = integrations/routes; columns Source→Destination,
  Status, Events, Updated; detail = audit log timeline.
- **Hatid** (green): rows = licenses; columns Licensee, Tier, Posture, Expiry,
  Status; detail = key + signing info.
- **Bayad** (caramel, **white text**): rows = invoices; columns Account, Period,
  Amount, Due, Status; detail = bill breakdown + Pay action.
- **Susi** (green): rows = API keys / access; columns Key, Scope, Last used,
  Status; detail = key management.
Swap `data-brand` + the logo SVG; pills/AI stay structurally identical.

## Assets
- `tabang-brand/logos/*.svg`, `tabang-brand/glyphs/*.svg` — brand marks (vector,
  no font dependency). Glyphs traced from **Noto Sans Tagalog** (SIL OFL).
- **Hanken Grotesk** — Google Fonts (SIL OFL).
- Avatars in the mocks are initials on tinted circles — replace with real
  customer avatars where available.
- No other raster assets; everything else is CSS.

## Files
| File | What it is |
|---|---|
| `Tabang Brand System.dc.html` | Full brand system reference (logos, color, type, in‑use, dark mode) |
| `Tabang Web Example.dc.html` | Marketing landing, light + dark |
| `Tugon Console.dc.html` | **The reusable admin shell**, light + dark |
| `tabang-brand/tokens.css` | Color tokens + `data-brand`/`data-theme` theming |
| `tabang-brand/README.md` | Brand asset usage notes + the black‑text rule |
| `tabang-brand/logos/` · `glyphs/` | Brand SVGs (outlined, font‑free) |
| `support.js` | DC runtime — **ignore when porting**; only needed to preview the `.dc.html` files |

*Recreate these in the target app's React + Tailwind + shadcn environment using
its existing primitives; do not ship the HTML directly.*
