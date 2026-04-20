# Portfolio Publish Playbook (GitHub)

## Do
- Keep only source code and production assets in the repo.
- Use clear project metadata: title, description, canonical URL, Open Graph image, and robots rules.
- Pin and review dependencies regularly (`npm audit`, dependency updates).
- Optimize images and serve only required file sizes.
- Keep one visual system (colors, spacing, typography) across all sections.
- Test responsive layouts on mobile (<= 480px), tablet, and desktop.
- Validate links (resume, LinkedIn, GitHub, Credly, mailto, tel).
- Keep copy concise, outcome-focused, and measurable.
- Run `npm run build` before every push.
- Keep secrets out of Git (tokens, private docs, internal URLs, credentials).

## Don't
- Do not publish private files (`.env`, local notes, personal archives, draft credentials).
- Do not keep dead components or unused layouts in a public portfolio codebase.
- Do not use broken asset references (missing badge/image files).
- Do not overload the page with duplicate sections or repeated claims.
- Do not commit generated folders (`dist`, `.astro`, `node_modules`).
- Do not mix too many font families or inconsistent heading styles.
- Do not ship without checking accessibility basics (contrast, focus, semantic headings).
- Do not leave TODO/debug/test content visible in production.

## Remove From This Repository Before/During Publish
- Legacy component architecture no longer used by the new single-page design:
  - `src/components/About.astro`
  - `src/components/Certifications.astro`
  - `src/components/Contact.astro`
  - `src/components/Education.astro`
  - `src/components/Experience.astro`
  - `src/components/Footer.astro`
  - `src/components/Hero.astro`
  - `src/components/Nav.astro`
  - `src/components/Projects.astro`
  - `src/components/Sidebar.astro`
  - `src/components/Skills.astro`
  - `src/components/Testimonials.astro`
- Any stale references to badge images that do not exist in `public/badges/`.
- Draft-only tracking notes if you do not want to expose process docs publicly (`ISSUES.md`).
- Any private qualification archives (already protected by `.gitignore`: `MyQualifications/`).

## Redesign TODO
- [x] Audit existing structure, content blocks, and assets.
- [x] Create publishing do/don't guidance and cleanup list.
- [x] Replace old layout system with a new visual design language.
- [x] Build a completely new responsive homepage structure.
- [x] Introduce professional typography and refined spacing rhythm.
- [x] Rebuild section hierarchy for stronger narrative flow.
- [x] Remove unused legacy components from `src/components`.
- [x] Update 404 page style to match the new brand direction.
- [x] Run final build verification.
- [x] Final content polish pass and publish.