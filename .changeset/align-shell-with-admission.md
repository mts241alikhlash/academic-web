---
'academic-web': minor
---

Match admission-web's shell and page styling: upgrade `@mts241alikhlash/ui` to
1.11 and `web-shared` to 1.3.1, take the shared `theme.css` tokens, move to the
fixed top bar with the app switcher and `NavUser`, use the inset sidebar with
`SidebarToggle`, apply the flat list wrapper and admission's card spacing, and
adopt admission's auth and profile screens.

`profileRoutes` is now registered, so `/profile` and `/profile/edit` resolve
instead of falling through to the 404 page.

`vite.config.ts` excludes `@mts241alikhlash/ui` from the dependency pre-bundle,
as admission-web and assessment-web already do; without it the layout fails
with `Symbol(SidebarContext) not found` because the package is loaded twice.
