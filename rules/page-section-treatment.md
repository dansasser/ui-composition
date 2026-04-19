# Page Section Treatment

## The Decision
How should each section of a page be treated in terms of animation, interactivity, and visual weight?

## Reading a Page Top to Bottom

A page is not a flat list of sections. It has rhythm — high-energy moments and resting points. The treatment follows the narrative arc:

**Hero / Opening:**
The first thing the user sees. This earns the most visual investment. Animated entrances, bold typography, background effects. The hero sets the tone — if it's static, the user assumes the whole page is static.

**Value proposition / Features:**
The "why you should care" section. Staggered card entrances work here. Each feature gets a moment. Interactive hover states reward exploration. But the animation serves comprehension — features should animate IN, not continuously animate.

**Social proof / Testimonials:**
Lower energy than the hero. Subtle fade-ins, maybe a carousel with smooth transitions. The content (the quote, the name, the logo) is the star, not the animation.

**Stats / Metrics:**
Counter animations shine here — numbers counting up on scroll. This is one of the few places where animation IS the content. A static "70%" is a fact. An animated "0 → 70%" is a story.

**Pricing:**
Clean, scannable, minimal animation. Hover states on the tier cards for comparison. Don't animate the prices — users need to read and compare quickly. The CTA button can be the one animated element (subtle pulse, glow, or scale on hover).

**CTA / Closing:**
Second-highest energy after the hero. This is the conversion moment. The CTA button needs to draw the eye — this is where bold hover effects, animated borders, or attention-grabbing treatments earn their keep.

**Footer:**
Zero animation. Static. Functional. Links, legal, contact info. Animating the footer is a red flag that you're over-animating.

## The Pattern

Energy follows an arc: **HIGH (hero) → MEDIUM (features) → LOW (testimonials) → MEDIUM (stats) → LOW (pricing) → HIGH (CTA) → ZERO (footer)**

Not every page follows this exact structure, but the principle holds: vary the intensity. Don't animate everything equally.

## One-Page Applications

Single-page apps (dashboards, tools) have different treatment:
- Animate STATE CHANGES (data loading, filter results updating, sidebar opening)
- Don't animate LAYOUT (the sidebar itself doesn't entrance-animate on every page load)
- Skeleton screens during loading (not spinners for content areas)
- Transitions between views/routes need to be fast (200ms) not dramatic
