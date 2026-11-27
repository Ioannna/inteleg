# Units of work

This project is built in small **units**. Each unit has a plan in this folder. The plan is reviewed and approved before any code is written, and the finished unit lands as one commit. When a unit is done, its plan records what changed from the approved version.

The UI follows [`docs/design.md`](../design.md), which is translated from the Figma design.

**Status values:** Not started · Plan in review · Approved · Done

## Iteration 1: Home page

### Intent

înțeleg is a mobile-first web app that helps 8th-grade students in Romania prepare for the Evaluarea Națională math exam. The app is in Romanian.

This iteration builds the Home page (Header and Hero) as designed in Figma, in Angular, and publishes it on a free static host. It is the first working version of the app.

**In scope:**

- The Angular app in `frontend/`.
- `tokens.css` from Figma, the Nunito font and the SVG assets.
- The components Home needs (Logo, Icon, Button, Badge, Header, Hero) and the Home page, including the focus state.
- Automatic deployment of the frontend.

**Out of scope:**

- The backend.
- Everything [`docs/design.md` 7.2](../design.md#72-not-designed-yet) lists as not designed: the menu, the button destinations, the other screens, and the hover, pressed and disabled states.

**Constraints:**

- No personal data: the users are minors, so there are no accounts, analytics, cookies or third-party requests.
- WCAG 2.2 AA.
- Mobile-first: reference width 360, verified at 414, supported from 320.

### User stories

**US-1: Understand the app at a glance.** As a student, I want to see what înțeleg does as soon as I open it, so that I can decide whether to start.

- **Given** a 360 × 800 viewport, **when** Home loads, **then** the Header and Hero are fully visible without scrolling.
- **Given** Home has loaded, **then** the badge, headline, intro text, both buttons and the privacy note show the Romanian text from Figma.
- **Given** a 414 viewport, **then** the layout matches the Figma 414 frame, including the headline's line breaks.
- **Given** a 1280 × 800 viewport, **then** the Header and Hero sit in a centred column 414 wide on the page background, as in the Figma desktop frame.

**US-2: See the two ways to begin.** As a student, I want an obvious main action and a lighter alternative, so that I know where to start.

- **Given** Home, **then** "Începe testul" is the only orange control, and "Încearcă un exercițiu" appears below it as the Secondary button.
- **Given** either button, **then** it is a native button whose accessible name equals its label.
- **Given** a tap on either button, **then** nothing happens and no error occurs. Their destinations are not designed yet.

**US-3: Use the page with a keyboard or a screen reader.** As a student who doesn't use a mouse, I want to reach and understand every control, so that I can use the app like anyone else.

- **Given** Home, **when** I press Tab, **then** focus moves through the menu button, "Începe testul" and "Încearcă un exercițiu", in that order.
- **Given** a control has keyboard focus, **then** the focus ring from `design.md` 6.5 is visible: outside the control, with a gap, following the control's radius.
- **Given** a mouse click on a button, **then** no focus ring appears.
- **Given** a screen reader, **then**:
  - the menu button is announced as "Deschide meniul", button
  - decorations are not announced
  - the Hero headline is the page's only level-1 heading
  - the page language is Romanian.

**US-4: Use it on any phone.** As a student with a small or large phone, I want the page to fit my screen, so that I never scroll sideways or miss content.

- **Given** any width from 320 to 414, **then** nothing scrolls horizontally, and no text is clipped or overlapped by a decoration.
- **Given** a width above 414, **then** the app column stays centred and nothing breaks.
- **Given** 200% browser zoom at a 1280 width, **then** all content stays readable and usable.

**US-5: Open it from a link, without an account.** As a student, I want to open the app from a link in my phone's browser, so that I don't have to install anything or sign up.

- **Given** the public URL, **when** I open it on a phone over a mobile connection, **then** Home loads and works.
- **Given** a new commit on `main`, **when** it is pushed, **then** the site updates automatically.

**US-6: Not be tracked.** As a student, I want to know that nothing about me is collected, so that I can use the app safely.

- **Given** Home is loading, **then** the browser makes requests only to the app's own origin.
- **Given** Home, **then** no cookies are set, and nothing is written to browser storage in this iteration.

### Non-functional requirements

| ID | Requirement | How it's verified |
|---|---|---|
| NFR-A1 | WCAG 2.2 AA | axe-core reports no violations at 320, 360 and 414; Lighthouse accessibility scores 100 |
| NFR-A2 | Contrast as in `design.md` 6.4 | Same token pairs as 6.4; the axe contrast rule passes |
| NFR-A3 | Touch targets of at least 44 × 44 | Each control's bounding box is measured in an automated check |
| NFR-A4 | Visible focus on every control (`design.md` 6.5) | Keyboard walk-through, plus an automated check of the focused element's outline |
| NFR-A5 | Text spacing (WCAG 1.4.12) and resize (1.4.4) | With the WCAG text-spacing override applied, nothing is clipped; at 200% zoom nothing is lost |
| NFR-W1 | Reflow from 320 to 414 | At 320, 360 and 414, the page's scroll width is no larger than the viewport width |
| NFR-W2 | Matches Figma at 360 and 414 | Side-by-side with the Figma frames: same line breaks, and spacing measured in DevTools equals the tokens |
| NFR-W3 | Above 414: the app column stays 414 wide and centred on the page background, as in the Figma frame "Home – desktop" | Matches the desktop frame at 1280 × 800; no horizontal scrolling or overlap at 768, 1280 and 1440 |
| NFR-B1 | Browsers: the last two versions of Chrome (desktop and Android), Safari (iOS), Firefox and Edge | Automated checks in Chromium, Firefox and WebKit; manual checks in Chrome, Firefox and Edge on desktop with device emulation, and on an Android phone |
| NFR-P1 | Lighthouse mobile performance ≥ 90; LCP ≤ 2.5 s; CLS ≤ 0.1 | Median of three Lighthouse runs on the deployed URL, recorded in the unit plan |
| NFR-P2 | The production build stays within Angular's default budgets | The build prints no budget warnings |
| NFR-S1 | No third-party requests, cookies or storage | Network and Application panels in DevTools |
| NFR-Q1 | Clean build, lint and tests | Build, lint, unit tests and end-to-end checks exit with no errors or warnings, locally and in CI |

### Risks and assumptions

| # | Risk or assumption | How it is reduced or checked |
|---|---|---|
| 1 | Assumption: Angular 22 supports Node 26, and its default unit-test runner is Vitest | Checked when the app is generated (U01). If either is wrong, work stops and the plan is revised. |
| 2 | Browser line breaks may differ from Figma's (`design.md` 7.2 #14) | Line breaks compared at 360 and 414 (U09). Any difference is resolved in Figma, not patched in code. |
| 3 | Something may not fit at 320, a width that isn't designed | 320 checked first in U08 and U09. If it doesn't fit, the fix is decided in Figma. |
| 4 | Figma asset download links expire after 7 days | SVGs downloaded in the same unit that uses them, and compared with Figma. |
| 5 | Token values copied from Figma by hand could contain mistakes | A per-token checklist in U03, read from Figma and reviewed. |
| 6 | "Fits 360 × 800" is ambiguous on a phone, where the browser bars take space | Defined as a 360 × 800 CSS viewport in DevTools. |
| 7 | Safari on iOS is not tested on a device | Automated checks run in WebKit; recorded as a known gap. |
| 8 | The host's free tier or terms could change | The site is plain static files; moving to another host is one small unit. |
| 9 | Lighthouse scores vary from run to run | Median of three runs on the deployed URL. |
| 10 | Buttons with no action may confuse a student | A recorded interim decision (`design.md` 7.2 #3, #4); the destinations come in a later iteration. |

### Units

| # | Unit | Goal | Stories | Depends on | Status | Commit |
|---|---|---|---|---|---|---|
| 01 | [Angular setup](01-angular-setup.md) | An empty, strict Angular app in `frontend/`, with linting and line-ending rules | Foundation | — | Approved | — |
| 02 | Deploy | Build, lint and test on every push to `main`, and publish to GitHub Pages; root README | US-5 | 01 | Not started | — |
| 03 | Design tokens | `tokens.css` from Figma, global base styles and the global focus rule | Foundation | 01 | Not started | — |
| 04 | Typography and font | Self-hosted Nunito and the `.text-*` classes | US-1, US-6 | 03 | Not started | — |
| 05 | Logo and icon | Logo/Full and Icon components, their SVG assets, and the favicon | US-1, US-3 | 04 | Not started | — |
| 06 | Button | Primary, Secondary and Icon variants, with the focus ring | US-2, US-3 | 03, 04, 05 | Not started | — |
| 07 | Badge | The Badge component | US-1 | 04 | Not started | — |
| 08 | Header | The Header, placed on Home in the App column | US-1, US-3, US-4 | 05, 06 | Not started | — |
| 09 | Hero | The Hero with its decorations, placed on Home | US-1 to US-4 | 06, 07, 08 | Not started | — |
| 10 | Home verification | Playwright and axe-core checks in CI at 320, 360, 414 and 1280; Lighthouse results | All | 02, 09 | Not started | — |
