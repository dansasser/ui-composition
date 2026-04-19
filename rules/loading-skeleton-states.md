# Loading & Skeleton States

## The Decision
How to handle loading states for async content.

## The Principle
Never show a blank space where content will appear. The user should always see SOMETHING — either the content or a representation of its shape.

## Patterns

**Skeleton screens (preferred for content areas):**
Show the layout shape with pulsing placeholder blocks where text, images, and cards will load. This sets expectations about what's coming.

Search: `mcp doc-index search db=ui-components query="skeleton loading placeholder"`
Candidates: shadcn Skeleton, DaisyUI skeleton, Flowbite skeleton.

```css
.skeleton {
  background: linear-gradient(90deg, #e0e0e0 25%, #f0f0f0 50%, #e0e0e0 75%);
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
  border-radius: 4px;
}
@keyframes shimmer {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

**Spinners (for discrete actions):**
Button clicks, form submissions, API calls. The spinner replaces or overlays the trigger element.
- Use inside the button, not as a page-level overlay
- Disable the button while loading to prevent double-clicks
- Spinner size matches the element it's replacing

Search: `mcp doc-index search db=ui-components query="spinner loading indicator"`
Candidates: DaisyUI loading, Flowbite spinner, shadcn spinner.

**Progress bars (for long operations):**
File uploads, multi-step processes, known-duration tasks.
- Use linear easing (constant fill rate communicates predictable progress). See: `animation-timing-easing.md`
- Width transition on the fill bar, duration matches the update interval
- Search: `mcp doc-index search db=ui-components query="progress bar animated"`
- Candidates: DaisyUI progress, shadcn Progress, Flowbite progress.

**Empty states (when loading completes with no data):**
Not a loading pattern, but the transition from loading → empty needs handling.
See: `empty-state-treatment.md`

## Timing

- Show skeleton immediately (0ms delay) — don't wait to see if content loads fast
- If content loads in under 200ms, the skeleton may flash — use a minimum display time of 300ms to prevent jarring flicker
- Spinners on buttons: show after 150ms delay (if the action completes faster, no spinner shown — avoids flash)

## Transition from Loading to Content

Don't just swap skeleton for content. Fade the content in:
```css
.content-loaded {
  animation: fade-in 0.2s ease-out;
}
@keyframes fade-in {
  from { opacity: 0; }
  to { opacity: 1; }
}
```

The skeleton itself doesn't need an exit animation — the content replacing it is enough.
