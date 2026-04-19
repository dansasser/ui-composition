---
name: ui-composition
description: Build modern, animated, production-grade UI using a curated component knowledge base. Activates when building web pages, components, landing pages, dashboards, or any frontend interface. Searches indexed component libraries first, falls back to Context7 and GitHub for deeper detail.
---

You are building frontend UI. Your job is to produce modern, polished, production-grade interfaces that feel alive and intentional. You have access to a curated knowledge base of 950+ components across 10+ libraries.

## Knowledge Retrieval Hierarchy

**Always search before building.** Don't reinvent what exists.

1. **ui-components doc-index** (FIRST) — `mcp doc-index search db=ui-components query="..."`. This is your curated library with full source code, install commands, dependencies, and framework compatibility notes. Search here first for any component need.

2. **Context7** (SECOND) — if the index doesn't have what you need, or you need deeper documentation (API changes, version-specific behavior, configuration options), use Context7 to pull the library's own docs.

3. **GitHub** (THIRD) — for exact source code, latest changes, or components not yet in the index, fetch directly from the library's GitHub repo.

**Never skip step 1.** Even if you think you know how to build something, search the index first. You might find a better implementation than what you'd write from scratch.

## Decision Framework

Every UI decision flows through context. There are no universal rules — only judgment patterns that apply differently depending on the project, the page, the section, and the user's intent.

### Reading the Project

Before touching any component, understand:
- **What framework is this?** Astro, React, Next.js, plain HTML+Tailwind? This eliminates incompatible libraries immediately.
- **What's already on the page?** If shadcn components are in use, prefer shadcn. If HyperUI blocks structure the layout, stay HTML+Tailwind. Don't introduce a new library unless nothing in the current stack can do the job.
- **What's the dependency budget?** Marketing page = lightweight (CSS, zero-dep components). App dashboard = heavier deps acceptable. Each new animation library (motion, gsap, ogl) adds bundle weight.

### Reading the Section

Each section of a page has a job. The treatment matches the job:
- **Selling sections** (hero, features, pricing, testimonials) — these earn animation. Entrance reveals, counter animations, hover interactions. The user should feel energy.
- **Content sections** (blog body, documentation, terms) — these earn clarity. Clean typography, good spacing, readable hierarchy. Animation here is noise.
- **Navigation** (header, footer, sidebar, breadcrumbs) — functional. Fast. Predictable. Subtle hover states at most.
- **Interactive elements** (forms, modals, drawers, tabs) — smooth transitions that confirm the user's action. Not decorative — communicative.

### Animation Judgment

Animation is not decoration. It communicates: something appeared, something changed, something succeeded, something needs attention. When deciding whether to animate:

- What is this animation DOING? (clarifying, rewarding, guiding, communicating)
- Would this feel incomplete WITHOUT the animation?
- Would this feel SLOWER with the animation?
- How often does this animation fire? (once per page load = fine. every keystroke = reconsider)

Don't default to "more" or "less." Default to "what does this moment need?"

### Component Selection

When the index returns multiple candidates for a need:

1. **Same visual layer = same library.** Three feature cards on one row all come from the same source.
2. **Different layers CAN mix.** An HTML layout block (HyperUI) containing a React animation island (react-bits) is fine — they don't conflict because they operate at different levels.
3. **Check deps.** If the project already has `motion`, prefer components that use `motion` over ones that pull in `gsap`. Don't add a second animation runtime for one component.
4. **Check framework_compat.** For Astro: components with `hydration: n/a` are free (no client JS). Components needing `client:load` add weight. Prefer server-rendered when the component doesn't need interactivity.
5. **Zero-dep components are gold.** Pure CSS/HTML components load instantly, need no hydration, and never break. Use them when they achieve the same effect as a JS-heavy alternative.

### Modern CSS

Many effects that used to require JavaScript are now CSS-native. Before reaching for a React component:

- `animation-timeline: scroll()` — scroll-driven animations without IntersectionObserver
- `@starting-style` — entry animations on element creation
- `view-transition-api` — page transition animations
- `container queries` — responsive components based on parent, not viewport
- `:has()` selector — parent styling based on child state
- `scroll-snap` — carousel/paging behavior without JS
- `@property` — animatable custom properties (gradient animations, etc.)

If CSS can do it, CSS should do it. Smaller bundle, faster paint, works without hydration.

## Rules

This skill is backed by individual rule files in the `rules/` directory. Each rule encodes one judgment pattern. Rules cross-reference each other — follow the references to build complete understanding of a decision.

### How to find the right rule

**Building a page from scratch?** Start with:
- `page-section-treatment.md` — overall page rhythm and per-section energy levels
- `component-discovery-workflow.md` — how to search the index for what you need

**Adding animation to a section?** Read in order:
1. `scroll-triggered-animation.md` — should this section animate on scroll?
2. `animation-timing-easing.md` — how fast, what curve?
3. `performance-motion-budget.md` — can the page afford this?
4. `accessibility-reduced-motion.md` — what happens with reduced motion?

**Choosing a component?** Follow:
1. `component-discovery-workflow.md` — search the index
2. `library-mixing.md` — is it safe to use with what's already on the page?
3. `modern-css-first.md` — does this even need a JS component, or can CSS do it?

**Framework-specific work:**
- Astro → `astro-framework-decisions.md` (hydration, islands, is:inline)
- Any framework → `modern-css-first.md` (CSS capabilities checklist)

**By element type:**
- Buttons/CTAs → `cta-button-treatment.md`
- Cards → `card-component-patterns.md`
- Text effects → `typography-animation.md`
- Images → `image-reveal-techniques.md`
- Navigation → `navigation-animation.md`
- Modals/drawers → `modal-drawer-transitions.md`
- Forms → `form-feedback-animation.md`
- Loading states → `loading-skeleton-states.md`
- Empty states → `empty-state-treatment.md`
- Hover effects → `hover-interaction-patterns.md`
- Colors/themes → `color-theme-transitions.md`
- Toasts/notifications → `toast-notification-patterns.md`
- Carousels/sliders → `carousel-slider-patterns.md`
- Data tables → `data-table-states.md`
- Mobile/responsive → `responsive-animation.md`

Rules reference each other. When one rule says "See: `performance-motion-budget.md`", read that rule too. The cross-references build a complete picture.

Rules grow over time. Every real project encounter that produces a non-obvious judgment call becomes a new rule.

## What This Skill Does NOT Do

- It does not prescribe an aesthetic direction (that's the user's call, or the frontend-design skill's territory)
- It does not choose fonts or color palettes
- It does not generate full page layouts from scratch without user direction
- It does not have a bias toward maximalism or minimalism — it has a bias toward INTENTIONALITY
