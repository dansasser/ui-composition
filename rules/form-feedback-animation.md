# Form Feedback Animation

## The Decision
How to communicate form state changes (validation, submission, success, error) through motion.

## The Principle
Form animations are COMMUNICATIVE, not decorative. Every motion tells the user something: "this is wrong," "this worked," "we're processing." If the animation doesn't communicate, remove it.

## Validation Feedback

**Error — shake:**
A quick horizontal shake (2-3 oscillations, ±4px, 300ms total) communicates "nope" without being aggressive.
```css
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-4px); }
  75% { transform: translateX(4px); }
}
.input-error { animation: shake 0.3s ease-in-out; }
```

**Error — border/color transition:**
Border color transitions from neutral to red. Duration: 150ms. Pair with error message that fades in below the input.
```css
.input { border-color: #e5e7eb; transition: border-color 0.15s; }
.input.error { border-color: #ef4444; }
.error-message {
  animation: fade-slide-in 0.2s ease-out;
}
@keyframes fade-slide-in {
  from { opacity: 0; transform: translateY(-4px); }
  to { opacity: 1; transform: translateY(0); }
}
```

**Success — check/green:**
Input border goes green, optional checkmark icon fades in. Keep it subtle — the user wants to move to the next field, not celebrate each input.

## Submission States

**Button loading:**
Replace button text with spinner. Button stays the same size (prevent layout shift). Disable the button.
```css
.btn-loading {
  position: relative;
  color: transparent; /* hide text */
  pointer-events: none;
}
.btn-loading::after {
  content: '';
  position: absolute;
  width: 20px; height: 20px;
  border: 2px solid currentColor;
  border-top-color: transparent;
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}
```
See: `loading-skeleton-states.md` for timing guidance on when to show the spinner.

**Success state:**
Button briefly shows a checkmark or "Sent!" message, then returns to normal (if the form is reusable) or redirects.
Duration of success state: 1-2 seconds.

**Error state:**
Button shakes, returns to normal state. Error message appears above or below the form.
See shake animation above.

## Multi-Step Forms

**Step transitions:**
Slide current step out, slide next step in. Direction matters:
- Forward → slide left (current step exits left, new enters from right)
- Back → slide right (opposite direction)
- Duration: 250ms, ease-in-out

**Progress indicator:**
Animated width bar or stepped indicator. Width transitions smoothly between steps.
Search: `mcp doc-index search db=ui-components query="stepper progress steps form"`
Candidates: DaisyUI steps, Flowbite stepper, shadcn Stepper.

## Accessibility

- Error messages: use `aria-live="polite"` so screen readers announce them
- Don't rely solely on color (red border) for error — add an icon or text message
- Shake animation: respect `prefers-reduced-motion` — show static error state instead
- Button loading: aria-busy="true" and aria-label describing the loading state
