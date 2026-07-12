# Laitstyles

## Durable storefront decisions

- The canonical public catalogue/listing route is `/shop`.
- Canonical product detail URLs are `/products/[slug]`.
- Storefront listing pages must read the same repository-backed product truth used by the application; prototype product arrays are visual references only.
- The Phase 2-3N homepage product card is shared with the catalogue through `src/features/storefront/product-card.tsx`; real catalogue usage selects its typed link variant rather than fake cart or saved-item behavior.

## Source journal

- `myday/2026/07/2026-07-12--migrating-laitstyles-products-page.md`
