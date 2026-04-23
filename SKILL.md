---
name: kubimax-glass-design
description: Use this skill to generate well-branded interfaces and assets for KubiMax (a personal Jellyfin instance with a frosted-glass theme system), either for production CSS or throwaway prototypes/mocks. Contains the four glass themes (Deep Forest, Marine Blue, Brown, Monochrome), Montserrat typography, design tokens, and a Jellyfin-shaped UI kit for prototyping.
user-invocable: true
---

Read the `README.md` file within this skill for the complete visual system, then explore:

- `colors_and_type.css` — shared design tokens (typography, radii, spacing, motion, glass math).
- `themes/_base.css` — the structural layer every theme imports (layouts, blurs, cards, buttons, dialogs, inputs, tabs, progress, focus). **Do not duplicate structural rules into individual theme files.**
- `themes/kubimax-glass-<name>.css` — one theme = one small token file (accent scale + canvas gradient + Jellyfin variable bridge).
- `ui_kits/jellyfin/` — interactive recreation of Jellyfin Home / Library / Detail / Player with the four themes as a live switcher.
- `preview/` — small design-system cards (colors, type specimens, button states, glass surfaces).

If creating visual artifacts (slides, mocks, throwaway prototypes), copy assets out of `assets/` and `fonts/` and create static HTML files that `@import` the shared tokens.

If working on production Jellyfin CSS, copy `themes/_base.css` + one `kubimax-glass-*.css` + the `fonts/` folder into your repo; reference them via jsDelivr or host them yourself; paste an `@import` URL into Jellyfin's Custom CSS box.

If the user invokes this skill without any other guidance, ask them:
1. Which surface — new theme variant, tweak to existing theme, a full Jellyfin page mock, or a marketing asset?
2. Which of the four moods is the starting point, or is a new accent needed?
3. Any known Jellyfin version constraints (10.10 vs 10.11)?

Then act as an expert designer who outputs either static HTML artifacts (prototype) or drop-in CSS (production), depending on the need. Always keep: pill buttons, rounded cards, heavy frosted glass, Montserrat, and the home-blur / detail-brightness rules intact unless the user explicitly asks to change them.
