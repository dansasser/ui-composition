# Carousel & Slider Patterns

## The Decision
When to use a carousel, what type, and how to implement transitions.

## When to Use a Carousel

**Use when:**
- Showing a curated set of items (3-10) where each deserves individual focus (testimonials, portfolio pieces, featured products)
- Space is constrained and multiple items need to share the same area
- The content is browseable (user explores at their own pace)

**Don't use when:**
- All items should be visible at once (feature grid > carousel for feature comparison)
- Content is critical and shouldn't be hidden behind navigation (burying important info in a carousel means most users never see it)
- There are 20+ items (use a paginated grid instead)

## Types

**CSS scroll-snap (preferred for simple cases):**
```css
.carousel {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
}
.carousel::-webkit-scrollbar { display: none; }
.carousel-item {
  flex: 0 0 100%; /* or 33.3% for 3-per-view */
  scroll-snap-align: start;
}
```
Pros: Zero JS, native scroll behavior, works on touch. See: `modern-css-first.md`
Cons: No autoplay, limited control over indicators, no custom transition effects.

**React component carousels:**
Search: `mcp doc-index search db=ui-components query="carousel slider"`
Candidates: react-bits Carousel, Flowbite carousel, shadcn Carousel, react-bits ElasticSlider.

**Fancy/3D carousels:**
Search: `mcp doc-index search db=ui-components query="circular gallery 3d carousel"`
Candidates: react-bits CircularGallery, react-bits DomeGallery, react-bits CardSwap.

## Autoplay

**If autoplaying:**
- Pause on hover (desktop) and on touch (mobile)
- Provide a visible pause button
- Speed: 4-6 seconds per slide (enough to read)
- Stop autoplaying after user manually navigates (they've taken control)

**Default recommendation:** Don't autoplay. Let the user navigate. Autoplay is appropriate for hero image banners and nowhere else.

## Navigation Controls

- **Arrows:** Left/right buttons, visible on hover (desktop), always visible (mobile)
- **Dots/indicators:** Show current position in the set. Clicking a dot jumps to that slide.
- **Keyboard:** Arrow keys should navigate. Focus management matters.
- **Swipe:** Touch swipe on mobile. Use CSS scroll-snap (free) or `@use-gesture/react` for custom behavior.

## Transition Styles

**Slide (most common):** translateX between items. Duration: 300ms, ease-in-out.
**Fade:** Crossfade between items. Duration: 400ms. Good for image galleries where content size varies.
**Scale:** Current item scales down as next scales up. Dramatic, use for hero/featured items only.

## Accessibility

- Carousel region: `role="region"` + `aria-label="Testimonials carousel"`
- Each slide: `role="group"` + `aria-roledescription="slide"` + `aria-label="Slide 1 of 5"`
- Navigation: `aria-label="Previous slide"` / `aria-label="Next slide"`
- Autoplay: must have a pause mechanism
- See: `accessibility-reduced-motion.md` — with reduced motion, slides should snap instantly (no transition)
