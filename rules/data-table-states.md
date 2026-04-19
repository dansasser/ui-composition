# Data Table States

## The Decision
How to animate state changes in data tables and grids (dashboards, admin panels, data-heavy pages).

## The Principle
Data tables are reading-intensive. Animation should clarify state changes (sorting, filtering, selection, insertion, deletion) without disrupting the user's scan pattern.

## Sorting Transitions

When a column sorts:
- Column header indicator (arrow) rotates: ↑ to ↓ or vice versa. CSS transition, 150ms.
- Row positions animate: rows slide to their new positions. Duration: 200ms.
- Option: if row animation is too complex, crossfade the entire table body instead (fade out at current order → fade in at new order). Duration: 200ms.

```css
.sort-arrow { transition: transform 0.15s ease; }
.sort-arrow.desc { transform: rotate(180deg); }

/* Crossfade approach for re-ordering */
tbody {
  transition: opacity 0.15s ease;
}
tbody.sorting { opacity: 0.3; }
```

## Row Operations

**Row insertion:**
New row slides down from 0 height + fades in. Duration: 200ms.
```css
@keyframes row-insert {
  from { opacity: 0; max-height: 0; }
  to { opacity: 1; max-height: 80px; }
}
```

**Row deletion:**
Row fades out + collapses height. Duration: 200ms. Then remove from DOM after animation.
Background flash red briefly before fade (optional, communicates "this is being removed").

**Row selection:**
Background color transition on click. Duration: 100ms. Instant enough to feel responsive.

**Row hover:**
Subtle background highlight. Duration: 100ms. See: `hover-interaction-patterns.md`

## Filtering

When filters change:
- If few results change: animate rows in/out individually (slide + fade)
- If many results change: crossfade the entire table body
- Show a brief skeleton or loading state if the filter requires a server call. See: `loading-skeleton-states.md`

## Pagination

Table body crossfades between pages. Duration: 200ms.
Page indicator updates position. See: `navigation-animation.md` for pagination treatment.

Don't slide table content left/right between pages — tables are not carousels.

## Components

Search: `mcp doc-index search db=ui-components query="table data grid sortable"`
Candidates: shadcn Table, DaisyUI table, Flowbite tables, Meraki tables.

For advanced data tables with built-in sorting/filtering:
Use Context7 to check TanStack Table (React Table) docs for integration patterns.

## Performance

Large tables (100+ rows visible):
- Don't animate individual rows — batch operations as crossfade
- Use virtualization (react-virtualized, TanStack Virtual) for rendering performance
- Animation budget is smaller here — data-dense UIs need speed over polish
- See: `performance-motion-budget.md`
