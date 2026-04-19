# Scroll-Triggered Animation

## The Decision
Should this element animate on scroll, and if so, how?

## Context Signals

**Animate on scroll when:**
- The element is below the fold and needs to "arrive" as the user reaches it
- The section contains a selling metric (counter, stat, percentage) that gains impact from counting up
- A row of items benefits from staggered entrance (3-6 items, not 40)
- The content creates a narrative flow that scroll progression reinforces

**Skip scroll animation when:**
- The element is above the fold (it should already be visible on load)
- The section is dense content the user is reading (blog text, docs, terms)
- There are too many items to stagger without the animation becoming tedious (>8 items)
- The page already has 4+ scroll-triggered sections — animation fatigue sets in

## Implementation Patterns

**CSS-first (preferred when possible):**
```css
/* Modern: scroll-driven animation timeline */
.reveal {
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 100%;
}
@keyframes reveal {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}
```

**IntersectionObserver (vanilla JS):**
When CSS `animation-timeline` isn't sufficient (need JS callbacks, class toggling, or counter start triggers).

**React components from index:**
Search: `mcp doc-index search db=ui-components query="scroll triggered reveal animation"`
Candidates: react-bits ScrollFloat, ScrollReveal, FadeContent, AnimatedContent. Magic UI blur-fade, text-reveal.

**Astro inline script pattern:**
```html
<script is:inline>
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); });
}, { threshold: 0.1 });
document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
</script>
```

## Stagger Timing

When multiple items enter in sequence:
- Base delay between items: 80-150ms (faster = snappier, slower = more dramatic)
- Total sequence duration should not exceed 800ms for a group
- 3 items at 120ms stagger = 360ms total (good)
- 8 items at 120ms stagger = 960ms total (too long — reduce to 60ms or animate in groups)

## Accessibility

Always respect `prefers-reduced-motion`:
```css
@media (prefers-reduced-motion: reduce) {
  .reveal { animation: none; opacity: 1; transform: none; }
}
```
