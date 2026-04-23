# KubiMax Glass — Jellyfin Theme

A complete **glassmorphism CSS theme** for Jellyfin with four interchangeable color moods, full responsive support (web · mobile · TV), and a rich customization API built entirely on CSS variables.

> Tested against Jellyfin **10.10 / 10.11** web client.  
> Typography: **Montserrat** (self-hosted, no Google Fonts dependency).

---

## Previews

| Deep Forest | Marine Blue |
|-------------|-------------|
| *(screenshot)* | *(screenshot)* |

| Brown | Monochrome |
|-------|-----------|
| *(screenshot)* | *(screenshot)* |

---

## Quick Install

1. Open Jellyfin → **Dashboard → General → Branding → Custom CSS**
2. Paste **one** of the following lines and click **Save**:

```css
/* Deep Forest — pine green + amber */
@import url("https://cdn.jsdelivr.net/gh/tenkuba/GlassJelly@main/themes/kubimax-glass-deep-forest.css");

/* Marine Blue — navy + icy cyan */
@import url("https://cdn.jsdelivr.net/gh/tenkuba/GlassJelly@main/themes/kubimax-glass-marine-blue.css");

/* Brown — cognac + warm sand */
@import url("https://cdn.jsdelivr.net/gh/tenkuba/GlassJelly@main/themes/kubimax-glass-brown.css");

/* Monochrome — pure graphite, wallpaper-first */
@import url("https://cdn.jsdelivr.net/gh/tenkuba/GlassJelly@main/themes/kubimax-glass-mono.css");
```

> Repo: `https://github.com/tenkuba/GlassJelly`  
> Changes apply immediately — no server restart needed.

---

## The Four Themes

| Theme | Accent | Canvas | Best for |
|-------|--------|--------|----------|
| **Deep Forest** | Pine `#2f7a4d` + amber | Deep pine-black radial | Premium cinema feel |
| **Marine Blue** | Marine `#1a6aa8` + icy cyan | Navy-black cool vignette | Classic media server polish |
| **Brown** | Cognac `#8a5a35` + warm sand | Espresso-black warm | Library / archive tone |
| **Monochrome** | Graphite `#525252` + white | Pure black-to-charcoal | Brand-neutral, wallpaper-first |

All four share: frosted-glass surfaces, pill buttons, 1.1em card radius, Montserrat 300–900.

---

## Customization

Add any of these overrides **after** your `@import` line in the Custom CSS box.

### Logo

Replace Jellyfin's default logo with your own image (drawer + header):

```css
:root {
    --logoUrl:    url("https://cdn.jsdelivr.net/gh/tenkuba/GlassJelly@main/assets/banner-light.png");
    --logoHeight:   1.8em;   /* height of the logo */
    --logoMaxWidth: 180px;   /* max width cap */
}
```

> `banner-light.png` (1631 × 463 RGBA) is included in `assets/`.  
> Leave `--logoUrl` unset to keep Jellyfin's default logo.

### Login Page

```css
:root {
    /* Custom wallpaper behind the login card */
    --loginPageBgUrl: url("https://your-server/wallpaper.jpg");

    /* Optional tagline shown at the bottom of the login screen */
    --loginPageText: "My Home Cinema";
}
```

### Imagery

```css
:root {
    --bgBlurHome:         blur(50px) saturate(120%); /* backdrop blur on homepage */
    --bgBrightnessDetail: 0.4;                       /* backdrop brightness on detail page (0–1) */
    --itemBackdropMask:   none;                      /* set to none to disable backdrop gradient fade */
}
```

### UI Visibility Toggles

Show or hide individual UI elements without editing CSS:

```css
:root {
    --clearLogoVisibility:         block;  /* clear-art title logo on detail page */
    --itemTitleVisibility:         block;  /* card title text below thumbnail */
    --libraryLabelVisibility:      block;  /* episode / item count badge */
    --extraCardButtonsVisibility:  none;   /* additional overlay buttons on cards */
    --itemOriginalTitleVisibility: none;   /* original-language subtitle (e.g. Japanese) */
    --miniOverlayButtonVisibility: block;  /* mini play button appearing on card hover */
}
```

### Card Grid Density

Override the number of card columns (default follows responsive breakpoints):

```css
:root {
    --cardCount: 6;   /* force 6 columns on all screen sizes */
}
```

### Icon Pack

Switch to the newer Material Symbols icon style:

```css
:root {
    --iconPack: "Material Symbols Rounded";
}
```

