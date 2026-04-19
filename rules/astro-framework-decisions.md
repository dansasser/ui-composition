# Astro Framework Decisions

## The Decision
When building in Astro: what should be a native .astro component, what should be a React island, and what hydration directive to use?

## The Hierarchy

**Native Astro (no hydration, no client JS):**
- Static layout: headers, footers, cards, grids, text sections
- Content from CMS/markdown: blog posts, docs, product pages
- HTML+Tailwind components: HyperUI blocks, Meraki UI, fulldev/ui
- Components that don't need JavaScript interactivity at all
- **This is the default.** Everything starts here.

**React island with `client:visible`:**
- Scroll-triggered animations (only hydrate when scrolled into view)
- Below-fold interactive elements (carousels, accordions)
- Counter animations that start on scroll
- Any component the user won't interact with until they scroll to it
- **Best for performance** — defers JS until needed

**React island with `client:load`:**
- Above-fold interactive elements (hero animations, nav dropdowns)
- Elements that must be interactive immediately on page load
- Form components with validation (need JS from the start)
- Anything using Radix UI primitives (shadcn components — they need JS for accessibility)

**React island with `client:idle`:**
- Lower-priority interactivity (social share buttons, comment sections)
- Elements that need JS but aren't urgent — let the main thread settle first

**React island with `client:media`:**
- Components that only need JS at certain viewport sizes
- Example: mobile hamburger menu → `client:media="(max-width: 768px)"`

## Common Mistakes

**Mistake: Making everything a React island**
If a card is just HTML + Tailwind classes with no onClick, no state, no animation — it's an .astro component. Don't wrap it in React for no reason.

**Mistake: Using `client:load` for below-fold content**
A counter animation at the bottom of the page doesn't need JS loaded at page start. Use `client:visible` — it hydrates when the user scrolls there.

**Mistake: Inline `<script>` without `is:inline`**
Astro bundles bare `<script>` tags and they only execute once. On ClientRouter navigation, they don't re-run. Always use `<script is:inline>` for scripts that need to run on every page visit. No TypeScript in inline scripts.

**Mistake: Fetching data client-side when Astro can do it server-side**
Astro's frontmatter runs on the server. Fetch data there, pass it as props. Don't add a React component just to do a fetch + render.

## Decision Flowchart

1. Does this element need ANY JavaScript? → No → native .astro
2. Is it above the fold? → Yes → `client:load`
3. Is it scroll-triggered or below fold? → Yes → `client:visible`
4. Is it low priority? → Yes → `client:idle`
5. Is it viewport-dependent? → Yes → `client:media`

## Index Integration

When searching for components to use in Astro, filter by framework_compat:
```sql
SELECT c.slug, c.name, fc.hydration, fc.notes
FROM components c
JOIN framework_compat fc ON fc.component_id = c.id
WHERE fc.framework = 'astro' AND fc.supported = 1
```

Components with `hydration: n/a` are free — no client JS cost.
