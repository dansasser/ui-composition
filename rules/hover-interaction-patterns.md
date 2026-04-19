# Hover Interaction Patterns

## The Decision
What should happen when a user hovers over an element?

## Context Signals

**Interactive elements (buttons, cards, links, nav items):**
Hover states are EXPECTED. A clickable element with no hover feedback feels broken. At minimum: cursor change + subtle visual shift.

**Static content (headings, paragraphs, images in content):**
No hover state needed. Adding hover effects to non-interactive elements is misleading — it implies clickability.

**Cards that link somewhere:**
The entire card should respond to hover, not just the link text inside it. This means the hover effect goes on the card container, not the child.

## Patterns by Intensity

**Subtle (most common, safest default):**
```css
.card { transition: transform 0.2s ease, box-shadow 0.2s ease; }
.card:hover { transform: translateY(-2px); box-shadow: 0 8px 25px rgba(0,0,0,0.1); }
```

**Medium (for feature highlights, portfolio items):**
```css
.card:hover { transform: translateY(-4px) scale(1.02); box-shadow: 0 12px 40px rgba(0,0,0,0.15); }
.card:hover img { transform: scale(1.05); }
```

**Bold (for hero elements, CTAs, showcase items):**
Search index: `mcp doc-index search db=ui-components query="hover effect glare tilt 3d"`
Candidates: react-bits GlareHover (glossy sweep), DaisyUI hover3d, Magic UI magic-card (spotlight effect).

**Magnetic (cursor-aware):**
Search index: `mcp doc-index search db=ui-components query="magnet hover cursor attraction"`
Candidate: react-bits Magnet — element pulls toward cursor within a radius.

## Timing

- Hover-in: 150-200ms (fast enough to feel responsive)
- Hover-out: 200-300ms (slightly slower than in — feels more natural)
- Never exceed 400ms for hover transitions — feels laggy

## Touch Devices

Hover doesn't exist on touch. Use `@media (hover: hover)` to scope hover effects:
```css
@media (hover: hover) {
  .card:hover { transform: translateY(-2px); }
}
```
On touch devices, active/tap states replace hover. Use `:active` for tap feedback.

## Performance

- `transform` and `opacity` are GPU-composited — use these for hover effects
- Avoid animating `width`, `height`, `margin`, `padding`, `border-width` on hover — these cause layout reflow
- `box-shadow` transitions are expensive at large sizes — keep shadow spread reasonable
