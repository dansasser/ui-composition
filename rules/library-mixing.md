# Library Mixing

## The Decision
When is it safe to use components from multiple libraries on the same page?

## Safe Mixing Patterns

**Different layers, different libraries:**
- Layout structure from HyperUI (HTML+Tailwind) + animated counter from react-bits (React island) = fine. They operate at different levels. The HTML provides the skeleton, the React component is a self-contained island inside it.
- shadcn form components + Magic UI animated hero text = fine. Forms are interactive widgets, hero text is a visual effect. Different concerns.

**Same layer, same library:**
- Three feature cards in a row → all from the same source. Don't mix a HyperUI card, a Flowbite card, and a Meraki card in one grid. Different border-radius, padding, shadow conventions will clash.
- Navigation items → all one library. A shadcn dropdown next to a Flowbite mega-menu looks inconsistent.

**The exception — utility components:**
DaisyUI classes, Tailwind utilities, and CSS-only effects can be mixed freely with anything because they don't have visual opinions beyond what you apply. A `btn` class from DaisyUI inside a react-bits animated wrapper is fine.

## Dependency Conflicts

Before mixing, check component_deps:
- Two components both using `motion` (Framer Motion) → compatible, one shared dep
- One using `motion`, another using `gsap` → two animation runtimes in the bundle. Avoid unless both are genuinely needed.
- One using `ogl`, another using `three` → two WebGL runtimes. Avoid.

**Budget rule of thumb:** if total animation-related deps exceed 40KB gzipped, reconsider. See: `performance-motion-budget.md` for the full budget framework. Pick one primary animation runtime and stick with it.

## Framework Mixing in Astro

Astro uniquely allows mixing React, Vue, Svelte, and Solid components on the same page. Each becomes an independent island. This means:
- A React component from react-bits and a Svelte component from Flowbite-Svelte CAN coexist
- But each island hydrates independently → more JS shipped to the client
- Prefer staying in one framework per page unless a specific component only exists in another framework

## When to Consolidate

If you find yourself using 4+ libraries on one page, stop and ask:
- Can one library cover most of these needs?
- Am I mixing because I'm being lazy (grabbing the first result) or because each library genuinely does something the others can't?
- Would a custom component (no library) be simpler than juggling multiple deps?
