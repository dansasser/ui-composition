# Toast & Notification Patterns

## The Decision
How to show transient feedback messages (success, error, info, warning).

## The Principle
Toasts are interruptions. They need to communicate quickly and get out of the way. The animation should draw attention without demanding it.

## Placement

**Top-right (desktop default):** Non-blocking, visible, dismissable. Most common.
**Top-center:** For critical alerts that need immediate attention.
**Bottom-right:** Less intrusive. Good for background process updates.
**Bottom-center (mobile):** Thumb-reachable, full-width.

Stick to ONE position per app. Toasts appearing from multiple directions is chaotic.

## Entry/Exit

**Entry:** Slide in from the edge + fade. Duration: 200ms, ease-out.
```css
@keyframes toast-in {
  from { transform: translateX(100%); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}
```

**Exit:** Fade out + slide. Duration: 150ms, ease-in. Faster exit than entry.

**Auto-dismiss:** 3-5 seconds for info/success. Error toasts should NOT auto-dismiss — the user needs time to read them. Let the user dismiss manually.

## Stacking

When multiple toasts appear:
- Stack vertically with 8px gap
- New toast appears at top, pushes others down
- Animate the position change (200ms translateY transition)
- Cap at 3-5 visible toasts. Beyond that, collapse into "X more notifications"

## Components

Search: `mcp doc-index search db=ui-components query="toast notification snackbar"`
Candidates: shadcn Sonner (React, the gold standard for toasts), DaisyUI toast (CSS), Flowbite toast.

Sonner is the de facto React toast library. If the project uses React, use Sonner. If HTML+Tailwind only, use DaisyUI toast classes.

## Types by Severity

| Type | Color | Icon | Auto-dismiss | Duration |
|------|-------|------|-------------|----------|
| Success | Green | ✓ check | Yes | 3s |
| Info | Blue | ℹ info | Yes | 4s |
| Warning | Yellow/amber | ⚠ warning | Yes | 5s |
| Error | Red | ✕ error | NO | Manual |

## Accessibility

- Use `role="alert"` for error toasts (screen readers announce immediately)
- Use `role="status"` for info/success toasts (polite announcement)
- Ensure dismiss button is keyboard-accessible
- Don't rely on color alone — include an icon for each toast type
- See: `accessibility-reduced-motion.md`
