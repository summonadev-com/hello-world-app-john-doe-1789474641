---
status: pending
title: Minimal Hello World single-page app
---

Current state: the project contains only `README.md`. Everything below must be created from scratch.

1. Initialize the project manifest and tooling config at the repo root: `package.json` (ESM, npm, scripts for dev/build/preview), `tsconfig.json` and `tsconfig.node.json` (strict mode, bundler module resolution, `@/*` path alias mapped to `src/*`), and `.gitignore` (node_modules, dist, generated router artifacts kept out of manual edits). Expected outcome: `npm install` and `npm run dev` are runnable once dependencies are added.

2. Add dependencies in `package.json`: runtime — `react`, `react-dom`, `@tanstack/react-router`; dev — `vite`, `@vitejs/plugin-react`, `typescript`, `@types/react`, `@types/react-dom`, `@tailwindcss/vite`, `tailwindcss`, `@tanstack/router-plugin`. Expected outcome: single lockfile via npm, no other package manager.

3. Create `vite.config.ts` wiring three plugins in order: the TanStack Router plugin (file-based route generation from `src/routes`), the React plugin, and the Tailwind CSS v4 Vite plugin. Also declare the `@` → `src` resolve alias so it matches `tsconfig.json`. Expected outcome: routes under `src/routes/` are auto-discovered and `src/routeTree.gen.ts` is generated on dev/build.

4. Create `index.html` at the repo root with a `#root` mount node, a sensible `<title>` (e.g. "Hello World"), `lang="en"`, and a module script pointing at `/src/main.tsx`. Expected outcome: Vite has a valid entry document.

5. Create `src/styles/global.css` whose first line is exactly `@import "tailwindcss";`. No other rules unless a base body background/antialiasing tweak is needed. Expected outcome: Tailwind utilities available app-wide from one stylesheet.

6. Create `src/main.tsx`: import `@/styles/global.css` once, create the router from the generated route tree, register the router type, and render `<RouterProvider>` into `#root` inside React StrictMode. Expected outcome: app boots with routing active. Never hand-edit `src/routeTree.gen.ts`.

7. Create `src/routes/__root.tsx` as the app shell: a root route rendering an `<Outlet />` inside a full-height wrapper that sets the light theme baseline (white/near-white background, dark neutral text, antialiased). No nav, no header, no footer — scope is a single page. Expected outcome: consistent light canvas for all routes.

8. Create `src/routes/index.tsx` for the `/` route: a centered layout (flex, full viewport height, horizontal and vertical centering, generous padding) containing a single large heading reading "Hello, World!" with a comfortable type scale and slight tracking. No subtitle, button, counter, or input. Expected outcome: visiting `/` shows only the centered greeting.

9. Verification checklist:
   - `npm install` then `npm run dev` starts with no TypeScript or Vite errors.
   - `src/routeTree.gen.ts` is generated automatically and untouched by hand.
   - `/` renders exactly one visible text element: "Hello, World!".
   - Greeting is centered both axes on desktop and mobile widths; text stays legible and does not wrap awkwardly at ~320px.
   - Background is white/near-white, text dark neutral; no dark-mode or color accents.
   - `npm run build` completes cleanly.
