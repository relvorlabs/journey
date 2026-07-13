---
date: 2026-07-13
timezone: Africa/Lagos
status: open
headline: "Lifting storefront feedback into the Laitstyles root layout"
projects_touched:
  - laitstyles
started_at: "2026-07-13T10:25:55+01:00"
last_updated: "2026-07-13T10:25:55+01:00"
closed_at: null
tags:
  - nextjs
  - react
  - storefront
  - auth-ui
  - shared-state
---

# Day in the Life — 2026-07-13

## Today's Headline

> Lifting storefront feedback into the Laitstyles root layout without turning the layout into a Client Component.

## Starting Point

### What I intended to work on

- Move the account page's auth dialog, toast feedback, and “You're holding” countdown alert into the root layout so every route beneath it can use the same state and actions.

### Context carried into today

- `src/app/account/page.tsx` was a large Client Component that owned its own fake signed-in state, auth form, item countdown, toast, and bottom hold alert.
- The storefront specification explicitly says auth should not be reproduced inside `account/page.tsx` and that server-backed Supabase auth remains a separate phase and route boundary.

## Today's Timeline

### Morning — Laitstyles root storefront experience boundary

**Objective**

Expose the account feedback surfaces across the application while preserving the root layout as a Server Component.

**What I found**

- The root layout already composes `ThemeProvider` and `SiteChrome`.
- The account page held one seeded reservation and derived the hold count and countdown locally.
- The repository also has real Supabase auth routes, so the extracted dialog must remain clearly labelled as prototype behaviour rather than being presented as production authentication.

**Approach**

- Added a narrow `StorefrontExperienceProvider` under `ThemeProvider` in the root layout.
- Lifted prototype item/hold state, auth-dialog state, toast state, countdown effects, and shared actions into the provider.
- Extracted the auth dialog and global feedback UI into focused storefront shared modules.
- Made the account page consume the provider while keeping address dialogs and the account cart sheet local to the account feature.

**Why this approach**

- It gives sibling routes access without prop drilling.
- The root layout remains server-first; only the interactive provider is a Client Component.
- It removes duplicated global overlays from the account page and keeps the real Supabase auth boundary intact.

**Work completed**

- Added the root provider and shared storefront data/UI modules.
- Removed account-local auth, hold-alert, toast, and countdown ownership.
- Preserved the existing account interactions through typed provider actions.
- Added an accessible label to the decorative phone SVG while cleaning the changed file.

**Files, systems, or areas touched**

- `src/app/layout.tsx`
- `src/app/account/page.tsx`
- `src/features/storefront/shared/storefront-experience-provider.tsx`
- `src/features/storefront/shared/storefront-auth-dialog.tsx`
- `src/features/storefront/shared/storefront-feedback.tsx`
- `src/features/storefront/shared/storefront-experience-data.ts`

**Technologies and tools**

- Next.js 16 App Router
- React 19 context and Client Component boundaries
- Base UI/shadcn dialogs and alerts
- Biome, TypeScript, Vitest, React Doctor, and Playwright CLI

**Challenges**

- The pnpm wrapper attempted a dependency-status install and failed with a Windows `EPERM`, so checks used installed local binaries.
- TypeScript emitted no diagnostics but did not exit within a six-minute ceiling.
- Turbopack build first hit a disappearing `.next/static` temporary manifest file, then timed out; the webpack build also timed out and ended with a broken pipe after the command ceiling.
- React Doctor timed out without producing a report.
- Browser snapshots proved the global hold alert renders on both `/auth/sign-in` and `/shop`, but the local dev HMR WebSocket repeatedly returned `ERR_INVALID_HTTP_RESPONSE`. The existing theme toggle also failed, confirming the page was not hydrating; interaction validation therefore remains blocked by the dev runtime rather than being claimed as passed.

**Outcome**

- Partially completed: implementation, formatting, tests, and static cross-route browser rendering are verified; compiler/build/React Doctor and hydrated browser interaction gates remain blocked by the local Windows runtime.

**Next action**

- Re-run TypeScript, production build, React Doctor, and hydrated browser interactions in a stable dev/build environment before treating this storefront boundary as fully accepted.

## Decisions and Trade-offs

### Keep the root layout server-first

**Chosen direction**

- Mount one client provider inside the existing layout composition instead of adding `"use client"` to `layout.tsx`.

**Reason**

- Only auth/hold/toast interactions require browser state; route composition and metadata do not.

### Preserve the prototype boundary

**Chosen direction**

- Reuse the existing dialog behaviour but document that real Supabase session creation still belongs to the dedicated auth routes.

**Reason**

- Moving UI ownership must not convert fake timers or local state into a production-auth claim.

## What I Learned

### Technical learning

- Lifting shared feedback into a provider can reduce a page by hundreds of lines while retaining a narrow Client Component boundary.
- Static rendering across routes is useful evidence, but it is not evidence that React effects and event handlers hydrated.

### Workflow learning

- A control interaction, such as the pre-existing theme toggle, helps distinguish a feature handler bug from an application-wide hydration failure.

## Challenges and Friction

- Technical blockers: non-terminating TypeScript/build/React Doctor processes and a broken local HMR WebSocket.
- Tool limitations: the browser could render server output, but client interaction evidence was invalid because the whole page failed to hydrate.
- Decisions still unresolved: whether the prototype provider should later be replaced or adapted when real cart/hold/auth repositories land.

## Evidence and Proof of Work

- Biome changed-scope check: passed.
- Vitest: 35 files and 139 tests passed with a realistic filesystem-scan timeout.
- Browser evidence: the same root hold alert rendered on `/auth/sign-in?next=/account` and `/shop`; all requested static bundles returned HTTP 200.
- Blocked checks: TypeScript exit, Next production build, React Doctor, and hydrated browser interaction.

## Content Bank

### Potential LinkedIn angles

- Why moving global UI into a root layout does not require making the layout itself a Client Component.
- The difference between “the browser rendered it” and “the application hydrated it.”

### Potential X/Twitter posts

- “A server-rendered alert on two routes proves placement, not interaction. A dead theme toggle proved the whole dev page had failed to hydrate.”

## End-of-Day Reflection

This journal remains open. The code boundary and tests are in place, while the non-terminating compiler/build tools and broken HMR loop remain explicit blockers rather than being presented as successful verification.
