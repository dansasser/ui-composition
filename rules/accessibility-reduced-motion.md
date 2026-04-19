# Accessibility & Reduced Motion

## The Decision
How to handle animation accessibility, especially for users with motion sensitivity.

## prefers-reduced-motion

This is non-negotiable. Every animated component must respect this media query.

**CSS approach:**
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

**Per-component approach (more granular):**
```css
.hero-entrance {
  animation: slide-up 0.4s ease-out;
}
@media (prefers-reduced-motion: reduce) {
  .hero-entrance {
    animation: none;
    opacity: 1; /* ensure element is visible without animation */
  }
}
```

**React/Motion approach:**
```jsx
import { useReducedMotion } from 'motion/react';

function AnimatedCard({ children }) {
  const shouldReduce = useReducedMotion();
  return (
    <motion.div
      initial={shouldReduce ? false : { opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={shouldReduce ? { duration: 0 } : { duration: 0.4 }}
    >
      {children}
    </motion.div>
  );
}
```

## What Reduced Motion Means

Reduced motion does NOT mean no visual changes. It means:
- No parallax or scroll-jacking
- No auto-playing animations or videos
- No flashing or rapid state changes
- Transitions are instant or very fast (under 100ms)
- Content is still fully visible and functional

Acceptable under reduced motion:
- Opacity changes (fade without movement)
- Color transitions
- Instant state changes (element appears, just without the slide/bounce)

## Focus Management

Animated modals, drawers, and dialogs must manage focus:
- When a modal opens: focus moves to the first focusable element inside
- When it closes: focus returns to the element that triggered it
- Focus trap: Tab/Shift+Tab stays within the modal while open
- Escape key closes the modal

shadcn components and Radix UI handle this automatically. If using HTML+Tailwind components (HyperUI, Flowbite), you need to add focus management manually or use the library's JS plugin.

## Color Contrast

Not directly animation-related, but critical:
- Text on animated backgrounds must maintain 4.5:1 contrast ratio at ALL points in the animation
- If a gradient shifts colors behind text, test the worst-case contrast
- Animated overlays on images need a solid/blurred backdrop behind text
