# Roban AI — Web Design System

This document defines the visual language of [roban-ai.github.io](https://roban-ai.github.io/).
Every change to the site should follow — and update — these rules.

The site is dark, quiet, and restrained: a pre-launch home page that feels
composed rather than empty. Red is a signal color, used sparingly.

## 1. Brand foundation

### Color

| Token | Hex / value | Usage |
| --- | --- | --- |
| `--red` | `#E51717` | Official brand red. Primary button fill, the mark. |
| `--red-hover` | `#F52A2A` | Hover state of primary red. |
| `--red-press` | `#C21212` | Pressed state of primary red. |
| `--red-bright` | `#FF4D4D` | Red as *text/glow* on dark (legible against `#0D0D10`). |
| `--red-wash` | `rgba(229,23,23,.14)` | Hover fill in menus. |
| `--red-glow` | `rgba(229,23,23,.32)` | Hero orb gradient core only. |
| `--bg` | `#0D0D10` | Page background. |
| `--bg-raise` | `#131317` | Raised surfaces. |
| `--panel` | `rgba(23,23,28,.92)` | Floating panels (menu). |
| `--ink` | `#F5F5F6` | Primary text. |
| `--ink-dim` | `#C9C9CF` | Secondary text, buttons at rest. |
| `--muted` | `#94949C` | Captions, footer. |
| `--line` | `rgba(255,255,255,.08)` | Hairlines. |
| `--line-strong` | `rgba(255,255,255,.16)` | Control borders. |

Rules:

- Red is a **signal**, never a surface — the only red fills on the page are the
  primary button and the reversed logo lockup (which is artwork, not UI).
- Red text on dark uses `--red-bright`, not `--red`; `#E51717` text on
  `#0D0D10` fails contrast.
- Depth comes from the dark neutral scale, never from new hues.
- Total red area should stay well under ~10% of any viewport.

### Logo

Assets in `assets/`:

| File | Contents | Use |
| --- | --- | --- |
| `logo-mark.png` | Circular red mark, white glyph, **transparent** PNG. | Favicon (works on dark and light tab bars), header, footer. |
| `logo-lockup.png` | Horizontal lockup, transparent background. | Light placements, documents. |
| `logo-lockup-red.png` | Reversed lockup: white wordmark on a red card. | Dark backgrounds, hero showcase. |
| `apple-touch-icon.png` | Solid red square with white glyph. | iOS home-screen icon (iOS composites transparency to black). |

Usage rules:

- The mark's transparent corners are deliberate — never re-add a white box.
- Display lockups at their natural aspect ratio (`logo-lockup-red.png` ≈ 2.55:1).
  Always pair `width`-based sizing with `height: auto` (or `aspect-ratio`).
- Clear space around any lockup ≥ 25% of the mark's diameter.
- Minimum mark size: 20 px digital. Minimum lockup height: 20 px.
- Do not recolor, rotate, add effects, or stretch the logo.

## 2. Typography

- Family: `Inter, "SF Pro Text", -apple-system, "Segoe UI", Roboto, Arial, sans-serif`.
  No external font dependency — system rendering keeps load instant.
- Scale (fluid, desktop maxima):

  | Role | Size | Weight | Tracking |
  | --- | --- | --- | --- |
  | H1 | 42–74 px | 700 | -0.035em |
  | H2 | 27 px | 700 | -0.02em |
  | Body / row | 16.5 px | 500 | 0 |
  | Button / link | 13.5–14.5 px | 620–640 | 0.005em |
  | Status pill / micro-label | 11.5 px | 650–700 | 0.14em, uppercase |

- Headings use `text-wrap: balance`; one idea per heading, no taglines.
- Micro-labels are muted and uppercase; the small red dot is the only accent.

## 3. Space and shape

- Page shell: `min(1120px, 100% − 48px)`, centered.
- Compact rhythm: header 64 px (sticky, translucent, blurred), hero padding
  64/48 px, section padding 44/64 px, footer ~58 px.
- Radii: cards/panels 12–20 px; buttons, triggers and status pills are **pills**
  (`999 px`), echoing the circular mark.
- 1 px hairlines for structure; shadows only on floating layers (menu, hero card).

## 4. Atmosphere (the "pre-launch" layer)

Decorative background, all optional and cheap:

| Layer | Implementation | Notes |
| --- | --- | --- |
| Starfield | `<canvas id="starfield">`, fixed, z 0 | ~170 twinkling stars, slow upward drift, ~10% faint red; one meteor every 4.5–10.5 s. Static single frame under reduced motion. |
| Ambient glows | `.ambient` divs, fixed | One faint red radial (top right), one neutral (bottom left); 18–24 s drift. |
| Film grain | `body::after` SVG turbulence | Fixed, 4.5% opacity, `pointer-events: none`. |
| Orb + ring | `.hero-visual` | Breathing red radial glow + 1 px ring, 8 s cycle. |
| Float | `.hero-card` | ±6 px vertical float, 7 s. |
| Parallax | hero `--px/--py` vars | Desktop pointers only; orb and card move in opposite directions (±18 px / ∓10 px). |
| Reveals | `.reveal` / `[data-reveal]` | Staggered rise on load; IntersectionObserver for below-fold content. |

Rules:

- Every animation collapses under `prefers-reduced-motion: reduce`.
- Effects never carry information; removing them must leave the page fully usable.
- Keep it calm: long durations (7–24 s), low opacities. No pulsing text.

## 5. Components

### Buttons

- **Primary** — `--red` fill, white text, pill, soft red glow shadow.
  Hover lifts 1 px and brightens; active presses down 1 px.
- **Ghost** — transparent, `--line-strong` border, `--ink-dim` text.
  Hover brightens border and text, adds a 4% white fill.
- Sizes: default 46 px tall; `btn-sm` 38 px (header).
- External links open in a new tab and end with a `↗` glyph.

### Status pill / micro-label

- Pill: 1 px border, 3% white fill, uppercase tracked text, 7 px red dot with
  `pulse` + expanding `ping` halo — the "awaiting launch" signal.
- Micro-label: same typography, no border; 5 px static red dot.

### Dropdown menu

- Pill trigger with rotating caret; opens on click, closes on outside click
  and `Escape`; `aria-expanded` / `hidden` managed; focus returns to the
  trigger on `Escape`.
- Panel: top-right anchored, 216 px min, blurred `--panel` background,
  menu shadow; hidden state uses `visibility` so items are not tabbable.
- Items hover with `--red-wash`.

### Rows

- Work rows: `name / link` grid inside hairline top and bottom borders.
  Link color goes `--red-bright` on hover.

## 6. Bilingual content (EN / 中文)

Both languages live in the same document; switching never navigates.

- Every translatable string is a pair of spans: `<span data-lang="en">…</span><span data-lang="zh">…</span>`.
  One is hidden via `html.lang-en [data-lang="zh"]{display:none!important}` and the inverse rule.
- The switcher is a 64×30 px pill in the header (`.lang-toggle`) with "EN / 中" labels
  and a light knob that slides 30 px (`transform`, 280 ms) — active label sits on the knob in dark text.
- State lives on `<html>`: class `lang-en` / `lang-zh` plus the `lang` attribute (`en` / `zh-CN`)
  for screen readers. The choice persists in `localStorage` (`roban-lang`); default is English.
- Markup ships with `class="lang-en"`, so the page is English even before JS runs.
- Glyphs shared by both languages (GitHub, arrows, the wordmark) stay outside the
  `data-lang` spans so nothing shifts between languages; the font stack keeps Latin
  fonts ahead of the CJK fallbacks (`PingFang SC`, `Microsoft YaHei`, `Noto Sans SC`).
- Uppercase micro-labels keep their letter-spacing in Chinese — the airy tracking is intentional.

## 7. Motion

- Easing: `cubic-bezier(0.32, 0.72, 0, 1)` for transforms; plain ease for colors.
- UI durations: 180 ms (color/border), 320 ms (transform/shadow),
  140 ms close delay on menus, 280 ms for the language knob.
- Ambient durations: 7–24 s (see §4).
- `prefers-reduced-motion` disables everything in §4 and shortens UI transitions.

## 8. Accessibility

- Text contrast: `--ink` on `--bg` ≈ 17:1; `--ink-dim` ≈ 10:1; `--muted` ≈ 5:1;
  white on `--red` ≥ 4.5:1. Never set `--red` text on dark — use `--red-bright`.
- Focus: 2 px `--red-bright` outline, 3 px offset, on `:focus-visible` only.
- Decorative layers (`ambient`, `#starfield`, `.hero-visual`) are `aria-hidden`.
- Touch targets ≥ 38 px.

## 9. File map

```
index.html     markup + starfield, parallax, dropdown, reveal, language scripts
styles.css     all styling (tokens in :root)
assets/        logo-mark (transparent) / logo-lockup / logo-lockup-red / apple-touch-icon
DESIGN.md      this document
```

## 10. Do / Don't

**Do** — keep one idea per screen; let the dark field do the talking; spend red
on one signal per view (button, dot, or word); reuse the pill; keep copy minimal.

**Don't** — flood the page with red or bright colors; add new hues or hard
gradients; use red text at `#E51717` on dark; stretch or recolor the logo;
re-add a white box behind the mark; add taglines or filler sections.
