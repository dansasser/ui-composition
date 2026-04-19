# CTA & Button Treatment

## The Decision
How should buttons and calls-to-action behave and draw attention?

## Button Tiers

Not all buttons are equal. Treatment scales with importance:

**Primary CTA (one per page section):**
The thing you want the user to click. "Book a Call," "Start Free," "Get Started."
- Solid fill, high-contrast color
- Hover: transform + shadow lift + optional color shift. See: `hover-interaction-patterns.md`
- Consider: animated border (shimmer, glow, gradient rotation)
- Search: `mcp doc-index search db=ui-components query="animated button shimmer glow border"`
- Candidates: Magic UI shimmer-button, shiny-button, pulsating-button, rainbow-button. react-bits StarBorder, ElectricBorder.

**Secondary buttons:**
Supporting actions. "Learn More," "View Details."
- Outlined or ghost style
- Hover: fill transition (outline → solid) or subtle background change
- No animated borders — that's reserved for primary CTA

**Tertiary/text buttons:**
"Cancel," "Skip," "Back."
- Text-only, no border
- Hover: underline or subtle color change
- Minimal animation — these should be quiet

## Attention Patterns

**Pulsing glow (for hero CTAs):**
A subtle breathing glow around the button draws the eye without being obnoxious.
```css
.cta-glow {
  box-shadow: 0 0 20px rgba(59, 130, 246, 0.3);
  animation: pulse-glow 2s ease-in-out infinite;
}
@keyframes pulse-glow {
  0%, 100% { box-shadow: 0 0 20px rgba(59, 130, 246, 0.3); }
  50% { box-shadow: 0 0 30px rgba(59, 130, 246, 0.5); }
}
```
Use ONCE per page. Multiple pulsing elements compete for attention and cancel each other out.

**Shimmer sweep:**
A diagonal light sweep passes across the button periodically. Subtle, premium feel.
Search index for shimmer-button or ShinyText applied to button labels.

**Arrow animation on hover:**
CTA text "Get Started →" — the arrow slides right on hover. Simple, effective.
```css
.cta-arrow { transition: transform 0.2s; }
.cta:hover .cta-arrow { transform: translateX(4px); }
```

## Click Feedback

Every button needs click feedback:
- Scale down to 0.97-0.98 on `:active` (feels like a physical press)
- Duration: instant (50ms or less)
```css
.btn:active { transform: scale(0.97); }
```

For richer feedback:
Search: `mcp doc-index search db=ui-components query="click spark confetti ripple feedback"`
Candidates: react-bits ClickSpark (particle burst on click), Magic UI cool-mode (emoji burst), Magic UI confetti.

## Context

- Marketing page CTAs: earn bold treatment (glow, shimmer, animation)
- App UI buttons: subtle, fast, functional. No glow. Just clean state transitions.
- Form submit buttons: see `form-feedback-animation.md`
- Destructive buttons (Delete, Remove): red, no animation that encourages clicking. Simple hover darken.
