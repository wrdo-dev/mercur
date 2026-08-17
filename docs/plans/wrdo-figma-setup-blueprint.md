# WRDO — Figma File Setup Blueprint

> A proper, production-ready WRDO design file — structured so it converts cleanly to
> storefront React code and stays maintainable. Built from Alwyn's real spec (read via
> Figma Dev-Mode MCP, file 9MCHXMg15fz67ID8QiE98K, node 2005-1268).
>
> **Why a clean file:** the current "10 Product Cards (Community)" file is a community
> template sketched over. This blueprint is the *real* WRDO design system.

---

## 0. File + naming conventions (read first — this is what makes it convert cleanly)

- **File name:** `WRDO — Product Design`
- **Component names: PascalCase**, no spaces. `ProductCard`, `NavTile`, `RideTracker`.
  (Figma component name → code component name is 1:1 when named this way. Messy Figma
  names = messy code.)
- **Variants: slash-organized** with named properties. e.g. `ProductCard` with a
  `kind` property = `product | relic | sundowner`, and a `compact` boolean.
- **Use Figma Variables/Styles for ALL tokens** (colors, type, shadow, radius) — never
  hardcode a hex on a layer. This is what lets the whole file re-theme + map to code
  tokens. Name them to match the code tokens (below).
- **Auto-layout everything** — frames use auto-layout (not absolute positioning) so they
  convert to flex/grid cleanly and stay responsive. (Your current file uses absolute
  positioning — that's fine for a sketch, but the clean file should be auto-layout.)
- **Component properties** for swappable bits: `label` (text), `icon` (instance swap),
  `image` (image fill), so one component handles many instances.

---

## 1. Pages (the tabs across the top of the Figma file)

1. **🎨 Foundations** — tokens: color styles, type scale, spacing, shadow, radius.
2. **🧩 Components** — every reusable component, organized in sections.
3. **📱 Mobile** — mobile screens (390px wide frames).
4. **💻 Web** — web/desktop screens (1440px wide frames, responsive intent).
5. **🗃 Assets** — hand-drawn icons, spiral logo, tape labels, character art.

---

## 2. 🎨 Foundations — the tokens (your EXACT real values)

### Color styles (name them exactly like the code tokens)
| Style name | Hex | Role |
|---|---|---|
| `void` | `#1C110D` | near-black, dark surfaces, text |
| `lime` | `#CCFF00` | THE accent + active + CTA |
| `lime/alt` | `#A9D300` / `#A9D200` | the slightly-muted lime used on titles/prices (you use this for card titles) |
| `ember` | `#E46227` (and `rgba(228,98,39,0.95)`) | warm secondary / ember-deal accent |
| `dust` | `#F5F2ED` | warm page background |
| `mist` | `#678590` | cool calm / secondary |
| `tile/grey-1` | `#EFEFEF` | inactive nav tile (Shop) |
| `tile/grey-2` | `#ECECEC` | inactive nav tile (Book) |
| `tile/grey-3` | `#E8E8E8` | inactive nav tile (Stash) |
| `text/body-60` | `rgba(38,38,38,0.6)` | "powdery" 60%-opacity body text |
| `text/heading-80` | `rgba(48,48,48,0.8)` | section headings ("Deals") |

> The multi-grey inactive tiles are a real, premium detail — keep all three distinct.

### Type scale (Inter — confirmed, all weights)
| Style | Size | Weight | Use |
|---|---|---|---|
| `Heading/Section` | 45px | Medium | "Deals" |
| `Heading/Card` | 30px | Semibold | "Your ride is on the way!" |
| `Title/Nav` | 25px | Semibold | nav tile labels |
| `Title/Card` | 24px | Semibold | product card titles (in accent color) |
| `Price` | 22px | Bold | card price (accent color) |
| `Body` | 14px | Regular | descriptions (text/body-60) |
| `Driver/Name` | 26px | Semibold | tracker driver name |

### Shadow style
- `shadow/lift` = `0px 60px 100px 0px rgba(72,72,72,0.16)` — the big soft premium lift.
  Used on every card/tile. This shadow is doing a LOT of the premium feel — keep it.

### Radius (your real values)
- `radius/card` = **42px** (cards, nav tiles — the big soft squircle)
- `radius/image` = **32px** (inner image panel)
- `radius/sm` = ~12px (chips, small elements)

---

## 3. 🧩 Components (build these, named exactly)

### `Logo`
- Spiral mark + "WRDO" + lime-tape handwritten "as you are". ~223px. Variants: `full`,
  `compact` (spiral + WRDO only).

### `NavTile`
- 211×193px, `radius/card` (42px), `shadow/lift`.
- Properties: `label` (text), `icon` (instance swap), `state` = `active | default`.
- `active`: `bg = lime`, black text + label, the hand-drawn icon.
- `default`: `bg = tile/grey-1|2|3` (pass grey as a variant or property), `void` text.
- Label centered at bottom, `Title/Nav` (25px semibold).

### `ProductCard` (LOCKED 2026-06-18 — restraint rules from the A/B/C test)
- 358×540px, `radius/card` (42px), white bg, `shadow/lift`.
- Properties: `kind` = `product | relic | sundowner | rental | local`, `variant` =
  `preview | opener`.
- Inner image panel: `radius/image` (32px), inset ~10px.

**The restraint system — two layers, two jobs (the load-bearing rule):**
| Layer | What | Treatment | Frequency |
|---|---|---|---|
| **Brand gesture** | New/Sale/Pre-order tape · WRDO❤ · kind-badges | hand-drawn sticker/tape — loud | **ONE per card max** |
| **Functional metadata** | Local · Free Delivery · Collect · Verified | **flat pill** (IconPark icon + label + soft brand-tint) — quiet | repeats every card |
> Test for which is which: *"does it appear on many cards?"* yes → must be a flat pill
> (repetition + illustration = noise). Rare/special → can be a sticker. **Local & Free
> Delivery are PILLS, never stickers** — the illustrated Local/Free-Delivery badges are
> for hero/marketing moments where they appear ONCE, not the product grid.

**Card anatomy (fixed positions — consistency across the grid is the whole point):**
- `TapeLabel` (status only: New/Sale/Pre-order/Sold-out) — **top-left**, rotated ~-8°, MAX ONE.
- **Vendor mark** — **top-right**, the vendor's REAL logo (not text/initials), EVERY card.
  Trust-critical for multi-vendor. Vendor uploads logo at onboarding → field on the
  Mercur vendor record → renders here.
- Title: `Title/Card` (24px semibold) in **accent color** (lime/alt for product+relic+rental,
  ember for ember-deals/pre-order — title picks up the deal's color).
- Price: `Price` (22px bold), same accent.
- Description: `Body` (14px, text/body-60).
- **Flat pill row** (bottom of body): Local · Free Delivery · Collect etc. — flat + soft
  brand-tint + IconPark stroke icon. Calm but crafted (NOT default-Bootstrap-tag plain).

**Buy button — NO cart on discovery cards (decided 2026-06-18):**
- A cart button is a promise: "tap and THIS exact item starts moving to checkout." On a
  homepage discovery feed that promise is premature (no variant chosen yet; fights the
  "human taps to confirm" spine rule; 20 buttons = clutter).
- `variant=preview` → **NO button. Whole card taps through to the product page** (where
  variant is chosen + Add-to-Cart lives).
- `variant=opener` (first card in each scroll row) → a **"See all New →" gateway button**
  (arrow/label, NOT a cart icon) → taps to the full section listing. The notch + floating
  button slot is reused, but the icon/label is a gateway, not a cart. Affordance matches
  behaviour — this is WHY the two card styles differ, and users grasp it instantly.
- Cart icon lives ONLY on listing pages (quick-add in buy-mode) + product detail (the real
  Add to Cart). Never on the homepage grid.

### `RideTracker` (the WRDO × Paarl Taxis co-brand card)
- `bg = void`, `radius/card` (42px), `shadow/lift`.
- Driver avatar + name (`Driver/Name`) + verified-star icon.
- Paarl Taxis logo. "Your ride is on the way! Arriving in **8 minutes**".
- **Lime progress fill** with 3 step-nodes (icons on lime).
- Lime side-panel with call / cancel / chat buttons.

### `AskBar`
- The "What do you need?" glass element, `radius/card`-ish, frosted, lime submit chip.

### `WrdoMessage`
- The voice bubble: spiral ring + "WRDO" label (lime/alt) + message text.

### `Button`
- Squircle (use `radius/card` scaled down, ~20-24px for buttons). Variants: `primary`
  (lime bg, void text), `void` (void bg, dust text), `ghost` (outline).

### `Chip`, `TapeLabel`, `Avatar`
- `TapeLabel`: the torn-tape look, rotated slightly when placed. **The real assets are
  pre-rendered PNGs** (see §6) — hand-lettered text is outlined, so the 6 status tapes
  (`label_new.png` etc.) are finished image fills, NOT a live text+colour swap. For
  *new* labels later, composite text over the blank `label_tape.png` base.

---

## 4. 📱 Mobile + 💻 Web screens

### Frame sizes
- **Mobile:** 390 × (variable) — iPhone-ish width. Single column.
- **Web:** 1440 × (variable). The same content, re-laid-out: nav tiles in a row up top or
  a left rail, cards in a multi-column grid, the tracker as a wider banner.

### Homepage = Discovery (build both mobile + web)
Compose from the components, in this order (your real layout):
1. `Logo` (top-left).
2. `WrdoMessage` greeting + `AskBar`.
3. **Nav tile row**: `NavTile` ×4 (Pay active/lime, Shop/Book/Stash default greys).
4. `RideTracker` (if an active ride) — the void+lime co-brand card.
5. **"Deals"** section heading (`Heading/Section`, 45px) → horizontal scroll of
   `ProductCard`s (relic/sundowner/product, accent titles, tape labels).
6. (Web) re-flow: nav as a row/rail, cards in a grid, tracker as a banner.

**Responsive intent:** design mobile + web from the SAME components so they stay in sync —
only the layout (column count, nav placement) changes, not the component design.

---

## 5. The handoff to code (how this becomes the storefront)

Once the clean file exists:
1. Alwyn selects a component or screen in Figma desktop (Dev Mode MCP server ON, port 3845).
2. Claude reads it via `get_design_context` → gets exact React+Tailwind + the real asset
   URLs (your hand-drawn icons download as PNG/SVG).
3. Claude converts to **storefront-native** components: maps Figma color/type/shadow/radius
   styles → the storefront's design tokens (mercur-storefront uses a 2-tier token system),
   downloads your real icons into the repo, builds proper React components with auto-layout
   → flex/grid. NO Tailwind hardcoded hexes — everything via the token system.
4. Result: your exact design, your real icons, as production React in the storefront.

**Conventions that make step 3 clean:** PascalCase component names, Figma Variables/Styles
for all tokens (named like the code tokens), auto-layout (not absolute positioning),
component properties for swappable content. Build the file this way and the conversion is
near-mechanical instead of a guessing game.

---

## 5b. 🔣 Icon layer — IconPark outline (LOCKED 2026-06-18)

The **functional icon layer** is **IconPark** (ByteDance, Apache-2.0 — fully
shippable). Imported into Figma file `KDf708Snj4DDfDoBTZYp6a` (WRDO-Frontend),
"icon pack" page — ~2,900 icons, 38 categories, 24×24 grid.

**The decision (Alwyn's, and it's correct):** *icons stay plain.* The brand
flavour comes from the stickers, tape labels, scribbles, and the spiral — NOT
from the icons. Clean icons are the **quiet** that lets the stickers shout; if
the icons were also quirky, everything reads as AI-busy and the special moments
stop landing. Same discipline as one-accent colour: restraint everywhere except
the deliberate brand gestures.

**Why IconPark fits (verified from SVG source):**
- `stroke-width="2"` uniform · `stroke-linecap/linejoin="round"` (warm-but-clean, not cold/cutesy)
- `fill="none"` outline style · `viewBox="0 0 24 24"`
- **`stroke="var(--stroke-0, …)"`** — built to inherit a colour token. Recolours to
  any brand token (void default → lime on active) with one variable. Maps straight
  to the storefront's 2-tier token system.

**Rules:** (1) **outline theme ONLY** — IconPark also ships filled/two-tone/duotone;
never import or mix them. (2) Icons render as single SVG components coloured via
token (`stroke: var(--brand-void)` → `var(--brand-lime)` active); sticker/tape PNGs
sit ON TOP as the flavour. Two layers, two jobs. (3) The rounded joins harmonise
with the soft squircle cards (`radius/card 42px`) + `shadow/lift` — a sharp icon
set would have fought them.

---

## 6. 🗃 Brand asset manifest (prepared 2026-06-18 — the real, cleaned files)

Alwyn's hand-drawn assets, exported from Kittl, cleaned + organised in
`~/dev/wrdo-assets/`. **30 production assets, 0 raster-stuffed SVGs.** Originals
preserved in `_raw/` (svg) and `_raw_png/` (png); cleaning config is
`~/dev/wrdo-assets/svgo.config.cjs`.

### The format rule (the load-bearing convention)
> **Flat / single-colour / geometric → SVG. Textured / illustrated / multi-colour → PNG.**

SVG only wins when the art is simple enough that vector paths are *smaller* than a
PNG. The moment there's a painted/brush texture, halftone, distressed-stamp edge,
or photo-like fill, **PNG wins on both size AND fidelity** — Kittl bakes those
textures as embedded rasters inside the SVG wrapper, so the "SVG" is just a heavy
PNG-in-a-box. The tell: a small graphic with a fat SVG size = baked texture →
make it PNG. This is why the hand-lettered tape labels + illustrated badges are
PNG while the spiral, wordmark, and nav-word labels stay SVG.

Cleaning: SVGs run through `svgo` (keeps viewBox, does **not** merge/collapse
paths — that would damage the hand-drawn gesture). PNGs run through `oxipng`
(lossless, `-o max --strip safe`), exported transparent.

### `logo/` — 8 SVG (all clean vector)
| File | Use |
|---|---|
| `logo_wrdo_full.svg` (95K) | full WRDO mark, spiral on Dust/cream — hero |
| `logo_wrdo_tape.svg` (80K) | spiral + WRDO™ on the lime textured tape (the slogan lockup). **Replaced** the old 578K raster `logo_tape_full.svg` (deleted). |
| `logo_wrdo_compact_{neon,dark}.svg` (6K) | spiral + WRDO only, two modes |
| `logo_wrdo_no_slogan_{neon,dark}.svg` (5K) | wordmark sans tape, two modes |
| `logo_spiral_{neon,dark}.svg` (3K) | the bare spiral mark, two modes |

### `labels/` — flat tape/text labels (SVG for nav words, PNG for textured tapes)
| File | Kind | Format |
|---|---|---|
| `label_new.png`, `label_sale.png`, `label_pre_order.png`, `label_sold_out.png`, `label_last_one.png`, `label_back_in_stock.png` (5–10K) | **status tapes** — hand-lettered on textured tape | PNG (were 30–95K as SVG; ~6× smaller as PNG) |
| `label_tape.png` (39K) | blank textured zebra-tape **base** (transparent) | PNG |
| `label_verified.png` (40K) | distressed "VERIFIED" rubber-stamp oval | PNG |
| `label_book/pay/shop/stash/work.svg` (5–11K) | **nav-word** labels — flat | SVG |
| `label_wrdo_love.svg` (18K) | lime thumbs-up sticker — flat enough | SVG |

> Status-label colour coding: New / Pre-Order / Back-in-Stock = lime; Sale = dust+ember;
> Last One = ember; Sold Out = void+lime. All share ~the same height (line up on cards).
> Text is outlined (hand-lettering can't be a live font) — so these are *finished*
> labels, not recolourable; the blank `label_tape.png` is the base for *new* ones.

### `badges/` — 7 PNG (illustrated kind/fulfilment badges)
| File | What |
|---|---|
| `label_sundowner.png` (45K) | circular sunset-scene badge (the Sundowner kind) |
| `label_relic.png` (67K) | the Relic (second-hand) kind badge |
| `label_rental.png` (46K) | lime smiley-sun starburst on orange tile (Rental kind) |
| `label_local.png` (71K) | Local kind badge |
| `label_free_delivery.png` (66K), `label_collect.png` (32K), `label_drop.png` (28K) | fulfilment chips |

### `icons/` — 1 SVG
| File | Use |
|---|---|
| `icon_apple_touch.svg` (64K) | spiral-on-lime app icon. Rasterise to a 180×180 `apple-touch-icon.png` at storefront build time (apple-touch is meant to be a PNG). |

### Folder map → component usage
- `ProductCard.TapeLabel` instance ← the `label_*.png` status tapes (image fill, not inline SVG).
- `ProductCard` `kind` variants ← the `badges/*.png`.
- `Logo` component ← `logo_wrdo_*` (full / tape / compact / no-slogan variants).
- `NavTile` / brand chrome ← nav-word `label_*.svg` + spiral.

**Note for the code handoff:** `label_tape` and `label_verified` are PNG (not SVG)
— in Figma/code they're **image fills**, not inline vectors. The 6 status tapes
and all badges are likewise image fills. Everything in `logo/`, the nav-word
labels, and `icon_apple_touch` are true inline SVG.