> Requires the font to be loaded separately — see [Google Fonts](https://fonts.google.com/icons).

### Motion Speeds

```css
:root {
    --dur-fast: 80ms;
    --dur-base: 160ms;
    --dur-slow: 280ms;
}
```

---

## Responsive Behaviour

### Web (desktop)

- Frosted-glass header, sidebar, dialogs, cards
- Transparent header on detail pages (backdrop does the visual work)
- Clear-art title logo floats in the backdrop area
- Card grid: 3–10 columns depending on viewport width
- Hover effects with scale + accent glow
- Custom scrollbar (10px, accent on hover)

### Mobile (`.layout-mobile`)

- Header auto-hides when scrolling down (`headroom` classes)
- `env(safe-area-inset-*)` on header, bottom nav and content (notch / island safe)
- Bottom navigation bar gets full glass treatment
- Touch targets minimum **48 px** (WCAG 2.5.5)
- Momentum scrolling + scroll-snap on carousels
- Drawer slides in as overlay (`min(80vw, 320px)`)
- Hover animations disabled (`@media (hover: none)`)

### TV (`.layout-tv`)

- **No backdrop-filter** on structural surfaces — zero GPU cost on SmartTV SoCs
- All elements: `cursor: none`
- Font scale: `×1.15` globally
- Detail page content shifted right to clear the always-visible sidebar
- **Complete focus ring system** for every interactive element type:
  - Cards — `scale(1.06)` + `outline` + accent glow
  - Buttons, links, `[role="button"]` — outline + background tint
  - Primary actions (Play, Submit) — colored gradient fill on focus
  - Form inputs — border + outline + ring
  - Tabs, list items, nav items — contextual fills
  - Dialogs — border accent when `focus-within`
- Hover animations disabled (D-pad ≠ pointer)

---

## File Structure

```
kubimax-glass/
├── themes/
│   ├── _base.css                   ← shared structural rules (all themes @import this)
│   ├── kubimax-glass-deep-forest.css
│   ├── kubimax-glass-marine-blue.css
│   ├── kubimax-glass-brown.css
│   └── kubimax-glass-mono.css
├── fonts/
│   └── Montserrat-*.ttf            ← self-hosted, 8 weights
├── assets/
│   └── banner-light.png            ← KubiMax logo (1631×463 RGBA)
├── preview/                        ← design-system preview cards (HTML)
└── colors_and_type.css             ← standalone token file for preview HTML
```

Each theme is a tiny token file (~45 lines) that `@import`s `_base.css`.  
All structural rules live once in `_base.css`.

---

## How `@import` Works with jsDelivr

When Jellyfin loads e.g. `kubimax-glass-deep-forest.css` from jsDelivr:

1. `kubimax-glass-deep-forest.css` does `@import url("./_base.css")`  
   → resolves to `https://cdn.jsdelivr.net/gh/tenkuba/GlassJelly@main/themes/_base.css` ✓

2. `_base.css` loads fonts via `url("../fonts/Montserrat-*.ttf")`  
   → resolves to `https://cdn.jsdelivr.net/gh/tenkuba/GlassJelly@main/fonts/` ✓

All paths resolve correctly as long as the repo structure is kept intact.

---

## Browser & Device Compatibility

| Platform | Status | Notes |
|----------|--------|-------|
| Jellyfin Web (Chrome/Edge) | ✅ Full | Primary target |
| Jellyfin Web (Firefox) | ✅ Full | |
| Jellyfin Web (Safari) | ✅ Full | `-webkit-` prefixes included |
| Jellyfin Mobile App (iOS) | ✅ Full | Safe-area insets handled |
| Jellyfin Mobile App (Android) | ✅ Full | |
| Jellyfin TV / WebOS / Tizen | ✅ Optimised | Blur disabled for GPU performance |
| Jellyfin Media Player (Qt5) | ⚠️ Partial | Qt5 WebEngine has CSS limitations — Jellyfin limitation |
| Old TV browsers (pre-Chromium 76) | ✅ Fallback | `@supports not (backdrop-filter)` → opaque surfaces |

### Accessibility

- `@media (prefers-reduced-motion: reduce)` — all animations and transitions disabled
- `:focus-visible` — focus rings shown only for keyboard/D-pad navigation, not mouse clicks
- Touch targets ≥ 44 px on web, ≥ 48 px on mobile (WCAG 2.5.5)
- Color contrast: white text on dark glass ≥ 4.5:1 across all four palettes

---

## Troubleshooting

**Theme not loading / styles not changing**  
→ Hard-refresh Jellyfin (`Ctrl+Shift+R`). Custom CSS is cached aggressively.

**Fonts not loading**  
→ Verify your repo is public and the jsDelivr CDN URL is correct (`gh/USER/REPO@BRANCH`).  
→ jsDelivr has a propagation delay of ~24h after a new push.

**Logo replacement not working**  
→ `content: url()` on `<img>` is a Chrome/WebKit extension. It works in Jellyfin's  
Chromium-based web client but not in Firefox. Use `background-image` approach for Firefox:
```css
.adminDrawerLogo img { display: none; }
.adminDrawerLogo::after {
    content: "";
    display: block;
    height: 2em;
    width: 180px;
    background: var(--logoUrl) center/contain no-repeat;
}
```

**Cards have no hover effect on touch screen**  
→ Intentional — hover effects are gated behind `@media (hover: hover) and (pointer: fine)`.

**Detail page title logo appears misplaced**  
→ The floating logo uses `position: absolute` on desktop. If your Jellyfin build has a different  
DOM structure, add this reset:
```css
.detailLogo { position: static !important; }
```

**TV navigation feels wrong after update**  
→ TV layout targets `.layout-tv` class on `<body>`. If Jellyfin changes this class name,  
open an issue.

**Syncplay pulse too distracting**  
→ Disable with:
```css
.syncPlayIconCircle { animation: none !important; }
```

---

## License

MIT — do whatever you want, attribution appreciated.

---

## Credits

Visual language: original KubiMax design.  
Architecture reference: [ElegantFin](https://github.com/lscambo13/ElegantFin) (layout patterns, Jellyfin CSS variable surface).  
Additional techniques: [ZestyTheme](https://github.com/stpnwf/ZestyTheme) (logo injection, headroom, badge corners).  
Jellyfin CSS docs: [jellyfin.org/docs/general/clients/css-customization](https://jellyfin.org/docs/general/clients/css-customization/).
