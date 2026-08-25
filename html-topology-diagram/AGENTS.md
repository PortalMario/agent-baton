# Agent Instructions

## Scope

- Treat the newest self-contained `index-v*.html` file as the visual baseline unless the user names another version.

## Required

- Inspect the current diagram before changing its layout, paths, labels, or animation timing.
- Keep all text in English unless the user explicitly requests another language.
- Make the smallest visual change that satisfies the request; preserve unrelated, accepted design decisions.
- Keep every diagram file self-contained. Embed all HTML, CSS, SVG, and animation assets in the file.
- Ensure a diagram works when opened directly via `file://`; do not require a development server.
- Design layouts to stay compatible and zoomable across different screen sizes. If the choice is not obvious, prefer larger-screen readability over smaller-screen density.
- Keep text in boxes centered and fully inside the box. Shorten wording or adjust the layout instead of allowing overflow.
- Aim for a clean, technical, minimal visual style with a cool, polished feel and no unnecessary clutter.
- Keep animated packets visible above their paths and cards. Use `prefers-reduced-motion` support for every new animation.
- Verify the final file has no `iframe`, `fetch`, external asset, or dependency on another diagram version.

## Versioning

- Unless the user explicitly requests an in-place edit, create the next `index-v<N>.html` file and leave previous versions untouched.
- Never implement a new version as an iframe wrapper, redirect, DOM patch, or runtime transformation of an older version.
- Do not add version numbers to the visible topology title unless the user asks for one.

## Quality And Safety

- Do not add external fonts, images, libraries, trackers, or network requests.
- Never add secrets, tokens, credentials, private addresses, or personally identifying information.
- Do not update documentation unless the user asks.
- If visual verification is unavailable, state that clearly instead of claiming the rendering was checked.
