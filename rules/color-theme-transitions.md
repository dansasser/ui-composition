# Color & Theme Transitions

## The Decision
How to handle transitions between color states: dark/light mode, theme changes, state-dependent colors.

## Dark Mode Toggle

When the user toggles dark/light mode, ALL color properties should transition smoothly — not snap.

```css
:root {
  --bg: #ffffff;
  --text: #111827;
  --surface: #f9fafb;
}
.dark {
  --bg: #0f172a;
  --text: #f1f5f9;
  --surface: #1e293b;
}
/* Transition goes on elements that CONSUME the variables, not on :root itself.
   CSS custom property values don't transition — only the properties using them do. */
body, .card, .surface {
  background-color: var(--bg);
  color: var(--text);
  transition: background-color 0.3s ease, color 0.3s ease;
}
```

Apply the transition to body and major surface elements. Don't transition every single element — just the ones the user perceives (background, cards, text).

Duration: 300ms. Fast enough to feel responsive, slow enough to see the change.

## State-Dependent Colors

**Error states:** Input border transitions from gray → red. Duration: 150ms. Fast.
**Success states:** Green confirmation. Same speed.
**Active/selected:** Background fill transition on tab selection, toggle states. Duration: 150ms.
**Focus ring:** Color transition on focus. Duration: 0ms-100ms (accessibility requires visible focus, don't make it slow).

See: `form-feedback-animation.md` for validation-specific patterns.

## Gradient Animations

Animated gradients for backgrounds, text, or decorative elements.
See: `modern-css-first.md` for the `@property` technique.

For text gradients:
Search: `mcp doc-index search db=ui-components query="gradient text animated color"`
Candidates: react-bits GradientText, Magic UI animated-gradient-text.

## Hover Color Changes

Button hover color transitions:
```css
.btn {
  background-color: var(--primary);
  transition: background-color 0.15s ease;
}
.btn:hover {
  background-color: var(--primary-dark);
}
```

Keep color transitions fast (100-200ms). Slow color changes feel laggy because the user's eye is very sensitive to color shifts.

## Accessibility

- Color transitions must maintain contrast ratios at ALL intermediate states
- During a dark mode transition, there should be no point where text becomes unreadable against the background
- Use `transition` not `animation` for theme changes — transitions can be interrupted if the user toggles again quickly
