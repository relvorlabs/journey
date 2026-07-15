# Laitstyles

## Durable storefront decisions

- The canonical public catalogue/listing route is `/products`; legacy `/shop` and `/shop/category/[slug]` URLs permanently redirect there through `next.config.ts`.
- Canonical product detail URLs are `/products/[slug]`.
- The legacy `(shop)` App Router group and its `/p/[slug]` route were retired on 2026-07-15; `/p/[slug]` remains compatible through a permanent configuration redirect.
- Storefront listing pages must read the same repository-backed product truth used by the application; prototype product arrays are visual references only.
- The Phase 2-3N homepage product card is shared with the catalogue through `src/features/storefront/product-card.tsx`; real catalogue usage selects its typed link variant rather than fake cart or saved-item behavior.
- The root layout mounts a narrow client `StorefrontExperienceProvider` for prototype auth-dialog access, hold countdown feedback, and storefront toasts across routes. Real Supabase auth routes remain authoritative; the provider must not be described as production authentication or server-backed hold persistence.
- Application actions use the unmodified shadcn `base-mira` Button primitive. Feature code selects built-in variants and sizes; generic anchor styling must exclude `[data-slot="button"]`, and Button-rendered links must set Base UI `nativeButton={false}`.

## Source journal

- `myday/2026/07/2026-07-12--migrating-laitstyles-products-page.md`
- `myday/2026/07/2026-07-13--lifting-storefront-feedback-into-root-layout.md`
- `myday/2026/07/2026-07-15--storefront-home-architecture-clean-sweep.md`
