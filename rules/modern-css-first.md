# Modern CSS First

## The Decision
Should this effect be CSS or JavaScript?

## The Principle
If CSS can achieve the effect, prefer CSS. Zero bundle cost, no hydration needed, works in every framework, renders on the server.

## CSS Capabilities (2025-2026)

**Scroll-driven animations (no IntersectionObserver needed):**
```css
.reveal {
  animation: slide-in linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 100%;
}
@keyframes slide-in {
  from { opacity: 0; transform: translateY(40px); }
  to { opacity: 1; transform: translateY(0); }
}
```

**Entry animations on new elements:**
```css
.card {
  @starting-style { opacity: 0; transform: scale(0.95); }
  transition: opacity 0.3s, transform 0.3s;
}
```

**Animated gradients via @property:**
```css
@property --gradient-angle {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}
.gradient-bg {
  --gradient-angle: 0deg;
  background: conic-gradient(from var(--gradient-angle), #e81cff, #40c9ff, #e81cff);
  animation: spin 3s linear infinite;
}
@keyframes spin { to { --gradient-angle: 360deg; } }
```

**Container queries (component-level responsive):**
```css
.card-container { container-type: inline-size; }
@container (min-width: 400px) {
  .card { flex-direction: row; }
}
```

**:has() for parent-aware styling:**
```css
/* Style the form when it contains an invalid input */
form:has(:invalid) { border-color: red; }
/* Style a card differently when it has an image */
.card:has(img) { grid-template-rows: 200px 1fr; }
```

**Scroll-snap for carousels:**
```css
.carousel { scroll-snap-type: x mandatory; overflow-x: auto; }
.carousel-item { scroll-snap-align: start; }
```

**View Transitions API (page transitions):**
```css
::view-transition-old(root) { animation: fade-out 0.2s; }
::view-transition-new(root) { animation: fade-in 0.2s; }
```

## When JavaScript IS needed

- **Physics-based springs** — CSS doesn't do spring dynamics. Use `motion` or react-spring.
- **Gesture tracking** — drag, pinch, swipe. Need `@use-gesture/react` or similar.
- **Complex orchestration** — 10 elements with interdependent timing, callbacks on completion, conditional sequences. CSS animations fire and forget.
- **3D/WebGL** — three.js, ogl, react-three-fiber. CSS 3D transforms are limited.
- **Dynamic values** — animation target depends on runtime data (scroll position → element position → cursor distance). CSS needs fixed keyframes.

## The Check

Before adding a JS animation library for an effect, ask:
1. Can `animation-timeline: view()` do this? (scroll-triggered)
2. Can `@starting-style` do this? (entry animation)
3. Can `transition` + a class toggle do this? (state change)
4. Can `@keyframes` do this? (repeating/looping)

If yes to any → CSS. If no to all → JavaScript, and search the index for the right component.
