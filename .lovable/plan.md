## Goal
Bring every page from Accounts → Contact Us into closer visual parity with aplusfcu.org, fix the mega-menu featured images, and verify responsiveness at desktop/tablet/mobile.

## 1. Navbar dropdown image fix
The featured panel images come from `https://aplusfcu.org/wp-content/uploads/global/img/menu-feature_*.webp`. These hotlinked URLs are returning blank/blocked in our preview (CORS/hotlink protection or wrong path).

Fix:
- Download the 6 menu-feature images locally into `src/assets/nav/` and import them into `src/data/site.ts` so they bundle.
- Update `Header.tsx` `MegaPanel` to render the featured card with: `aspect-[4/3]` image, white card on green background, eyebrow label, bold serif title, body, and a "Learn More →" link — matching the live site's right-rail card.
- Verify spacing: 4-column grid (3 link cols + 1 featured), gap-10, py-10, max-w container, brand-green panel background with white inner card.
- Tighten hover-intent (open 80ms / close 180ms — already set) and confirm caret indicator under active trigger.

## 2. Per-page hero corrections
The yellow `PageHero` is being applied site-wide. On aplusfcu.org only a subset of landing pages use the yellow hero; most interior pages use a white/cream hero with a right-side image, breadcrumb, and green CTA button.

Plan:
- Introduce a second hero component `SplitHero` (white bg, breadcrumb, eyebrow, serif H1, body, primary CTA on left; image on right; subtle bottom border).
- Keep `PageHero` (yellow) only for top-level section landing pages where the live site uses it: `/accounts`, `/loans`, `/services`, `/business`, `/who-we-are`, `/guidance`.
- Switch all child pages to `SplitHero`:
  - Accounts: checking, savings, certificates, youth
  - Loans: home-loans, vehicle-loans, personal-credit, student-loans
  - Services: online-banking, mobile-banking, member-services, insurance-investments
  - Business: accounts, lending, services
  - Who We Are: about, community, membership
  - Guidance: financial-education, rates, tools
  - Contact Us, Locations, Join

## 3. Sequential page audit
For each page, compare to live site and adjust:
- Hero variant (per §2)
- Section order, card grid columns, icon usage
- Spacing scale (py-16 lg:py-24 for sections, container max-w-1200)
- Typography: serif for H1/H2, sans body, ink color
- Color usage: green CTAs, yellow accents only where the live site uses them
- Footer remains shared

## 4. Responsive QA
Screenshot each updated page at 1440, 1024, 768, 390. Fix:
- Mega menu hidden < lg, sheet drawer with accordion sections
- SplitHero stacks image below text on < md
- Card grids: 3 → 2 → 1 column

## Out of scope
- Real backend/forms, auth flows, verbatim marketing copy (paraphrased), exact pixel-perfect imagery for non-hero sections.

## Files touched (expected)
- `src/data/site.ts` (local image imports)
- `src/components/site/Header.tsx` (featured card markup)
- New `src/components/site/SplitHero.tsx`
- All route files listed in §2
- New `src/assets/nav/*.webp`
