# Plan: Replicate aplusfcu.org Navigation + Pages

## Scope

Match the real site's main-nav behavior and section pages as closely as possible, with the navbar dropdown as the highest-priority fix. All six main-nav sections (Accounts, Loans, Services, Business, Who We Are, Guidance) plus Contact Us will be reviewed and aligned.

> Note: I'll replicate layout, structure, spacing, typography, colors, and component patterns. I won't reproduce long-form proprietary copy verbatim — body text will paraphrase the original while preserving structure and intent. Public marketing imagery and icon URLs already used in the project will be reused.

---

## 1. Reference capture (first step)

Before editing, I'll fetch the live site to ground every change:

- Screenshot + scrape `https://aplusfcu.org/` (desktop + mobile widths) for the navbar in every state (closed, hover-open per item, mobile drawer).
- Fetch each landing page: `/accounts/`, `/loans/`, `/services/`, `/business/`, `/who-we-are/`, `/guidance/`, `/contact-us/` and key children (checking, savings, certificates, youth, home-loans, vehicle-loans, etc.).
- Extract: exact dropdown column structure, featured promo card on the right, link groupings, hover/transition timing, breakpoints, font stack, color tokens, header height, utility-bar layout.

## 2. Navbar rebuild (priority)

Current `Header.tsx` uses a small 2-column hover popover. The real aplusfcu.org dropdown is a full-width "mega menu" with: a left grid of grouped link columns + a right-side featured promo (image + headline + CTA) + a bottom CTA strip on some menus.

Changes:
- Rewrite `src/components/site/Header.tsx` mega-menu panel:
  - Full-bleed white panel below header, anchored to viewport width (not centered popover).
  - Multi-column grid for link groups; each group has a bold heading + underlined link list.
  - Right-rail "Featured" card slot per nav item (image, eyebrow, title, link).
  - Hover-intent: small open delay (~120ms) and close delay (~200ms) to prevent flicker; ESC and outside-click close.
  - Smooth fade/slide transition; brand-green underline indicator on active top-level item.
  - Keyboard a11y: arrow keys between top items, Enter to open, Tab through links, focus trap while open.
- Extend `src/data/site.ts` `NAV` shape to include `featured: { image, eyebrow, title, href, cta }` per item and `cta` strip rows; populate from the real site.
- Utility bar: tighten spacing, color, and divider to match reference.
- Mobile drawer: full-screen, slide-in panel, accordion groups with the brand-green chevrons, sticky header bar with close + login pill, exactly like reference.

## 3. Page-by-page audit & alignment

For each route below, compare against the live page and adjust hero, section order, card grids, color usage, and spacing. No business-logic changes — presentation only.

- `/` index — verify hero carousel arrows/dots, rates strip, content blocks alternation, awards grid spacing, articles row.
- `/accounts` and children `/checking`, `/savings`, `/certificates`, `/youth` — category grid, comparison table styling, benefits band.
- `/loans` and children `/home-loans`, `/vehicle-loans`, `/personal-credit`, `/student-loans` — rate cards, calculator CTA, benefits.
- `/services` and children `/online-banking`, `/mobile-banking`, `/member-services`, `/insurance-investments` — feature lists, app screenshots.
- `/business` and children `/accounts`, `/lending`, `/services` — business hero treatment, 3-up cards.
- `/who-we-are` and children `/about`, `/community`, `/membership`.
- `/guidance` and children `/financial-education`, `/rates`, `/tools`.
- New route `src/routes/contact-us.tsx` — locations, phone numbers, hours, contact form layout matching reference.

## 4. Design tokens (`src/styles.css`)

Confirm and tune semantic tokens to match reference: `brand-green`, `brand-green-dark`, `brand-cream`, `ink`, `border`, plus a serif display token (current site uses a transitional serif for headlines and a humanist sans for body). Add tokens only where missing — no inline color hexes in components.

## 5. Responsive QA

After each batch of edits, screenshot at 1440, 1024, 768, 390 in the browser tool and compare to the reference at the same widths. Adjust breakpoints to match where the real site collapses to the mobile drawer (~1024px).

---

## Technical Details

- `NAV` type extended; touch only `src/data/site.ts` and `src/components/site/Header.tsx` for nav structure.
- Mega-menu uses Radix `NavigationMenu` primitive (already in `src/components/ui/navigation-menu.tsx`) for a11y + hover-intent, styled to match reference rather than the current custom popover.
- Animations via Tailwind transitions + `data-[state=open]` from Radix.
- Mobile drawer stays in `Header.tsx` but moved to a Radix `Sheet` for focus-trap and swipe support.
- New images for "Featured" rail are hot-linked from the same `aplusfcu.org/wp-content/uploads/global/img/...` paths already used elsewhere in the project.
- Contact-Us route: new file `src/routes/contact-us.tsx` with `head()` meta; no other route files added/removed.

## Out of scope

- Real backend forms (contact form will post to a stub server fn returning success).
- Authenticated member portal / online-banking login flows beyond the existing visual block.
- Verbatim copy of long marketing paragraphs — paraphrased to preserve meaning.

## Suggested execution order

1. Reference capture (live screenshots + scrape).
2. Navbar rebuild (desktop mega menu + mobile drawer) — single largest visible win.
3. Token/typography pass in `styles.css`.
4. Page audit batches: Accounts → Loans → Services → Business → Who We Are → Guidance → Contact Us.
5. Responsive QA pass at 4 widths and fix regressions.
