# Responsive Animation

## The Decision
How should animation behavior change across screen sizes?

## The Principle
Animation that works on a 27" monitor can be overwhelming on a phone, and animation designed for mobile can feel underwhelming on desktop. Adapt — don't apply uniformly.

## Desktop → Mobile Adjustments

**Reduce distance and scale on mobile:**
A card that slides 60px on desktop should slide 20-30px on mobile. Scale effects should be subtler (1.05 → 1.02).
```css
.card-entrance {
  --slide-distance: 60px;
}
@media (max-width: 768px) {
  .card-entrance {
    --slide-distance: 24px;
  }
}
```

**Disable heavy effects on mobile:**
WebGL backgrounds (three.js, ogl), complex particle systems, and continuous canvas animations eat battery and may lag. Replace with static fallbacks.
```css
@media (max-width: 768px) {
  .webgl-background { display: none; }
  .static-gradient-fallback { display: block; }
}
```

See: `performance-motion-budget.md` for mobile performance guidance.

**Simplify hover on touch:**
See: `hover-interaction-patterns.md` — use `@media (hover: hover)` to scope hover effects. On touch devices, `:active` replaces hover.

**Parallax on mobile:**
Disable it. Parallax on small screens feels claustrophobic and performs poorly. Show a static image instead.

## Mobile-Specific Patterns

**Pull-to-refresh indicator:**
Native on many mobile browsers, but custom implementations need a spring-like pull indicator.

**Bottom sheet (mobile modals):**
On mobile, modals should be bottom sheets, not center modals. They're easier to reach with a thumb.
See: `modal-drawer-transitions.md`

**Swipe gestures:**
Carousels, dismissible cards, navigation. These are mobile-native interactions that desktop doesn't have. Use `@use-gesture/react` or CSS scroll-snap.

## Column Collapse

When a 3-column grid becomes a single column on mobile:
- Don't stagger 3 items side-by-side (they're now stacked vertically)
- Stagger the vertical stack instead, but reduce stagger delay (80ms → 50ms, since vertical scroll is faster than horizontal scanning)

## Testing

Use Chrome DevTools device emulation + CPU throttling (4x) to simulate real mobile conditions. What runs smoothly at 60fps on a MacBook may drop to 30fps on a mid-range phone with 4x throttling.
