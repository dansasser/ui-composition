# Empty State Treatment

## The Decision
What to show when a section, list, or page has no data.

## The Principle
Empty states are opportunities, not failures. An empty inbox, an empty search result, a fresh dashboard — these are moments to guide the user toward their first action.

## Patterns

**Illustration + message + CTA:**
The standard pattern. A simple illustration (icon or SVG), a friendly message, and a button to take action.
```html
<div class="flex flex-col items-center justify-center py-16 text-center">
  <svg class="w-16 h-16 text-gray-300 mb-4"><!-- icon --></svg>
  <h3 class="text-lg font-medium text-gray-900">No projects yet</h3>
  <p class="text-gray-500 mt-1">Create your first project to get started.</p>
  <button class="mt-4 btn btn-primary">Create Project</button>
</div>
```

Search: `mcp doc-index search db=ui-components query="empty state no data placeholder"`
Candidates: HyperUI empty-states, Flowbite empty-content, shadcn empty.

**Skeleton that never resolves:**
For initial loading that returns no data — the skeleton appears, then cross-fades to the empty state message. Don't just remove the skeleton abruptly.
See: `loading-skeleton-states.md`

**First-use onboarding:**
Different from "no data" — this is a fresh account. Show a guided setup flow, not just an empty message. Step-by-step cards or a checklist.

## Animation

Empty states benefit from a gentle entrance — fade in + slight translate up. The illustration can have a subtle idle animation (floating, breathing, gentle bob) to prevent the page from feeling dead.

But: the animation should be calming, not attention-demanding. The user is already dealing with "nothing here" — don't add visual stress.

```css
.empty-state {
  animation: gentle-in 0.4s ease-out;
}
@keyframes gentle-in {
  from { opacity: 0; transform: translateY(12px); }
  to { opacity: 1; transform: translateY(0); }
}
.empty-illustration {
  animation: float 3s ease-in-out infinite;
}
@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-6px); }
}
```

## Context Matters

- **Search with no results:** "No results for 'xyz'. Try different keywords." No illustration needed — keep it tight, the user wants to retry quickly.
- **Empty dashboard:** Illustration + guided setup. This is the user's first impression.
- **Empty list after filtering:** "No items match your filters." + clear filters button. Quick recovery path.
- **Empty cart:** Illustration + "Continue shopping" CTA. Light, encouraging.
