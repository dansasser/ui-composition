# Typography Animation

## The Decision
When and how to animate text.

## The Spectrum

**Static text (most text on the page):**
Body copy, navigation labels, form labels, descriptions, legal text. Never animated. Readable, accessible, instant.

**Entrance-animated text (headlines, hero text, section titles):**
Text that benefits from a reveal moment. The user should experience the text "arriving," not just being there. Appropriate for: hero headlines, section headers on scroll, feature titles.

**Continuously animated text (very rare, very intentional):**
Gradient color cycling, rotating word carousels, typewriter effects. These demand attention constantly. Use sparingly — one per page maximum. Usually only in the hero or a standout CTA.

## Patterns from the Index

Search: `mcp doc-index search db=ui-components query="text animation"`

**Word/letter reveal:**
- react-bits BlurText — blur-in per word or letter with stagger
- react-bits SplitText — GSAP-powered split reveal
- Magic UI text-animate — motion-based text entrance
- Magic UI blur-fade — combined blur + fade

**Gradient/color effects:**
- react-bits GradientText — animated gradient flow
- react-bits ShinyText — shimmer highlight pass
- Magic UI animated-gradient-text — gradient text with motion
- Magic UI aurora-text — aurora color effect on text

**Rotating/cycling text:**
- react-bits RotatingText — cycles through string array
- Magic UI word-rotate — motion-based word rotation
- Magic UI morphing-text — morphs between strings
- Magic UI flip-text — flip animation between words

**Typewriter/decode:**
- react-bits DecryptedText — scramble-then-reveal
- Magic UI typing-animation — classic typewriter
- Magic UI hyper-text — scramble on hover
- react-bits FuzzyText — fuzzy/static text effect

## Selection Guide

| Need | Best fit | Why |
|------|----------|-----|
| Hero headline reveal | BlurText or SplitText | One-time entrance, high impact |
| Rotating tagline | RotatingText or word-rotate | Cycles through value props |
| Stat label effect | GradientText or ShinyText | Draws eye to metrics |
| Interactive surprise | DecryptedText or hyper-text | Rewards hover/scroll |
| Loading/placeholder | typing-animation | Communicates "working" |

## Accessibility

Animated text must remain readable:
- Never animate faster than the user can read
- Rotating text needs enough time per word (minimum 2 seconds per rotation)
- Reduced-motion users see static text — the final state, not blank
- Screen readers get the full text content regardless of animation state
- Don't animate text that conveys critical information (error messages, form validation, warnings)
