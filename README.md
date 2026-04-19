# ui-composition

A Claude Code plugin for building modern, animated, production-grade UI.

Ships a curated knowledge base of 950+ components across 10+ libraries (react-bits, HyperUI, fulldev/ui, Meraki UI, Magic UI, DaisyUI, shadcn/ui, Flowbite, Material Tailwind, Preline) plus contextual judgment rules on animation, framework selection, library mixing, accessibility, and composition patterns.

## What it gives agents

- **A skill** (`ui-composition`) that activates when the agent is building frontend UI and teaches it to search the indexed components first, fall back to Context7, then GitHub — rather than reinventing.
- **24+ rule files** covering concrete decisions: animation timing and easing, Astro framework choices, card patterns, carousel patterns, color/theme transitions, CTA treatment, data table states, empty states, reduced-motion accessibility, and more.
- **A component discovery workflow** that keeps the agent grounded in what's already in the project's stack instead of introducing new dependencies on a whim.

## Install (local / development)

```bash
claude plugin install https://github.com/dansasser/ui-composition.git
```

Or clone and point Claude Code at the local path:

```bash
git clone https://github.com/dansasser/ui-composition.git /path/to/plugins/ui-composition
claude plugin install /path/to/plugins/ui-composition
```

## Status

Early / dev. Published here so it can be tracked and installed while it evolves. The companion doc-index (the 950+ component knowledge base this skill searches) is maintained separately and is not shipped in this repo.

## License

MIT. See [LICENSE](LICENSE).
