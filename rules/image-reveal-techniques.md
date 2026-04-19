# Image Reveal Techniques

## The Decision
How should images appear on the page?

## The Principle
Images are heavy visual elements. Their appearance should feel intentional — either instantly present (above fold) or revealed with purpose (below fold).

## Above the Fold

Images above the fold should be VISIBLE on first paint. No lazy loading, no entrance animation. Priority load them:
```html
<img src="hero.jpg" alt="..." fetchpriority="high" decoding="async" />
```

## Below the Fold — Lazy Loading

Every image below the fold lazy loads:
```html
<img src="photo.jpg" alt="..." loading="lazy" decoding="async" />
```

## Reveal Patterns

**Fade in (simplest, most common):**
Image fades from opacity 0 → 1 on scroll. Works for any image.
```css
img.reveal {
  opacity: 0;
  transition: opacity 0.4s ease-out;
}
img.reveal.visible { opacity: 1; }
```

**Fade + scale (subtle zoom-in):**
Image scales from 1.05 → 1.0 while fading in. Creates a "coming into focus" effect.
```css
img.reveal {
  opacity: 0;
  transform: scale(1.05);
  transition: opacity 0.4s ease-out, transform 0.4s ease-out;
}
img.reveal.visible {
  opacity: 1;
  transform: scale(1);
}
```

**Clip-path reveal (dramatic, editorial):**
Image reveals via expanding clip-path — like a curtain opening.
```css
img.reveal {
  clip-path: inset(0 50% 0 50%);
  transition: clip-path 0.6s ease-out;
}
img.reveal.visible {
  clip-path: inset(0 0 0 0);
}
```

**Blur-to-sharp (progressive loading feel):**
Start blurred, sharpen on load. Works well with image placeholders.
```css
img.reveal {
  filter: blur(10px);
  transition: filter 0.4s ease-out;
}
img.reveal.visible { filter: blur(0); }
```

**Ken Burns (slow zoom on static images):**
Continuous subtle zoom creates movement without interaction. Used in hero backgrounds, testimonial sections.
```css
.ken-burns {
  animation: ken-burns 20s ease-in-out infinite alternate;
}
@keyframes ken-burns {
  from { transform: scale(1); }
  to { transform: scale(1.08); }
}
```

## Gallery/Grid Images

For image grids (portfolio, product gallery):
- Stagger the reveals — each image enters 50-100ms after the previous
- See: `scroll-triggered-animation.md` for stagger timing
- Keep stagger fast — 8 images at 100ms = 800ms total, which is the max before it drags
- Hover: subtle scale 1.02-1.05 + shadow lift. See: `hover-interaction-patterns.md`

## Placeholder Strategies

While images load, show:
- **Dominant color placeholder:** Extract the dominant color and show a solid block. Low effort, effective.
- **Blur-up (LQIP):** Show a 20px wide version, scale it up blurred, crossfade to full image on load.
- **Skeleton:** Gray pulsing rectangle matching the image dimensions. See: `loading-skeleton-states.md`
