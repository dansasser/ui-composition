# Animation Timing & Easing

## The Decision
How fast should this animation be, and what curve should it follow?

## Duration Guidelines

**Micro-interactions (hover, click feedback, toggle):** 100-200ms
Fast enough to feel instant, slow enough to be visible.

**Entrances (fade-in, slide-in, scale-up):** 200-400ms
Perceptible but not slow. The user should feel the element "arrive," not wait for it.

**Exits (fade-out, slide-out, collapse):** 150-300ms
Slightly faster than entrances. Users want dismissed things to leave quickly.

**Page transitions:** 200-500ms
Enough to create continuity between views without blocking navigation.

**Complex sequences (staggered lists, orchestrated reveals):** 400-800ms total
Individual items can be 200ms each with stagger delays, but the full sequence shouldn't drag.

**Loading indicators:** 1000-2000ms loop
Deliberately slow — communicates "something is happening" without implying it'll be fast.

## Easing Functions

**ease-out (decelerate):**
Starts fast, ends slow. Use for: entrances, things arriving on screen.
```css
transition-timing-function: cubic-bezier(0, 0, 0.2, 1);
```
The element "lands" — it rushes in then settles.

**ease-in (accelerate):**
Starts slow, ends fast. Use for: exits, things leaving the screen.
```css
transition-timing-function: cubic-bezier(0.4, 0, 1, 1);
```
The element "launches" — it picks up speed as it leaves.

**ease-in-out:**
Slow start, fast middle, slow end. Use for: things that move from point A to point B within the viewport (repositioning, sliding between states).
```css
transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
```

**linear:**
Constant speed. Use for: progress bars, loading indicators, infinite loops, scroll-linked animations. NOT for UI interactions — linear motion feels robotic.

**Spring (JS only):**
Physics-based, no fixed duration. Use for: playful interactions, bouncy feedback, natural-feeling motion.
```jsx
// Framer Motion / motion
<motion.div animate={{ y: 0 }} transition={{ type: "spring", stiffness: 300, damping: 24 }} />
```

Spring presets by feel:
- **Snappy:** stiffness 400, damping 30 — quick settle, minimal overshoot
- **Bouncy:** stiffness 300, damping 15 — visible bounce, playful
- **Gentle:** stiffness 150, damping 20 — slow, elegant, luxury feel
- **Heavy:** stiffness 100, damping 30 — weighty, deliberate

## The Rule

Match easing to what the element is DOING:
- Arriving → ease-out (decelerate into place)
- Leaving → ease-in (accelerate out)
- Moving between positions → ease-in-out
- Continuous motion → linear
- Playful/interactive → spring

Don't use the same easing for everything. A page where every animation uses `ease-in-out` feels monotonous. Vary the curves to match the intent of each motion.
