# ODC Design System 3.0

How the Figma design system is set up in this repo. The tokens live in [`src/styles/tokens.css`](../src/styles/tokens.css). Use this doc to understand the naming and the reasoning behind it.

- **Figma source:** [ODC Design System 3.0 › tokens](https://www.figma.com/design/l5w8YlFfqkzklYEY75XZCW/ODC-Design-System-3.0?node-id=2394-10526) (page `2394:10526`)
- **Translated:** 2026-10-15
- **Scope:** tokens only (color, type, space, breakpoints). There are no components on this page yet.

> Tokens (aka semantics) use primitive values to assign meaning to an element.

---

## Naming syntax

Primitives, tokens and styles follow one predictable syntax so they compose.

| Convention | Meaning |
|---|---|
| `.` prefix | Marks a collection as private, e.g. primitives like `.gray9` or `.black48` |
| `/` | Separates scope from token name (`fg/primary`) |
| `camelCase` | Multi-word names (`fg/invWeak`) |
| `-` | Ranges and variants (`space/sm-5xl`, `ui/base-400`) |

| Category | Pattern | Example | Notes |
|---|---|---|---|
| Primitive color | `colorLevel` / `colorOpacityValue` | `neutral/500`, `black48` | Levels run light to dark. Opacity uses 2–3 digits. |
| Color token | `scope/prefixHierarchyOrStatus` | `fg/invWeak` | Scope is `fg` or `bg`. Prefix `inv` = reversed context. |
| Space | `space/spaceMin-spaceMax` | `space/sm-5xl` | Shirt sizes 4xs–5xl. A range interpolates min → max across viewports. |
| Size | `size/length-variant` | `size/half-min` | Lengths: column, fraction, `vw`, `vh`, `mw` (min width), `fw` (full width). Variants: `-min`, `-max`, none. |
| Font | `font/step` | `font/base` | Step uses Tailwind names on the fluid scale. |
| Text style | `role/step-weight` | `ui/base-400` | Role is `heading`, `paragraph` or `ui`. Weight is the numeric font-weight. |

**In CSS:** `/` becomes `-` and camelCase stays the same. So `fg/invWeak` → `--fg-invWeak`, `space/sm-5xl` → `--space-sm-5xl`, and the text style `ui/base-400` → class `.ui-base-400`.

---

## Color

Every hue has 11 steps (50–950) at equal intervals from lightest to darkest. Step 500 is the reference step (marked `*` in Figma). Built with [inclusivecolors.com](https://www.inclusivecolors.com).

**Hues:** neutral, red, yellow, green, blue, purple. Also white and black.

**Opacity primitives** (`black61` … `black04`, `white80` … `white10`) are set so each step meets an [APCA](https://github.com/Myndex/SAPC-APCA/discussions/39#discussioncomment-1897367) level: Lc 90 / 75 / 60 / 45 / 30 / 15 / 0.

### Semantic tokens

| Hierarchy | `fg/` | `fg/inv…` | `bg/` | `bg/inv…` |
|---|---|---|---|---|
| strong | black | white | — | — |
| primary | neutral/800 | neutral/100 | white | neutral/800 |
| secondary | black61 | white70 | neutral/100 | black61 |
| tertiary | black48 | white59 | — | black48 |
| muted | black24 | white47 | — | black24 |
| weak | black13 | white33 | black04 | black13 |

| Status | `fg/` | `bg/` |
|---|---|---|
| valid | green/700 | green/100 |
| caution | yellow/700 | yellow/100 |
| urgent | red/700 | red/100 |
| info | blue/700 | blue/100 |
| beta | purple/700 | purple/100 |

- **`fg`** is for text, icons, borders, illustration and data.
- **`bg`** is for buttons, chips, cards, tiles, containers, banners and surfaces.
- **Hierarchy** maps to APCA contrast levels. Use the semantic tokens. Don't use primitives directly in components.

---

## Type

- **Family:** Geist Sans, Geist Mono ([vercel.com/font](https://vercel.com/font))
- **Weights:** 100 Thin · 200 ExtraLight · 300 Light · 400 Regular · 500 Medium · 600 SemiBold · 700 Bold · 800 ExtraBold · 900 Black. Geist is variable, so in-between weights like 275, 350, 375, 425 and 450 are used on purpose.
- **Default features:** `case`, `cpsp`, `zero`, `ss05`, `ss09` (plus `dlig` where you want it). These are set in `--font-features`.

### Fluid scale

Type scales fluidly across viewports from a 16px (1rem) base. The system combines Utopia's `clamp()` with Tailwind's naming, so design and development use the same names. The type scale also drives space and layout.

| | min (320px) | max (2560px) |
|---|---|---|
| Base size | 16px | 20px |
| Ratio | 1.125 (major second) | 1.25 (major third) |

Steps: `xs` (−2), `sm` (−1), `base` (0), `lg`, `xl`, `2xl` … `9xl` (+10). Each one is a `--text-{step}` custom property.

[Utopia type calculator](https://utopia.fyi/type/calculator?c=320,16,1.125,2560,20,1.25,10,2,702-1024-1440-1920&s=0.75%7C0.5%7C0.25%7C0.125,1.5%7C2%7C3%7C4%7C6%7C8%7C12,s-l%7Cs-xl%7Cs-2xl%7Cs-3xl%7Cs-4xl%7Cs-5xl%7Cl-2xl%7Cl-3xl%7Cxl-3xl%7Cxl-4xl%7C2xl-4xl%7C2xl-5xl%7C3xl-5xl%7C2xs-5xl%7C2xs-l&g=2xs,5xl,5xl,12) · for finer control, see [fluidtypography.com](https://fluidtypography.com/)

### Text styles

| Role | Use for |
|---|---|
| `heading` | Display, title, subtitle, eyebrow, primary text |
| `paragraph` | Body, columns, long-form |
| `ui` | Labels, tooltips, links |

| Style | Weight | Line height | Tracking | Notes |
|---|---|---|---|---|
| heading/9xl-275 | 275 | 1 | −5% | |
| heading/8xl-300 – 6xl-300 | 300 | 1 | −4% | Hero / display |
| heading/5xl-300 | 300 | 1.05 | −3% | Primary headline |
| heading/4xl-300 | 300 | 1.1 | −3% | |
| heading/3xl-300 | 300 | 1.1 | −2% | |
| heading/2xl-350 | 350 | 1.1 | −2% | |
| heading/xl-375 | 375 | 1.1 | −1% | |
| heading/lg-400 | 400 | 1.15 | −0.5% | Lede |
| heading/base-400 | 400 | 1.2 | 0 | Small lede |
| heading/sm-500 | 500 | 1.1 | 7%, uppercase | Primary eyebrow |
| heading/xs-500 | 500 | 1.1 | 8%, uppercase | Secondary eyebrow |
| paragraph/xl-375 | 375 | 1.2 | −1% | |
| paragraph/lg-400 | 400 | 1.3 | −0.5% | |
| paragraph/base-400 | 400 | 1.4 | 0 | Default body |
| paragraph/sm-425 | 425 | 1.4 | 0.5% | |
| paragraph/xs-450 | 450 | 1.4 | 1% | |
| ui/lg-400, lg-500 | 400 / 500 | 1.1 | −0.5% | |
| ui/base-400, base-500 | 400 / 500 | 1.1 | 0 | |
| ui/sm-400, sm-500 | 400 / 500 | 1.1 | 0.5% | |
| ui/xs-400, xs-500 | 400 / 500 | 1.1 | 1% | |

Headings use `hanging-punctuation: first last allow-end`. Paragraphs use `margin-block: 1lh`, with list items spaced by `space/2xs`.

---

## Space

Space is built from the base text step (16px → 20px), so it scales fluidly like type. It's used for gap, padding, radius, width, height, stroke and opacity.

| Step | × base | min | max |
|---|---|---|---|
| 4xs | 0.125 | 2px | 2.5px |
| 3xs | 0.25 | 4px | 5px |
| 2xs | 0.5 | 8px | 10px |
| xs | 0.75 | 12px | 15px |
| sm | 1 | 16px | 20px |
| md | 1.5 | 24px | 30px |
| lg | 2 | 32px | 40px |
| xl | 3 | 48px | 60px |
| 2xl | 4 | 64px | 80px |
| 3xl | 6 | 96px | 120px |
| 4xl | 8 | 128px | 160px |
| 5xl | 12 | 192px | 240px |

**Ranges** run from one step's min to another step's max, for more dramatic scaling: `sm-lg`, `sm-xl`, `sm-2xl`, `sm-3xl`, `sm-4xl`, `sm-5xl`, `lg-2xl`, `lg-3xl`, `xl-3xl`, `xl-4xl`, `2xl-4xl`, `2xl-5xl`, `3xl-5xl`, `2xs-5xl`, `2xs-lg`.

The design uses Tailwind-style names (`sm`, `md`, `lg`). Utopia's own names are `s`, `m`, `l`.

[Utopia space calculator](https://utopia.fyi/space/calculator?c=320,16,1.125,2560,20,1.25,10,2,702-1024-1440-1920&s=0.75|0.5|0.25|0.125,1.5|2|3|4|6|8|12,s-l|s-xl|s-2xl|s-3xl|s-4xl|s-5xl|l-2xl|l-3xl|xl-3xl|xl-4xl|2xl-4xl|2xl-5xl|3xl-5xl|2xs-5xl|2xs-l&g=2xs,5xl,5xl,12)

---

## Breakpoints

Breakpoints use Tailwind naming, with Utopia min and max caps added for fluid scaling.

| Name | rem | px | Devices | What it decides |
|---|---|---|---|---|
| min | 20rem | 320 | Smallest phone; 400% zoom | WCAG reflow; tightest constraints and touch targets |
| sm | 44rem | 704 | Phone landscape / foldable, tablet portrait | What goes to 2 columns |
| md | 64rem | 1024 | Tablet landscape, small laptop | Smallest full nav, tight multi-columns. Covers ~90% of design problems at 1280px. |
| lg | 90rem | 1440 | Primary desktop | Presentations, Figma default. Covers ~90% of design problems at 1536px. |
| xl | 120rem | 1920 | Large monitor, slide | Most max widths, gutter growth |
| max | 160rem | 2560 | Ultra-wide / 4K, TV | Grid and margin behavior, more columns |

The Figma file includes example layouts (nav, hero, grid, quote) at each breakpoint: 320, 702, 1024, 1440, 1920 and 2560.

---

## Gaps and open questions

These came up during translation and need a designer's call:

1. **Not defined yet:** the tokens index lists Borders, Grids, Motion, Opacity and Shadows, but this page has no values for them. The `size/` syntax is documented, but there are no `size/*` variables to translate yet. None of these were invented in code.
2. **`sm` breakpoint:** the styles frame and the Utopia config say **702px**, but the breakpoint frame says **44rem / 704px**. Code uses 44rem.
3. **`heading/xs-500` tracking:** the published text style is **8%**, but the styles spec frame says **5%**. Code follows the published style (8%).
4. **`paragraph/xl` weight:** the published style is **xl-375**, but the styles spec frame labels it **xl-400**. Code follows the published style (375).
5. **Dark mode:** `inv*` tokens cover reversed sections, but there's no full dark theme mode.
