# Modal & Drawer Transitions

## The Decision
How should overlays (modals, drawers, sheets, dialogs) enter and exit?

## The Principle
Overlays communicate layer changes. The animation should convey "something is appearing in front of your current context" — not just popping into existence.

## Entry Patterns

**Modal (center of screen):**
- Scale from 0.95 → 1.0 + fade in. Duration: 200ms, ease-out.
- Backdrop darkens simultaneously (opacity 0 → 0.5).
- Don't slide modals in from edges — that's drawer behavior.

**Drawer (slides from edge):**
- Translate from off-screen → visible position. Duration: 250ms, ease-out.
- Backdrop darkens behind.
- Match the edge: left drawer slides from left, bottom sheet from bottom.
- See: `animation-timing-easing.md` for curve selection.

**Sheet (bottom sheet, common on mobile):**
- Translate up from bottom. Duration: 300ms, spring or ease-out.
- May support drag-to-dismiss (needs gesture handling — JS required).

**Popover/tooltip:**
- Scale from 0.9 → 1.0 + fade. Duration: 150ms. Fast — these are lightweight.
- Anchor to the trigger element's position.

## Exit Patterns

Exits are faster than entries:
- Modal: fade out + scale to 0.95. Duration: 150ms, ease-in.
- Drawer: slide back to edge. Duration: 200ms, ease-in.
- Popover: fade out. Duration: 100ms.

## Backdrop

Always darken or blur the background:
```css
.backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  transition: opacity 0.2s;
}
/* Or blur: */
.backdrop { backdrop-filter: blur(4px); }
```

Click-outside-to-dismiss on the backdrop is expected behavior. Implement it.

## Focus Management

See: `accessibility-reduced-motion.md` for full focus management rules.

Critical: when overlay opens, focus moves inside. When it closes, focus returns to trigger. Tab is trapped inside the overlay. Escape closes it.

shadcn Dialog, shadcn Sheet, Radix Dialog handle all of this automatically. If using HTML+Tailwind overlays (HyperUI, Flowbite), you need their JS plugin or manual focus management.

## Component Selection

Search: `mcp doc-index search db=ui-components query="modal dialog overlay"`
- Interactive overlays → shadcn Dialog/Sheet (React, handles focus/keyboard)
- Simple overlays → DaisyUI modal (CSS-based, no JS for basic open/close)
- Astro pages → use `<dialog>` HTML element with inline script for native behavior

## Common Mistakes

- **No exit animation:** Modal appears smoothly but disappears instantly. Always animate both directions.
- **Slow overlay transitions:** Over 300ms for a modal feels sluggish. Keep it fast.
- **No backdrop:** Content behind the modal is still visible and clickable — confusing.
- **Scroll lock missing:** Background content scrolls while modal is open. Add `overflow: hidden` to body.
