---
date: 2026-07-12
timezone: Africa/Lagos
status: open
headline: "Migrating the Laitstyles products page without breaking storefront truth"
projects_touched:
  - laitstyles
started_at: "2026-07-12T21:00:00+01:00"
last_updated: "2026-07-12T22:05:00+01:00"
closed_at: null
tags:
  - nextjs
  - storefront
  - responsive-design
  - migration
---

# Day in the Life — 2026-07-12

## Today's Headline

> Migrating the Laitstyles products page without breaking storefront truth.

## Starting Point

### What I intended to work on

- Extend the completed Phase 2-3N storefront homepage migration to the public product listing.
- Decide whether the listing belonged at `/shop` or `/products` based on the live application rather than naming preference.

### Context carried into today

- The checkout already had an uncommitted high-fidelity homepage migration.
- `inventory.html` was the structural and visual reference, while the live repository remained authoritative for routes, product data, and filtering.

## Today's Timeline

### 21:00 — Laitstyles storefront catalogue migration

**Objective**

Migrate the product listing to the inventory reference while preserving the completed homepage, real catalogue queries, and canonical product URLs.

**What I found**

- The storefront information architecture defines `/shop` as the product listing and `/products/[slug]` as the product detail route.
- Creating `/products/page.tsx` would have introduced a competing listing route.
- The completed homepage card existed as a private homepage component, while `/shop` used a separate catalogue card.

**Approach**

- Kept `/shop` as the canonical listing route.
- Extracted the completed homepage product card into one shared storefront component.
- Added a typed linked variant for server-backed catalogue products so cards continue to open `/products/[slug]` without pretending save or cart mutations exist.
- Ported the inventory intro, quick navigation, sticky controls, desktop filter rail, product-grid rhythm, responsive behavior, and empty state into the existing scoped storefront stylesheet.
- Preserved live repository products, prices, availability, categories, filters, sorting, and product links.

**Why this approach**

It follows the repository route contract, avoids duplicate product datasets and card markup, and keeps the migration bounded to the requested products surface.

**Work completed**

- Migrated `/shop` to the inventory-page hierarchy.
- Reused the completed homepage card through a shared component.
- Kept the products page as a Server Component with only the existing small client boundaries.
- Corrected the shared header breakpoint so 768px no longer overflows.
- Restored filter-sheet access at the 768px tablet breakpoint.

**Verification**

- TypeScript passed after regenerating stale Next.js route types.
- Vitest passed: 35 files, 139 tests.
- Next.js 16.2.10 production build passed and generated `/shop` plus `/products/[slug]` routes.
- Playwright verified 375×812, 768×1024, and 1280×800. All 7 live products rendered; document width matched viewport width after the tablet fix; the tablet filter sheet opened as an accessible dialog and closed with Escape.
- Changed-scope Biome checks passed with two intentional plain-image performance warnings inherited from the homepage card convention.
- React Doctor completed at 75/100 with 58 warnings in pre-existing Control Room, homepage-lab, and shared UI code; no new `/shop` route failure was reported.

**Challenges**

- The Windows pnpm wrapper attempted an unnecessary install and failed to create temporary files, so checks ran through the installed local binaries.
- Stale `.next/types/validator.ts` entries referenced deleted homepage files; clearing only the generated `.next` cache and rerunning Next type generation fixed the false compiler errors.
- Browser validation initially found 14px horizontal overflow at exactly 768px because the shared header selected desktop layout at that boundary.

**Remaining known issues**

- Local production preview reports a 404 for Vercel Analytics outside Vercel.
- The pre-existing shared header points at the unimplemented `/account/saved` route, which can produce a prefetch 404.
- Repository-wide Biome remains blocked by pre-existing errors outside this migration.

**Outcome**

- Completed with verified route, build, tests, responsive layout, and tablet filter behavior.

## Decisions and Trade-offs

### Keep the listing at `/shop`

**Context**

- The request allowed `/shop` or `/products`, whichever best fit the application.

**Chosen direction**

- `/shop` remains the listing; `/products/[slug]` remains product detail.

**Reason**

- This is the documented and already-implemented route pattern, so it avoids route competition and broken links.

### Do not translate prototype scripts wholesale

**Chosen direction**

- Retain existing URL-backed server filtering and product links; omit prototype-only localStorage cart, saved-item, image-viewer, fake delivery, and injected DOM behavior.

**Reason**

- Those scripts would have duplicated or fabricated application behavior beyond the controlled migration.

## What I Learned

### Technical learning

- Exact breakpoint checks matter: a layout can pass at 767px and still overflow at the required 768px boundary.
- A shared visual component can support prototype interactions and real server-backed navigation through an explicit typed variant without duplicating markup.

### Workflow learning

- Route documentation resolved a naming question faster and more safely than creating a new top-level route.
- Generated Next.js route types should be regenerated before treating missing deleted pages as source-code failures.

## Evidence and Proof of Work

- Relevant route: `src/app/(shop)/shop/page.tsx`
- Shared card: `src/features/storefront/product-card.tsx`
- Inventory adapter: `src/features/storefront/catalogue/inventory-results.tsx`
- Storefront styles: `src/features/storefront/home-lab/home-lab.css`
- Tests: 139 passed.
- Build: passed on Next.js 16.2.10.
- Browser evidence: Playwright screenshots at 375×812, 768×1024, and 1280×800.

## Content Bank

### Potential LinkedIn angles

- Why route architecture should settle naming debates before implementation begins.
- The 768px breakpoint bug that only appeared when validating exact acceptance dimensions.

### Potential X/Twitter posts

- “The spec said products, the app said shop. The right answer was to read the route contract, not add another page.”

## End-of-Day Reflection

This journal remains open. The products-page migration is verified, while unrelated repository lint debt and two pre-existing local preview 404s remain visible rather than being presented as fixed.
