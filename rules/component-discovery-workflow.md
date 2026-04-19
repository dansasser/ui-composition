# Component Discovery Workflow

## The Decision
How to find the right component for a UI need.

## The Hierarchy

**Step 1: Search the ui-components index**
```
mcp doc-index search db=ui-components query="<what you need>"
```
Example queries:
- `"animated counter spring easing"` → counter components
- `"hero section landing page"` → full hero blocks
- `"gradient text animated"` → text effect components
- `"card hover effect scale"` → interactive card components
- `"navigation dock macos"` → dock/nav components

If the index returns results, you have candidates with full source, install commands, deps, and framework compat. Compare 3+ before picking.

**Step 2: Structured SQL for filtering**
When semantic search returns too many results, filter with SQL on relations.db:
```sql
-- All counter components that work in Astro without hydration
SELECT c.slug, c.name, l.name as library
FROM components c
JOIN libraries l ON c.library_id = l.id
JOIN framework_compat fc ON fc.component_id = c.id
WHERE fc.framework = 'astro' AND fc.hydration = 'n/a'
AND c.id IN (SELECT component_id FROM component_tags WHERE tag = 'counter')

-- Zero-dependency animation components
SELECT c.slug, c.name FROM components c
WHERE c.id NOT IN (SELECT component_id FROM component_deps)
AND c.id IN (SELECT component_id FROM component_tags WHERE tag = 'animation')
```

**Step 3: Context7 for deeper docs**
If the index has the component but you need more detail (full API, configuration options, version-specific behavior):
```
resolve-library-id: "<library-name>"
query-docs: "<specific question about the component>"
```

**Step 4: GitHub for latest source**
If the index is stale or the component was recently updated:
```
gh api repos/<owner>/<repo>/contents/<path> --jq '.content' | base64 -d
```

## Selection Criteria

When comparing candidates:
1. **Does it match the project's existing stack?** (Same library family preferred)
2. **Dependencies** — does it pull in libraries the project doesn't already use?
3. **Framework compatibility** — does it work in the target framework without workarounds?
4. **Code size** — is a 400-line component overkill for a simple need?
5. **Customizability** — can you modify colors/timing/content via props, or is it hardcoded?
