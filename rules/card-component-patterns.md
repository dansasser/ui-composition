# Card Component Patterns

## The Decision
How to select, compose, and animate card components.

## Card Types

**Feature cards (marketing pages):**
Highlight a capability, service, or selling point. Usually in rows of 3-4.
- Scroll-triggered entrance with stagger. See: `scroll-triggered-animation.md`
- Hover: lift + shadow. See: `hover-interaction-patterns.md`
- Search: `mcp doc-index search db=ui-components query="feature card grid"`

**Product cards (ecommerce):**
Display an item with image, title, price, CTA.
- Image hover: subtle zoom (1.05). See: `image-reveal-techniques.md`
- Quick-action buttons appear on hover (add to cart, wishlist)
- Search: `mcp doc-index search db=ui-components query="product card ecommerce"`
- Candidates: HyperUI product-cards, Flowbite card, Meraki cards.

**Testimonial cards:**
Quote, name, photo, company. Usually in a carousel or grid.
- Subtle entrance (fade only, no dramatic movement)
- Carousel auto-play with pause on hover
- Search: `mcp doc-index search db=ui-components query="testimonial card quote"`

**Profile/team cards:**
Person's photo, name, role, links.
- Hover: image overlay with social links or bio
- Search: `mcp doc-index search db=ui-components query="profile card team member avatar"`
- Candidates: HyperUI team-sections, Magic UI avatar-circles.

**Pricing cards:**
Tier name, price, features, CTA.
- Highlighted/recommended tier: visually elevated (scale 1.02, different border/shadow, "Popular" badge)
- Hover: subtle lift on all cards, bold CTA treatment on the recommended tier
- See: `cta-button-treatment.md` for the CTA inside pricing cards
- Search: `mcp doc-index search db=ui-components query="pricing card tier"`
- Candidates: HyperUI pricing, Flowbite pricing.

**Interactive/fancy cards:**
Tilt effects, glass morphism, decay effects, 3D hover. For portfolio/showcase pages.
- Search: `mcp doc-index search db=ui-components query="card tilt glass 3d hover effect"`
- Candidates: react-bits TiltedCard, DecayCard, FluidGlass, GlassSurface, SpotlightCard, PixelCard. Magic UI magic-card, neon-gradient-card.
- Use sparingly — these are attention-demanding. One section of fancy cards, not the whole page.

## Grid Layout

**Responsive columns:**
- Desktop: 3-4 columns
- Tablet: 2 columns
- Mobile: 1 column (stack)
- Gap: 24-32px desktop, 16-20px mobile
- Use CSS Grid with auto-fill for flexibility:
```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
}
```

**Masonry layout (for varied-height cards):**
Search: `mcp doc-index search db=ui-components query="masonry grid layout"`
Candidate: react-bits Masonry.

## Stagger Guidelines

See: `scroll-triggered-animation.md` for stagger timing.
- 3 cards: 120ms stagger (360ms total) — good
- 4 cards: 100ms stagger (400ms total) — good
- 6+ cards: 60-80ms stagger or reveal in groups of 3
- On mobile (single column): stagger still works but reduce delay to 50-80ms
