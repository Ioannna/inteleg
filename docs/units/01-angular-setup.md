# U01: Angular setup

**Status:** Approved
**Iteration:** 1, Home page ([index](README.md))
**Depends on:** nothing

## Goal

An empty, strict Angular 22 app in `frontend/`. It builds, lints and tests cleanly and shows an empty Home page. Every later unit builds on it.

## Stories

None directly. This unit is the foundation for US-1 to US-6.

## Scope

**In:**

1. Generate the app with the Angular CLI in `frontend/`:
   - standalone components, strict TypeScript
   - plain CSS, routing, no server-side rendering
   - npm as the package manager.
2. Keep the CLI defaults for everything the plan doesn't name: file names without type suffixes (`home.ts`, class `Home`), the unit-test runner and the change-detection setup. Record which defaults Angular 22 uses in "Changes from the approved plan".
3. Remove the CLI placeholder content:
   - the welcome template in `app.html`
   - the Angular favicon and its `<link>`
   - the generated `frontend/README.md`. The root README comes in U02.
   - `frontend/.vscode/`. VS Code reads only the `.vscode/` folder at the workspace root, so this one would be unused.
4. Page metadata in `src/index.html` ([`design.md` 6.6](../design.md#66-page-metadata)):
   - `<html lang="ro">`
   - `<title>înțeleg</title>`
   - `<meta name="description" content="Pregătește-te pentru Evaluarea Națională la Matematică, pas cu pas.">`

   The favicon comes in U05, together with the Logo/Mark asset.
5. An empty Home page:
   - `src/app/home/home.ts`, `home.html`, `home.css`, `home.spec.ts`, generated with `ng generate component home`
   - shown on the empty path `''` in `app.routes.ts`
   - the root template (`app.html`) contains only `<router-outlet />`.
6. Linting with angular-eslint (`ng add angular-eslint`):
   - the recommended TypeScript and template rule sets
   - the template accessibility rule set
   - `@typescript-eslint/no-explicit-any` set to error.
7. Node version pinned to 26: `frontend/.nvmrc`, and `engines.node` in `frontend/package.json`. CI in U02 reads `.nvmrc`.
8. `.gitattributes` at the repository root, containing `* text=auto eol=lf`. Then `git add --renormalize .` brings the files already committed into line with it.

**Out:**

- Tokens, fonts, assets and components (U03 to U09).
- CI and deployment (U02).
- Any visible content on Home.

## Approach and decisions

**Folder structure.** Code is grouped by feature, as the Angular style guide recommends, not by type: there are no `components/` or `services/` folders. Home lives in `src/app/home/`. Components shared across screens will live in `src/app/shared/` when U05 creates the first one.

**Routing.** Home is registered directly on the `''` route, not lazy-loaded. With a single page, lazy loading adds nothing. Later screens can be lazy-loaded when they exist.

**Linting.** `ng lint` checks the "no `any`" and "no unused imports" rules from `CLAUDE.md` automatically, instead of relying on review alone.

**Line endings.** `.gitattributes` makes Git store and check out text files with LF on every machine, so Windows and the Linux CI runner see identical files.

**Comparison with MERN:**

| Angular | MERN equivalent |
|---|---|
| `ng new` | `npm create vite@latest` |
| `angular.json` | `vite.config.js` |
| `app.routes.ts` | React Router's route list |
| A standalone component | A React function component that imports what it uses |
| `ng lint` (angular-eslint) | ESLint with `eslint-plugin-react` and `jsx-a11y` |

## Commands (PowerShell, from the repository root)

```powershell
npx @angular/cli@22 new inteleg --directory frontend --style css --routing --ssr false --skip-git --package-manager npm --interactive false
cd frontend
npx ng generate component home
npx ng add angular-eslint
```

If an Angular 22 CLI flag differs from these, the exact command used is recorded in "Changes from the approved plan".

## Files

**Created:**

- `.gitattributes`
- `frontend/`: the CLI scaffold (`angular.json`, `package.json`, `package-lock.json`, the `tsconfig` files, `.editorconfig`, `.gitignore`, `src/`, `public/`)
- `frontend/.nvmrc`
- `frontend/eslint.config.js`, added by `ng add angular-eslint`
- `frontend/src/app/home/home.ts`, `home.html`, `home.css`, `home.spec.ts`

**Changed after generation:**

- `frontend/src/index.html`: metadata
- `frontend/src/app/app.html`: only `<router-outlet />`
- `frontend/src/app/app.routes.ts`: the Home route
- `frontend/src/app/app.spec.ts`: tests adapted to the new root template
- `frontend/package.json`: `engines`
- `frontend/eslint.config.js`: the accessibility rule set and `no-explicit-any`

**Deleted after generation:**

- `frontend/README.md`
- `frontend/.vscode/`
- `frontend/public/favicon.ico`

**Docs:**

- this file, updated at the end
- [`README.md`](README.md): unit status.

## Acceptance criteria

- `npx ng version` shows Angular 22 and Node 26.
- `npx ng build` finishes with no errors and no warnings.
- `npx ng test --watch=false` passes.
- `npx ng lint` reports no problems.
- `npx ng serve`, opened at `http://localhost:4200/`:
  - shows an empty page with no Angular placeholder content
  - the tab title is "înțeleg"
  - `<html lang="ro">` and the meta description are present.
- `git ls-files --eol` shows `i/lf` for every text file.
- No file under `frontend/` contains `any` as a type.

Known and accepted until U05: the browser's automatic request for `/favicon.ico` returns 404.

## Tests

| Spec | What it checks |
|---|---|
| `app.spec.ts` | The root component renders a `<router-outlet>`. |
| `home.spec.ts` | Home is created. |
| `app.spec.ts` (routing) | Navigating to `''` with `RouterTestingHarness` renders Home. |

There is no business logic in this unit, so these specs only check the wiring.

## How to review (PowerShell)

```powershell
cd frontend
npm ci
npx ng build
npx ng test --watch=false
npx ng lint
npx ng serve
```

## Definition of done

- [ ] Build, lint and tests pass with no errors or warnings.
- [ ] All acceptance criteria are met.
- [ ] UI checks at 320, 360 and 414: not applicable, because there is no UI yet.
- [ ] This file records what changed from the approved plan.
- [ ] The code has been reviewed and can be explained line by line.

## Changes from the approved plan

To be filled in when the unit is done.
