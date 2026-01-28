# Global Agent Instructions

This document provides instructions that apply across all of Tyler's projects.

## Git Workflow

**CRITICAL: Never commit or push unless the user explicitly uses words like "commit", "push", "ship it", or "deploy"**

- Completing a feature/task does NOT imply permission to commit or push
- After finishing work, STOP and summarize what was done — do NOT automatically commit
- Each commit and push requires separate, explicit permission from the user
- The user will review all changes before committing
- Previous "ship it" instructions do NOT carry over to new features or tasks
- When in doubt, ALWAYS ASK before committing or pushing — even if the user's intent seems clear
- Building/implementing something is NOT the same as shipping it

**Common mistakes to avoid:**
- "Yep", "Yes", "Sounds good", or "Do it" are NOT commit/push instructions
- Approving a plan that mentions committing is NOT permission to commit
- Approval to make edits is NOT approval to commit those edits
- If the user's message doesn't literally contain "commit", "push", "ship it", or "deploy", ASK first

## Time Estimates

- When giving time estimates, provide AI-assisted time estimates, not human-coder estimates
- Be realistic but account for the speed advantage of AI tooling

## Formatting Preferences

- **Headlines and titles:** Use sentence case (e.g., "Income just hit?" not "Income Just Hit?")
  - This applies to ad copy, landing page headlines, and marketing content

## Writing Style for UI Copy

When writing user-facing text (help dialogs, tooltips, error messages, onboarding, etc.):

- **Be conversational and friendly:** Use "we" and "you" language (e.g., "We look at..." instead of "The system calculates...")
- **Avoid jargon:** Write for a layperson, not a technical audience
- **Be direct and clear:** Get to the point quickly without unnecessary complexity
- **Use concrete examples:** Instead of abstract explanations, show real examples (e.g., "75% means it filled up on 3 out of 4 similar days")
- **Question-based titles:** Use natural questions as dialog titles (e.g., "What does Full % mean?" instead of "Full percentage explained")
- **Keep it casual:** Don't be overly formal or academic
- **Avoid em dashes:** They look too much like AI generated copy

**Example of good UI copy:**

- Title: "How is risk calculated?"
- Body: "We look at how often the sailing fills up. High means it fills more than half the time (>50%). Moderate is 20-50%. Low is less than 20%."

**Example of what to avoid:**

- Title: "Risk level calculation methodology"
- Body: "Risk assessment is derived from historical capacity metrics utilizing a statistical threshold-based classification system."

## Code Quality & Best Practices

### Reuse Existing Components

Before creating a new component, search for existing components that do similar things. Prefer extending or reusing existing components over duplicating code.

**How to check:**

- Search the codebase for similar functionality
- Look in common component directories (`src/components/`, `components/ui/`)
- If unsure, ask the user if a similar component exists

### Use Established UI Component Libraries

When the project uses shadcn/ui (or similar component libraries), prefer using those components over custom Tailwind classes. Check `src/components/ui/` for available components before writing custom markup.

**Common shadcn components to check for:**

- Button, Card, Table, Badge, Dialog, Breadcrumb
- If a component doesn't exist but would be useful, consider installing it via `npx shadcn@latest add [component]`

### Use Suspense for Async Data in Server Components

When a Server Component fetches data, wrap it in `<Suspense>` with a skeleton so the page loads instantly. The parent should own the Suspense boundary, not the async component itself.

**Pattern:**

```tsx
// Parent component
<Suspense fallback={<TableSkeleton />}>
  <AsyncDataTable />
</Suspense>;

// Async component (just fetches and displays)
async function AsyncDataTable() {
  const data = await fetchData();
  return <Table data={data} />;
}
```

### Prefer Whitespace Over Cards

Use whitespace and typography to create visual separation instead of card borders when:

- Content is simple and homogeneous
- The page has a linear, top-to-bottom flow
- There are only a few sections (2-4)

Reserve cards for:

- Grids of clickable items (product cards, blog posts, etc.)
- Mixing different types of content on the same page
- Form containers or distinct workspaces
- Elements that need elevation (modals, popovers, dialogs)

**Example:**

```tsx
// Good: Whitespace for simple sections
<div className="space-y-10">
  <div>
    <h2 className="text-xl font-semibold mb-4">Section One</h2>
    <p>Content...</p>
  </div>
  <div>
    <h2 className="text-xl font-semibold mb-4">Section Two</h2>
    <p>Content...</p>
  </div>
</div>

// Use cards when: grid of interactive items
<div className="grid grid-cols-3 gap-4">
  {items.map(item => (
    <Card key={item.id} className="cursor-pointer hover:shadow-lg">
      <CardHeader>...</CardHeader>
    </Card>
  ))}
</div>
```

## When to Reference Other Documents

Use these specialized guidelines based on what you're working on:

- **Writing landing pages, marketing copy, headlines, or CTAs**: Read [COPYWRITING.md](./COPYWRITING.md) first
- **Writing UI copy (dialogs, tooltips, error messages, onboarding)**: Follow the "Writing Style for UI Copy" section above
