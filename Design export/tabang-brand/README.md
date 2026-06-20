# Tabang Brand Assets

Tabang (Tagalog: *to help, to aid*) is the parent brand. Tugon, Daloy, Hatid and
Bayad are products under it. The identity is built on **Baybayin**, the
pre-colonial script of the Philippines: each mark is its name's first syllable in
a shared warm squircle tile. One script, one tile — colour and character do the
talking.

| Product | Syllable | Glyph | Meaning | Accent |
|---------|----------|-------|---------|--------|
| **tabang** (parent) | Ta | ᜆ | to help, to aid | Mustard Gold `#dcb13b` |
| tugon  | Tu | ᜆᜓ | to respond  | Violet  `#7f66bc` |
| daloy  | Da | ᜇ | to flow     | Teal    `#2198a4` |
| hatid  | Ha | ᜑ | to deliver  | Green   `#479c6b` |
| bayad  | Ba | ᜊ | payment     | Caramel `#986539` |

Tugon's *Tu* is the parent *Ta* plus the **U-vowel kudlit** (ᜓ) — the parent grown
into a service.

---

## ⚠️ The one text rule — read this

> **Tabang is the only brand whose button / CTA text is BLACK.**
> Its mustard-gold accent is light, so text on it is ink (`--stone-900`).
>
> **Every other product (Tugon, Daloy, Hatid, Bayad) uses WHITE/cream text on
> its accent — in BOTH light and dark mode.**

Encoded in `tokens.css` as the `--on-*` variables; use `--on-accent` and you'll
always get it right. Glyphs *inside the logo tiles* are always white — this rule
is only about text/CTAs on accent-coloured surfaces.

---

## Contents

```
tabang-brand/
├── tokens.css                  Colour tokens + per-brand theming (--accent, --on-accent)
├── glyphs/                     Pure-outline Baybayin glyphs — no background, fill = currentColor
│   ├── ta-tabang.svg
│   ├── tu-tugon.svg
│   ├── da-daloy.svg
│   ├── ha-hatid.svg
│   └── ba-bayad.svg
└── logos/                      App-icon lockups — coloured tile + white glyph (512×512)
    ├── tabang.svg
    ├── tabang-reversed.svg     White tile, gold glyph (for dark / photo backgrounds)
    ├── tugon.svg
    ├── daloy.svg
    ├── hatid.svg
    └── bayad.svg
```

## Using the assets

**Every SVG is pure vector outlines — no font required, anywhere.** The glyphs
were traced from Noto Sans Tagalog into `<path>` data, so they render identically
in browsers, Figma, Illustrator, Sketch and on any OS with no font installed.

- **glyphs/** — bare glyph, no background, `fill: currentColor` (takes the CSS
  `color` of its context). Drop one in and colour it however you like.
- **logos/** — ready-made app icons (rounded tile + white glyph), 512×512 but
  fully scalable.

**Typeface** — the marks need none. The only font in the system is **Hanken
Grotesk** for wordmarks and UI; `tokens.css` imports it from Google Fonts
(self-host if you prefer).

**Tokens** — link `tokens.css`, set `data-brand="…"` (and optional
`data-theme="dark"`) on a container, then build with `var(--accent)` /
`var(--on-accent)`:

```html
<link rel="stylesheet" href="tokens.css">

<section data-brand="tugon">
  <button class="btn-primary">Generate reply</button>   <!-- white text -->
</section>

<section data-brand="tabang">
  <button class="btn-primary">Get started</button>      <!-- BLACK text -->
</section>
```

## Foundation (warm stone)

`50` canvas · `100` surface · `200` sunken · `300` hairline · `400–500` muted ·
`600` body · `700` heading · `800` ink · `900` night. In dark mode the scale
inverts; accents lift slightly for contrast and the text rule above is unchanged.

---

*Glyphs traced from Noto Sans Tagalog (SIL OFL). Hanken Grotesk is also SIL OFL.*
