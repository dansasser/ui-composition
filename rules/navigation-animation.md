# Navigation Animation

## The Decision
How should navigation elements (headers, sidebars, menus, breadcrumbs, tabs) behave with motion?

## The Principle
Navigation is functional infrastructure. Users interact with it constantly. Animation should make navigation FASTER to use and CLEARER to understand — never slower or decorative.

## Header/Navbar

**Scroll behavior:**
- Sticky header that shrinks on scroll: height transitions from ~80px to ~64px, logo scales down, background opacity increases. Duration: 150ms, ease-out.
- Hide-on-scroll-down, show-on-scroll-up: translateY transition, 200ms. This pattern reclaims screen space on mobile.
```css
.header { transition: transform 0.2s ease-out; }
.header--hidden { transform: translateY(-100%); }
```

**Mobile hamburger menu:**
- Icon morphs from ☰ to ✕ using CSS transform (rotate + scale).
- Menu panel slides in from the side or expands from top. Duration: 250ms, ease-out.
- Backdrop darkens behind.
- See: `modal-drawer-transitions.md` for the overlay pattern.

**Desktop dropdown menus:**
- Appear on hover with 100-150ms delay (prevents accidental triggers).
- Fade in + slight translateY(-4px → 0). Duration: 150ms.
- Disappear on mouse-leave with 200ms delay (grace period for diagonal mouse movement to submenu).

## Sidebar

- Collapse/expand: width transitions from full to icon-only. Duration: 200ms, ease-in-out.
- Don't animate individual sidebar items on collapse — animate the container width, let items respond via CSS.
- Active item indicator: a small bar or highlight that slides to the active item position.

Search: `mcp doc-index search db=ui-components query="sidebar navigation collapsible"`
Candidates: shadcn Sidebar, Flowbite sidebar, DaisyUI drawer.

## Tabs

- Active tab indicator slides between tabs (translateX). Duration: 200ms, ease-out.
- Tab content crossfades (outgoing fades out → incoming fades in). Duration: 150ms each.
- Don't slide tab content left/right — that implies a linear sequence. Tabs are random access.

Search: `mcp doc-index search db=ui-components query="tabs animated indicator"`
Candidates: DaisyUI tab, shadcn Tabs, Flowbite tabs.

## Breadcrumbs

No animation. Static. Functional. Adding animation to breadcrumbs would be bizarre.

## Pagination

Page transitions between paginated content: crossfade the content area. Duration: 200ms. Don't slide — the user isn't navigating a sequence, they're jumping to a page.

## Page Transitions (Astro/SPA)

For multi-page sites using Astro View Transitions or SPA routing:
- Crossfade between pages. Duration: 200ms.
- Shared elements (header, sidebar) should NOT transition — they persist.
- Content area transitions independently.
See: `modern-css-first.md` for View Transitions API.
