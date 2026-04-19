# Performance & Motion Budget

## The Decision
How much animation can this page afford?

## The Budget

**Bundle size:**
- Total animation-related JS: target under 40KB gzipped
- motion (Framer Motion): ~25KB — if you use this, it's most of your budget
- gsap: ~30KB — if you use this instead of motion, similar cost
- CSS animations: 0KB — always free
- Animate.css: ~4KB — negligible
- If you need BOTH motion AND gsap on one page, you've blown the budget. Pick one.

**Runtime performance:**
- Animate only `transform` and `opacity` — these are GPU-composited, don't trigger layout
- Avoid animating: `width`, `height`, `top`, `left`, `margin`, `padding`, `border-width`, `font-size` — these cause layout reflow
- `box-shadow` is expensive to animate at large sizes — keep blur radius under 40px during transitions
- `filter: blur()` is GPU-composited but expensive on large elements — avoid blurring full-screen backgrounds continuously

**Concurrent animations:**
- 1-3 elements animating simultaneously: fine on any device
- 4-8 elements: test on mobile, may cause frame drops
- 10+ elements: likely janky on mid-range phones. Stagger instead of simultaneous.
- Infinite/looping animations: keep to 1-2 per viewport. More creates visual noise and battery drain.

## Mobile Considerations

What runs smoothly on a 2026 MacBook Pro may stutter on a 2023 Android phone. Consider:
- Reduce or disable parallax on mobile (`@media (max-width: 768px)`)
- Replace spring animations with simpler ease curves on mobile (fewer frames to compute)
- Disable WebGL backgrounds (three.js, ogl) on mobile — use a static gradient fallback
- Test with Chrome DevTools CPU throttling (4x slowdown)

## Lazy Loading

**Images:**
```html
<img loading="lazy" decoding="async" src="..." alt="..." />
```
Every image below the fold should lazy load. No exceptions.

**Heavy components:**
In Astro, `client:visible` is lazy loading for JS components. The component's JavaScript doesn't download until the element scrolls into view.

**Fonts:**
```css
@font-face {
  font-family: 'Display';
  src: url('/fonts/display.woff2') format('woff2');
  font-display: swap; /* show fallback immediately, swap when loaded */
}
```

## The Check

Before shipping a page with animation:
1. Open DevTools Performance tab, record a scroll-through
2. Check for frame drops (red bars in the timeline)
3. If frames drop below 30fps on desktop → too much animation, cut something
4. Test with CPU throttling 4x → simulates mobile
5. Check `prefers-reduced-motion` fallback — page must be fully functional without animation
