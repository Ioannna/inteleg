# înțeleg — Design specification

Developers and coding agents use this document when they build the înțeleg UI. It lists the names, roles and rules behind the design. **It does not repeat the design**: every value, measurement and visual detail lives in Figma, and every section links to the node it describes.

Reflects the Figma file as of 2026-11-26.

## Contents

1. [Introduction](#1-introduction)
2. [Naming convention](#2-naming-convention)
3. [Foundations](#3-foundations)
4. [Components](#4-components)
5. [Pages](#5-pages)
6. [General rules](#6-general-rules)
7. [Open decisions](#7-open-decisions)
8. [Decision log](#8-decision-log)

---

## 1. Introduction

### Purpose

înțeleg is a mobile-first web app that helps 8th-grade students in Romania prepare for the Evaluarea Națională math exam. This document turns the Figma design into a specification for code:

- what each design token is called in CSS
- what each token and component is for
- the rules every screen must follow.

### Source of truth

**Figma is the single source of truth.**

- **Values** (colours, sizes, font metrics, shadows) live only in Figma. In code they live only in the global tokens stylesheet (`tokens.css`), which will be generated from Figma (not created yet). This document never states them.
- **If this document and Figma disagree, Figma wins.** Update this document to match it.
- **To change the design, change Figma first**, then regenerate the tokens and update this document.

Figma file: [inteleg - design](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design)

| Figma page | Node | In scope |
|---|---|---|
| Foundations | [3:2](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=3-2) | Yes: variables, text styles, effect styles |
| Components | [16:144](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=16-144) | Yes: Logo, Icon, Button, Badge, Header, Hero |
| Home | [3:3](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=3-3) | Yes: the two Home frames. The "Maketa" rectangle is a working reference and is excluded. |
| Cover, _Utilities | — | No |

### Mobile-first

The design is mobile-first:

- **Reference width:** 360. Build and review at this width first.
- **Verified width:** 414. Every layout must also hold here.

Wider layouts are not designed yet (see [7.2](#72-not-designed-yet)).

---

## 2. Naming convention

### 2.1 Variables and effect styles → CSS custom properties

**Rule:** add a type prefix, then copy the Figma name with each `/` replaced by `-`.

| Figma source | Prefix | Example |
|---|---|---|
| Collection **Colors** | `--color-` | `blue/600` → `--color-blue-600` |
| Collection **Spacing**, `space/*` | (none; the Figma group is the prefix) | `space/16` → `--space-16` |
| Collection **Spacing**, `radius/*` | (none) | `radius/lg` → `--radius-lg` |
| Effect styles `shadow/*` | (none) | `shadow/primary` → `--shadow-primary` |

Each collection has a single mode.

**Full mapping.** Every variable and effect style appears here exactly once.

| Figma name | CSS custom property |
|---|---|
| `blue/25` | `--color-blue-25` |
| `blue/50` | `--color-blue-50` |
| `blue/200` | `--color-blue-200` |
| `blue/600` | `--color-blue-600` |
| `blue/700` | `--color-blue-700` |
| `orange/50` | `--color-orange-50` |
| `orange/100` | `--color-orange-100` |
| `orange/400` | `--color-orange-400` |
| `orange/600` | `--color-orange-600` |
| `orange/700` | `--color-orange-700` |
| `orange/800` | `--color-orange-800` |
| `neutral/white` | `--color-neutral-white` |
| `neutral/50` | `--color-neutral-50` |
| `neutral/border` | `--color-neutral-border` |
| `neutral/ink-2` | `--color-neutral-ink-2` |
| `neutral/ink` | `--color-neutral-ink` |
| `feedback/success` | `--color-feedback-success` |
| `feedback/success-bg` | `--color-feedback-success-bg` |
| `feedback/error` | `--color-feedback-error` |
| `feedback/error-bg` | `--color-feedback-error-bg` |
| `space/4` | `--space-4` |
| `space/8` | `--space-8` |
| `space/12` | `--space-12` |
| `space/16` | `--space-16` |
| `space/20` | `--space-20` |
| `space/24` | `--space-24` |
| `space/32` | `--space-32` |
| `space/40` | `--space-40` |
| `space/48` | `--space-48` |
| `radius/sm` | `--radius-sm` |
| `radius/md` | `--radius-md` |
| `radius/lg` | `--radius-lg` |
| `radius/xl` | `--radius-xl` |
| `radius/full` | `--radius-full` |
| `shadow/sm` | `--shadow-sm` |
| `shadow/md` | `--shadow-md` |
| `shadow/primary` | `--shadow-primary` |

That is 20 colours, 9 spacing values, 5 radii and 3 shadows.

### 2.2 Text styles → custom properties + class

Each text style `{group}/{name}` becomes:

- four custom properties:
  - `--text-{group}-{name}-size`
  - `--text-{group}-{name}-line-height`
  - `--text-{group}-{name}-weight`
  - `--text-{group}-{name}-letter-spacing`
- one class, `.text-{group}-{name}`, which applies the family, the four properties and nothing else.

All text styles use one font family, Nunito, exposed as `--font-family-base`. Figma has no variable for the family; it comes from the text styles.

Figma gives letter-spacing as a percentage of font size. Convert it to `em` (percentage ÷ 100).

| Figma text style | Custom property prefix | Class |
|---|---|---|
| `brand/wordmark` | `--text-brand-wordmark-*` | `.text-brand-wordmark` |
| `heading/display` | `--text-heading-display-*` | `.text-heading-display` |
| `heading/h1` | `--text-heading-h1-*` | `.text-heading-h1` |
| `heading/h2` | `--text-heading-h2-*` | `.text-heading-h2` |
| `heading/h3` | `--text-heading-h3-*` | `.text-heading-h3` |
| `body/lg` | `--text-body-lg-*` | `.text-body-lg` |
| `body/md` | `--text-body-md-*` | `.text-body-md` |
| `ui/button` | `--text-ui-button-*` | `.text-ui-button` |
| `ui/label` | `--text-ui-label-*` | `.text-ui-label` |
| `ui/caption` | `--text-ui-caption-*` | `.text-ui-caption` |
| `ui/badge` | `--text-ui-badge-*` | `.text-ui-badge` |

### 2.3 Component-scoped custom properties

Some component dimensions have no Figma variable: control heights, icon size, Header height, logo mark size. Handle them like this:

- **Name:** `--{component}-{part}-{property}`, e.g. `--button-primary-height`, `--icon-size`, `--logo-mark-width`.
- **Where:** declare them in the component's own stylesheet, never in `tokens.css`.
- **Source:** take the value from the linked Figma node, and put the node link in a comment next to the declaration.
- **Derived values:** when Figma defines a value as a relationship, write the relationship, not the number. See the Header tagline inset in [4.5](#45-header).

### 2.4 Tokens only

- Components use tokens for colour, spacing, radius, typography and shadow. **Never hard-code these values.**
- Component dimensions without a token use [2.3](#23-component-scoped-custom-properties).
- If a needed value has no token, add it in Figma first (see [Source of truth](#source-of-truth)). Do not invent it in code.

---

## 3. Foundations

Source nodes: [Foundations page 3:2](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=3-2)

"Not used on any screen yet" means the token exists in Figma but no current component or page uses it. Its role is only the group role Figma gives it.

### 3.1 Colour

Source nodes: [Colors 16:147](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=16-147)

The four Figma groups have these roles: **blue** is Primary, **orange** is Accent, **neutral** is surfaces and text, **feedback** is answer states (correct and incorrect).

| Token | Role and current use |
|---|---|
| `--color-blue-25` | Primary group. Not used on any screen yet. |
| `--color-blue-50` | Hero background |
| `--color-blue-200` | Secondary button border; Deco/Circle blue |
| `--color-blue-600` | Brand blue text: Badge label, Header subtitle, Secondary button label. Also in Logo/Mark and Deco/Pi. |
| `--color-blue-700` | Primary group. Not used on any screen yet. |
| `--color-orange-50` | Accent group. Not used on any screen yet. |
| `--color-orange-100` | Deco/Circle peach |
| `--color-orange-400` | **Primary action.** Primary button background. Among interactive elements, orange is reserved for the Primary button. Also in Logo/Mark and Deco/Plus. |
| `--color-orange-600` | Used only as the colour of `--shadow-primary` (intentional) |
| `--color-orange-700` | Accent group. Not used on any screen yet. |
| `--color-orange-800` | Accent group. Not used on any screen yet. |
| `--color-neutral-white` | Surfaces: Header, Badge, Secondary button, Icon button |
| `--color-neutral-50` | Page background (Home) |
| `--color-neutral-border` | Header bottom border, Icon button border |
| `--color-neutral-ink` | Main text: headline, wordmark, Primary button label. Default icon colour. |
| `--color-neutral-ink-2` | Secondary text: Hero intro, privacy note |
| `--color-feedback-success` | Answer state: correct. Not used on any screen yet. |
| `--color-feedback-success-bg` | Answer state: correct. Not used on any screen yet. |
| `--color-feedback-error` | Answer state: incorrect. Not used on any screen yet. |
| `--color-feedback-error-bg` | Answer state: incorrect. Not used on any screen yet. |

### 3.2 Typography

Source nodes: [Typography 10:76](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=10-76)

Font family: `--font-family-base` (Nunito). Line heights follow the grid stated in the Typography frame.

**Text style names do not set HTML heading levels.** For example, the Hero headline uses `.text-heading-display`; its HTML level comes from the page's heading outline (see [7.2](#72-not-designed-yet) #8), and `heading/h1` does not mean `<h1>`.

| Class | Group | Current use |
|---|---|---|
| `.text-brand-wordmark` | Brand | Logo/Full wordmark |
| `.text-heading-display` | Interface | Hero headline |
| `.text-heading-h1` | Interface | Not used on any screen yet |
| `.text-heading-h2` | Interface | Not used on any screen yet |
| `.text-heading-h3` | Interface | Not used on any screen yet |
| `.text-body-lg` | Interface | Hero intro text |
| `.text-body-md` | Interface | Not used on any screen yet |
| `.text-ui-button` | Interface | Primary and Secondary button labels |
| `.text-ui-label` | Interface | Not used on any screen yet |
| `.text-ui-caption` | Interface | Hero privacy note |
| `.text-ui-badge` | Interface | Badge label; Header subtitle (intentional) |

### 3.3 Spacing

Source nodes: [Section/spacing 24:558](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=24-558)

| Token | Current use |
|---|---|
| `--space-4` | Header: gap between logo and subtitle. Badge: vertical padding. |
| `--space-8` | Logo/Full: gap between mark and wordmark. Button: gap between content items. Part of the Header tagline inset. |
| `--space-12` | Header: vertical padding. Badge: horizontal padding. Hero: gap between the two buttons. |
| `--space-16` | Header and Hero: horizontal padding. Hero: gap inside the Intro and Actions groups. |
| `--space-20` | Not used on any screen yet |
| `--space-24` | Button: horizontal padding (Primary, Secondary). Hero: gap between the Intro and Actions groups. |
| `--space-32` | Hero: vertical padding |
| `--space-40` | Not used on any screen yet |
| `--space-48` | Not used on any screen yet |

### 3.4 Radius

Source nodes: [Section/radius 24:559](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=24-559)

| Token | Current use |
|---|---|
| `--radius-sm` | Not used on any screen yet |
| `--radius-md` | Icon button |
| `--radius-lg` | Primary and Secondary buttons |
| `--radius-xl` | Not used on any screen yet |
| `--radius-full` | Badge (pill) |

### 3.5 Shadows

Source nodes: [Shadows 18:356](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=18-356)

| Token | Current use |
|---|---|
| `--shadow-sm` | Badge, Icon button |
| `--shadow-md` | Not used on any screen yet |
| `--shadow-primary` | Primary button. Tinted with `--color-orange-600`. |

### 3.6 Assets

Source nodes: listed per asset.

These are vector assets. Export them from Figma; never redraw them. Each asset's colours are bound to the tokens listed.

| Asset | Node | Colour tokens | Used in |
|---|---|---|---|
| Logo/Mark | [29:3](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=29-3) | `--color-blue-600`, `--color-orange-400` | Logo/Full, Header |
| Icon/Menu | [33:32](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=33-32) | `--color-neutral-ink` | Button (Icon), Header |
| Deco/Circle blue | [45:245](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=45-245) | `--color-blue-200` | Hero |
| Deco/Plus | [69:147](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=69-147) | `--color-orange-400` | Hero |
| Deco/Pi | [69:144](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=69-144) | `--color-blue-600` | Hero |
| Deco/Circle peach | [46:249](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=46-249) | `--color-orange-100` | Hero |

---

## 4. Components

Each component has the same subsections. "Not defined in Figma" means the design does not cover that point yet; it is tracked in [7.2](#72-not-designed-yet).

### 4.1 Logo

Source nodes: [Logo section 30:14](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=30-14) · [Logo/Mark 29:3](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=29-3) · [Logo/Full 30:13](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=30-13)

**Purpose.** The brand mark and the full logo.

- Use **Logo/Full** in the header and the footer.
- Use **Logo/Mark** only where space is tight.
- Keep clear space of at least `--space-8` around the logo.
- Do not recolour, stretch or rotate the logo.

**Anatomy.**

- Logo/Mark: one vector asset ([3.6](#36-assets)).
- Logo/Full: Logo/Mark followed by the wordmark "înțeleg" as live text, in one horizontal row.

**Variants and properties.** Two separate components, Logo/Mark and Logo/Full. No properties.

**Tokens used.**

- Mark: `--color-blue-600`, `--color-orange-400` (inside the asset).
- Wordmark: `.text-brand-wordmark`, `--color-neutral-ink`.
- Gap between mark and wordmark: `--space-8`.
- Mark size: component-scoped (`--logo-mark-width`, `--logo-mark-height`).

**Accessibility.**

- The wordmark is real text, so screen readers can read it.
- The mark's accessible name or alt text is not defined in Figma.

**Responsive behaviour.** Fixed size. Logo/Full is used at both 360 and 414. Nothing else is defined in Figma.

### 4.2 Icon

Source nodes: [Icon section 33:7](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=33-7) · [Icon/Menu 33:32](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=33-32)

**Purpose.** Interface icons. The size, stroke and cap style are specified in the Icon card.

**Anatomy.** One vector asset per icon.

**Variants and properties.** One icon exists: Icon/Menu. No properties.

**Tokens used.**

- Colour: `--color-neutral-ink` by default. Inside a button, the icon takes the button's text colour.
- Size: component-scoped (`--icon-size`).

**Accessibility.**

- An icon-only button always needs an `aria-label`, e.g. "Deschide meniul".
- Whether a standalone icon is decorative or meaningful is not defined in Figma.

**Responsive behaviour.** Fixed size. Nothing else is defined in Figma.

### 4.3 Button

Source nodes: [Button section 34:49](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=34-49) · [Button set 33:42](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=33-42)

**Purpose.**

- **Primary:** the main action. Use one per screen. Among interactive elements, orange is reserved for the Primary button.
- **Secondary:** alternative actions, e.g. "Încearcă un exercițiu".
- **Icon:** an icon-only action, e.g. the menu.

**Anatomy.** A container with its content centred, gap `--space-8`. The content is either:

- a label (Primary, Secondary), or
- an icon (Icon variant).

**Variants and properties.**

| Property | Values |
|---|---|
| `Variant` | `Primary` · `Secondary` · `Icon` |
| `Label` (text) | Button text. Applies to Primary and Secondary. |

There is no icon-swap property on the Icon variant; it always shows Icon/Menu.

**Tokens used.**

| Variant | Background | Border | Content | Radius | Shadow | Padding | Size |
|---|---|---|---|---|---|---|---|
| Primary | `--color-orange-400` | — | `.text-ui-button`, `--color-neutral-ink` | `--radius-lg` | `--shadow-primary` | `--space-24` horizontal | `--button-primary-height` |
| Secondary | `--color-neutral-white` | `--color-blue-200` (`--button-secondary-border-width`) | `.text-ui-button`, `--color-blue-600` | `--radius-lg` | — | `--space-24` horizontal | `--button-secondary-height` |
| Icon | `--color-neutral-white` | `--color-neutral-border` | Icon/Menu, `--color-neutral-ink` | `--radius-md` | `--shadow-sm` | — | `--button-icon-size` |

**Accessibility.**

- The Icon variant is the minimum touch target (44 × 44). Primary and Secondary meet it too.
- An Icon button always needs an `aria-label`.
- Label contrast: see [6.4](#64-contrast).
- Hover, focus, active and disabled states are not defined in Figma.

**Responsive behaviour.**

- In the Hero, Primary and Secondary stretch to full width (Fill).
- Nothing else is defined in Figma.

### 4.4 Badge

Source nodes: [Badge section 35:96](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=35-96) · [Badge 35:95](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=35-95)

**Purpose.** A short context label, such as the exam name.

- Keep it to one line, three or four words at most.
- Badges are not clickable. For actions, use a Button.

**Anatomy.** A pill-shaped container holding one text label.

**Variants and properties.**

- No variants.
- `Label` (text). Default: "Evaluarea Națională 2027".

**Tokens used.**

- Background: `--color-neutral-white`
- Text: `.text-ui-badge`, `--color-blue-600`
- Radius: `--radius-full`
- Shadow: `--shadow-sm`
- Padding: `--space-12` horizontal, `--space-4` vertical

**Accessibility.**

- Not interactive: render it as plain text content, not as a control.
- Contrast: see [6.4](#64-contrast).

**Responsive behaviour.** Hugs its content. Nothing else is defined in Figma.

### 4.5 Header

Source nodes: [Header section 37:129](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=37-129) · [Header 36:128](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=36-128)

**Purpose.** The app bar at the top of every screen.

**Anatomy.** A horizontal bar with two sides, pushed to opposite edges.

- **Left: Brand group.** Stacked vertically, gap `--space-4`:
  1. Logo/Full.
  2. The subject line (Subtitle). Its left inset aligns it with the wordmark: inset = `--logo-mark-width` + `--space-8`. This is intentional; in CSS, write it as that sum, not as a number.
- **Right:** a Button, variant Icon, showing Icon/Menu.
- **Bottom:** a border.

**Variants and properties.**

- No variants.
- `Subtitle` (text). Default: "Matematică · clasa a VIII-a".

**Tokens used.**

- Background: `--color-neutral-white`
- Bottom border: `--color-neutral-border`
- Subtitle: `.text-ui-badge` (intentional), `--color-blue-600`
- Padding: `--space-16` horizontal, `--space-12` vertical
- Height: component-scoped (`--header-height`)

**Accessibility.**

- The menu button needs `aria-label="Deschide meniul"`.
- What the menu opens is not defined in Figma.

**Responsive behaviour.** Stretches to full width (Fill). The layout is the same at 360 and 414.

### 4.6 Hero

Source nodes: [Hero section 47:251](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=47-251) · [Hero 46:250](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=46-250)

**Purpose.** The Home introduction. Use it once, at the top of the Home screen, directly below the Header.

**Anatomy.** A vertical stack, clipped to its bounds. From top to bottom:

1. **Intro** group ([39:243](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=39-243)):
   - Badge
   - headline, "Pregătește-te pentru examenul de matematică, pas cu pas"
   - intro text.
2. **Actions** group ([39:244](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=39-244)):
   - **Buttons** group: Primary "Începe testul", then Secondary "Încearcă un exercițiu"
   - centred privacy note, "Fără cont. Progresul rămâne doar pe telefonul tău."
3. **Decorations**, behind the text and clipped by the Hero:
   - top-right corner: Deco/Circle blue, Deco/Plus, Deco/Pi
   - bottom-left corner: Deco/Circle peach.

**Variants and properties.** No variants and no properties. The text is fixed inside the component.

**Tokens used.**

| Part | Tokens |
|---|---|
| Container | `--color-blue-50`; padding `--space-32` vertical, `--space-16` horizontal; gap between Intro and Actions `--space-24` |
| Intro, Actions | gap `--space-16` |
| Buttons group | gap `--space-12` |
| Headline | `.text-heading-display`, `--color-neutral-ink` |
| Intro text | `.text-body-lg`, `--color-neutral-ink-2` |
| Privacy note | `.text-ui-caption`, `--color-neutral-ink-2` |
| Decorations | colour tokens per asset, see [3.6](#36-assets) |

**Accessibility.**

- Decorations are `aria-hidden` in code.
- Text contrast: see [6.4](#64-contrast).
- The headline's heading level is not defined in Figma.

**Responsive behaviour.**

- Stretches to full width (Fill). Height hugs the content, so the headline wraps differently at 360 and 414 and the Hero gets shorter at 414.
- Decorations keep their corner through constraints:
  - top-right group: pinned top and right
  - Deco/Circle peach: pinned bottom and left.
- Header + Hero fit on a 360 × 800 screen without scrolling.

---

## 5. Pages

### 5.1 Home

Source nodes: [Home page 3:3](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=3-3) · [Home – mobile 3:7](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=3-7) (360 × 800) · [Home – mobile 414 48:311](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=48-311) (414 × 896)

**Layout.**

- Sections stack vertically and span the full width.
- Page background: `--color-neutral-50`.
- There are no gaps between sections.

| # | Section | Component | Node (360) | Node (414) |
|---|---|---|---|---|
| 1 | Header | [Header](#45-header) | [48:279](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=48-279) | [48:312](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=48-312) |
| 2 | Hero | [Hero](#46-hero) | [48:294](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=48-294) | [48:313](https://www.figma.com/design/4dlQSgg7V4eszO90nphpLl/inteleg---design?node-id=48-313) |

**Rules.**

- Header + Hero fit on the 360 × 800 frame without scrolling.
- Nothing is designed below the Hero (see [7.2](#72-not-designed-yet)).

---

## 6. General rules

### 6.1 Language

- **App content is Romanian.** All user-facing text: labels, `aria-label`s, messages.
- **Everything else is English:** code names (components, classes, custom properties, files) and all documentation.

### 6.2 Touch targets

Every interactive element has a touch target of at least 44 × 44.

### 6.3 Tokens

Follow [2.4](#24-tokens-only): tokens only, and component-scoped custom properties for dimensions without a token.

### 6.4 Contrast

The target is **WCAG 2.2 AA**:

- **4.5:1** for normal text
- **3:1** for large text
- **3:1** for non-text elements needed to understand the UI.

Ratios are computed from the Figma colour values.

**Text and icons:**

| Foreground | Background | Where | Ratio | AA |
|---|---|---|---|---|
| `--color-neutral-ink` | `--color-neutral-white` | Wordmark (Header) | 15.97:1 | Pass |
| `--color-blue-600` | `--color-neutral-white` | Header subtitle, Badge label, Secondary button label | 5.83:1 | Pass |
| `--color-neutral-ink` | `--color-orange-400` | Primary button label | 7.83:1 | Pass |
| `--color-neutral-ink` | `--color-blue-50` | Hero headline | 14.39:1 | Pass |
| `--color-neutral-ink-2` | `--color-blue-50` | Hero intro text, privacy note | 7.93:1 | Pass |
| `--color-neutral-ink` (icon, non-text) | `--color-neutral-white` | Icon/Menu in the Icon button | 15.97:1 | Pass |
| `--color-neutral-ink-2` | `--color-neutral-white` | Figma documentation pages only (not in the app) | 8.80:1 | Pass |

**Decorations** are `aria-hidden` and carry no information, so they are exempt:

| Foreground | Background | Where | Ratio | AA |
|---|---|---|---|---|
| `--color-orange-400` | `--color-blue-200` | Deco/Plus on Deco/Circle blue | 1.52:1 | Exempt |
| `--color-blue-600` | `--color-blue-200` | Deco/Pi on Deco/Circle blue | 4.33:1 | Exempt |

**Logo/Mark** (`--color-blue-600`, `--color-orange-400` on `--color-neutral-white`) is exempt too: WCAG excludes logos and brand marks from contrast requirements.

**Component boundaries, for information.** The label or icon identifies each button, so WCAG does not require these outlines to reach 3:1:

| Boundary | Against | Ratio |
|---|---|---|
| Secondary border, `--color-blue-200` | `--color-blue-50` (Hero) | 1.21:1 |
| Secondary border, `--color-blue-200` | `--color-neutral-white` | 1.35:1 |
| Icon button border, `--color-neutral-border` | `--color-neutral-white` (Header) | 1.18:1 |

---

## 7. Open decisions

### 7.1 Figma inconsistencies to fix

None open as of 2026-11-26. All the inconsistencies found while writing this document were fixed in Figma or confirmed as intentional (see [8](#8-decision-log)).

### 7.2 Not designed yet

| # | Topic | Notes |
|---|---|---|
| 1 | **Interaction states** | Focus indicator for every interactive element, plus hover, active and disabled states. WCAG 2.2 requires a visible focus indicator with enough contrast. |
| 2 | **Dark theme and semantic tokens** | A dark theme is planned, so a semantic token layer (e.g. "action/primary" instead of "orange/400") will be needed. It must be defined in Figma first. |
| 3 | Menu | What the Header menu button opens, and how. |
| 4 | Button destinations | What "Începe testul" and "Încearcă un exercițiu" lead to. |
| 5 | Footer | The Logo usage note mentions a footer; none is designed. |
| 6 | Wider layouts | Tablet and desktop layouts, and the breakpoints between them. |
| 7 | Unused tokens | Roles for tokens marked "not used on any screen yet" in [3](#3-foundations), including how the `feedback/*` answer-state colours pair as text and background. |
| 8 | Heading structure | The heading level of the Hero headline and the page's heading outline. |
| 9 | Logo accessibility | The accessible name or alt text for Logo/Mark. |
| 10 | Icon semantics | Whether standalone icons are decorative or meaningful. The icon-only button rule ([4.2](#42-icon)) is defined. |
| 11 | Hero content | The Hero's text is fixed inside the component. Where the content comes from in code is not defined. |
| 12 | Font loading | How Nunito is served, and which weights and character sets (including Romanian diacritics) are loaded. |
| 13 | Motion | No transitions or animations are defined. |
| 14 | Browser text wrapping | At 360, Deco/Pi's bounding box touches the headline's text box, though the glyphs don't overlap. Check during implementation that browser line wrapping matches Figma and that no text and decoration collide. |
| 15 | Repository structure | Where the Angular app lives and the path of `tokens.css` are decided in the Angular setup unit. |

---

## 8. Decision log

| Date | Decision | Reason |
|---|---|---|
| 2026-11-26 | Figma is the single source of truth. This document lists names, roles and rules; values live only in the global tokens stylesheet (`tokens.css`), which will be generated from Figma (not created yet). | Avoid two diverging copies of the design |
| 2026-11-26 | `tokens.css` is written once from Figma through the Figma MCP and verified against Figma. An automated pipeline (Figma → JSON → Style Dictionary) may replace this later. | A single source of truth without the setup cost of a pipeline at this project size |
| 2026-11-26 | Token naming: type prefix + the Figma name with `/` replaced by `-` ([2.1](#21-variables-and-effect-styles--css-custom-properties)). | The names map one-to-one and predictably, so they can be generated |
| 2026-11-26 | Text styles become one custom property per font property plus one `.text-*` class per style. | Letter-spacing is not part of the CSS `font` shorthand; the class keeps each style as one unit |
| 2026-11-26 | Component dimensions with no Figma variable use component-scoped custom properties linked to their Figma node ([2.3](#23-component-scoped-custom-properties)). | Keeps "no hard-coded values" without inventing global size tokens |
| 2026-11-26 | No semantic aliases for now. A dark theme is planned, so a semantic layer will be defined in Figma first ([7.2](#72-not-designed-yet) #2). | Figma has only primitive tokens; aliases in code would invent a layer |
| 2026-11-26 | Tokens not used on any screen are documented with their Figma group role only. `feedback/*` = answer states (correct / incorrect). | Don't invent roles the design hasn't assigned |
| 2026-11-26 | Numbers allowed in this document: viewport widths (360, 414), the 44 × 44 touch target, contrast ratios and WCAG thresholds. Everything else is a token name or a Figma link. | Values live only in Figma and `tokens.css` |
| 2026-11-26 | Figma variable collections confirmed by the design owner: "Colors" (20 variables) and "Spacing" (`space/4`–`space/48` including `space/20`, plus `radius/*`), one mode each. | `space/20` and unused variables can't be read from bound nodes alone |
| 2026-11-26 | Letter-spacing fixed in Figma: `brand/wordmark`, `heading/display` and `heading/h1` use negative percentages, and the Foundations labels match. | The styles and their labels disagreed on sign and unit |
| 2026-11-26 | Hero "Intro" and "Actions" gaps bound to `space/16` in Figma. | They were raw values |
| 2026-11-26 | The Hero's π and "+" flattened to vector assets (Deco/Pi, Deco/Plus) in Figma, with fills bound to `blue/600` and `orange/400`. | They were text with no text style |
| 2026-11-26 | Deco/Pi and Deco/Plus moved in Figma so they clear the headline at 360. | After the letter-spacing fix, the headline's first line reached Deco/Pi |
| 2026-11-26 | The Header tagline inset is intentional and defined as `Logo/Mark` width + `space/8`. | It aligns the subtitle with the wordmark |
| 2026-11-26 | The Header subtitle uses `ui/badge` by design. | Intentional reuse; no separate style needed |
| 2026-11-26 | `orange/600` is used only as the colour of `shadow/primary`, by design. | Intentional |
| 2026-11-26 | The "Maketa" rectangle on the Home page is excluded. | It is a working reference, not part of the design |
| 2026-11-26 | Angular component names are left out of this document. | They belong to the Angular construction unit |
| 2026-11-26 | Sections added to the requested outline: 3.6 Assets, 6.4 Contrast, Open decisions split into 7.1 and 7.2, and a "Source nodes" line under each section. | Assets are referenced by components; contrast needs a home; Figma fixes and missing design need different owners; every section links to Figma |
