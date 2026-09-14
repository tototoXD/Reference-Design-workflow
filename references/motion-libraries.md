# Motion selection and transitions

Read this before HTML implementation when adding motion. Reuse the existing project's animation system first. Choose based on the interaction, framework, and product character; GitHub stars indicate popularity, not quality.

## Shortlist

Discovery snapshot from GitHub search on 2026-09-12; counts are approximate and may be cached.

| Library | Stars | Fit | Package / docs |
|---|---:|---|---|
| [Motion](https://github.com/motiondivision/motion) | 33.6k | Application UI, enter/exit, layout changes, gestures; React or vanilla JS | `motion`; https://motion.dev/docs |
| [Anime.js](https://github.com/juliangarnier/anime) | 69k | Coordinated timelines, staggered elements, expressive product storytelling | `animejs`; https://animejs.com/documentation/ |
| [Swup](https://github.com/swup/swup) | 5.2k | Page navigation in server-rendered multi-page websites | `swup`; https://swup.js.org/ |

All three core libraries list MIT licenses. Check the selected version's official documentation before implementing; Motion premium products are separate from its core package.

## Choose the smallest sufficient solution

- Simple hover, focus, pressed, and opacity changes: CSS transitions suffice.
- React tabs, drawers, dialogs, and layout transitions: prefer Motion when a library is justified; new Motion React imports use `motion/react`.
- Vanilla HTML with coordinated sequences: choose Anime.js or Motion's JavaScript API according to the required behavior. Use the current installed API; do not mix Anime.js v3 examples into v4.
- Multi-page server-rendered sites: use Swup only when actual page transitions are needed. Preserve history, scroll restoration, focus, page titles, script lifecycle, forms, and normal link fallback. Do not layer it over an existing SPA router.
- Use one main animation engine per project. Install only the chosen package in the target project when needed, preserving its package manager and lockfile. Prefer bundled local dependencies over runtime CDN requirements, especially on restricted networks.

## Design motion deliberately

Write a short motion section in the generated AGENTS.md: purpose, trigger, affected element, duration, easing, and reduced-motion behavior. Static screenshots do not establish animation behavior; mark added motion as a proposed design choice.

Starting defaults, adjustable to the reference and product:
- Hover/press feedback: 100–160 ms.
- Tab/content crossfade: 160–240 ms with at most 4–8 px displacement.
- Drawer/dialog: 180–280 ms, with minimal scale only when it supports hierarchy.
- Page transitions: 200–350 ms; keep navigation responsive.
- Stagger only a small group when it clarifies order. Keep cumulative delay short enough that users can immediately act.

Animate to explain navigation, reveal hierarchy, or confirm a state change. Preserve stable headers and navigation when only content changes. Avoid animating every section on every render, autoplay loops, scroll hijacking, and long decorative entrance sequences in task-focused tools.

## Implementation and verification

- Prefer transform and opacity for routine transitions. Profile necessary layout animation; avoid blanket `transition: all`.
- Honor `prefers-reduced-motion` in both CSS and JavaScript. Remove large travel, parallax, and decorative sequencing; show the final state immediately or use a brief opacity change.
- Preserve keyboard focus and accessible dialog behavior through enter/exit. Exiting content must not remain accidentally focusable or intercept clicks.
- Rapid repeated actions, reversing a transition, and navigating away must leave a valid final state. Clean up animation instances and listeners on unmount or page replacement.
- Content must remain available when animation fails or JavaScript enhancement is unavailable. Do not leave important content permanently at opacity zero.
- During Product Design QA, test primary transitions, keyboard operation, reduced motion, rapid clicks, browser back/forward when applicable, and mobile performance. Record defects alongside the normal design QA.
- For screenshot exports, wait until transitions settle and capture the intended final state.
