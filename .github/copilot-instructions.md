# GitHub Copilot Instructions for Mona Mayhem

## Project Context
Mona Mayhem is an Astro 5.14.4 SSR application with Node.js adapter that fetches and displays GitHub user contributions data.

## Key Stack
- **Framework:** Astro 5.14.4 with `@astrojs/node` adapter
- **Language:** TypeScript
- **API Route:** `/api/contributions/[username].ts` for GitHub data
- **Home Page:** `src/pages/index.astro`

## Development Commands
```bash
npm run dev      # Start dev server (http://localhost:3000)
npm run build    # Build for production
npm run preview  # Preview production build
```

## Astro Best Practices

### File Structure
- Pages in `src/pages/` auto-route (e.g., `about.astro` → `/about`)
- Dynamic routes use `[param].astro` syntax (e.g., `[username].astro` → `/user/john`)
- API endpoints in `src/pages/api/` respond with `new Response(json)`
- All files are server-rendered (SSR) by default

### Component Guidelines
- `.astro` files are server-rendered only
- When importing client-side components, use `client:*` directives:
  - `client:load` - Load immediately
  - `client:idle` - Load when browser is idle (preferred for non-critical UI)
  - `client:visible` - Load when in viewport
  - `client:only` - Never SSR this component
- Prefer server-side data fetching in `.astro` components
- Use `export const prerender = true` for static generation when applicable

### Data Fetching
- Fetch data at the top level of `.astro` components (server-side)
- In API routes, use `Astro.request` to access URL parameters and body
- Dynamic route params are passed via the `params` prop

### Styling
- Use scoped `<style>` blocks in `.astro` components
- CSS is automatically scoped to the component
- Use `:global()` selector for global styles only when necessary

### Code Quality
- Run `npm run build` before commits to catch TypeScript and build errors
- Keep client components minimal for better performance
- Avoid unnecessary client-side interactivity

## API Route Pattern
For `/api/contributions/[username].ts`:
```typescript
export async function GET({ params }: { params: { username: string } }) {
  const data = await fetchGitHubData(params.username);
  return new Response(JSON.stringify(data), {
    headers: { 'Content-Type': 'application/json' }
  });
}
```

## What NOT to Do
- Don't add unnecessary client-side hydration (`client:*` directives without reason)
- Don't fetch data inside components without considering SSR
- Don't commit without running `npm run build`
- Don't modify `astro.config.mjs` without understanding the Node adapter requirements
- Don't create files in `workshop/` - it's documentation only

## For More Information
See `CLAUDE.md` in the project root for detailed guidelines.
