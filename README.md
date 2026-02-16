# OpenColor Labs

![Version](https://img.shields.io/badge/version-2.0.0-blue)
![Speyer UI](https://img.shields.io/badge/Speyer_UI-v2.1.2-green)
![Licence](https://img.shields.io/badge/licence-MIT-lightgrey)

An open-source colour vision analysis tool. Procedurally generated Ishihara plates, three-axis CVD detection, a split-screen Reality Simulator, and daltonisation-based reveal — all in a single HTML file.

**[Live Demo →](https://adrianspeyer.github.io/Open-Colour-Labs/)**

---

## What It Does

| Feature | Description |
|---------|-------------|
| **14-Plate Test** | Procedural Ishihara plates: demo, R/G vanishing, R/G desaturated, brightness traps, tritan (B/Y), hidden-digit, and noise traps |
| **Three-Axis Detection** | Classifies Protan (red-weak), Deutan (green-weak), and Tritan (blue-yellow) deficiencies |
| **Hidden-Digit Plates** | Reverse plates where CVD observers see a number that normal vision cannot — positive confirmation |
| **Severity Estimation** | Differentiates anomalous trichromacy (mild) from dichromacy (severe) based on response patterns |
| **Qualia Validator** | Draggable split-screen simulator using Machado et al. (2009) SVG filters in linearRGB |
| **Reveal Mode** | Daltonisation (Fidaner et al. 2005) shifts R-G opponent signal into the blue channel so CVD users can see what they're missing |

---

## How It Works

### Test Engine

Each plate is **procedurally generated on a `<canvas>`** with HSL jitter, rejection sampling, and 2,000 dots per plate. No plate is ever identical. The 14-plate sequence follows Hardy, Rand & Rittler's (1945) four pseudoisochromatic design types:

1. **Vanishing plates** — a number visible to normal vision, invisible to CVD
2. **Diagnostic plates** — differentiate protan from deutan via luminance
3. **Hidden-digit plates** — a number visible to CVD, invisible to normal vision
4. **Noise traps** — random dots with no number (guessing detection)

The scoring pipeline validates screen calibration (demo plates), detects guessing (noise traps), confirms with hidden-digit results, then classifies by type and severity.

### Simulator

The "Qualia Validator" applies **SVG `feColorMatrix` filters** with `color-interpolation-filters="linearRGB"` to a test image. Users drag a slider to compare the original image with its simulated counterpart. If both sides look identical, the diagnosis is confirmed.

**What colourblind users see:** In "Reveal" mode, the *right* side is daltonised — colours shifted so the CVD observer can perceive differences they normally miss. The left side looks the same as always.

**What normal-vision users see:** In simulation modes (Protanopia, Deuteranopia, Tritanopia), the right side shows the degraded colour space. Normal-vision users see a dramatic difference; CVD users see little or none.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Design System | [Speyer UI v2.1.2](https://github.com/adrianspeyer/speyer-ui) — topbar, nav, footer, cards, badges, shields, progress, alerts, stat/KPI |
| Icons | [Lucide](https://lucide.dev/) (pinned, deferred, auto-init on load) |
| Simulation | Machado, Oliveira & Fernandes (2009) — severity 1.0 matrices |
| Daltonisation | Fidaner, Ozguven & Cemgil (2005) — B' = 0.5R − 0.4G + B |
| Plate Design | Hardy, Rand & Rittler (1945) — four PIP design types |
| Comparison | Draggable split-screen slider with `clip-path` + touch/mouse support |
| SEO | Open Graph, Twitter Cards, JSON-LD (`WebApplication` + `FAQPage`) |
| Accessibility | WCAG 2.1 AA, `prefers-reduced-motion`, `prefers-contrast: more`, `prefers-color-scheme`, safe-area insets, skip navigation, ARIA labels, 44px touch targets |

**Zero dependencies. Zero build steps. Single HTML file.**

---

## Scientific References

- **Machado, S., Oliveira, M. M., & Fernandes, L. A. F.** (2009). A physiologically-based model for simulation of color vision deficiency. *IEEE Transactions on Visualization and Computer Graphics*, 15(6), 1291–1298.
- **Brettel, H., Viénot, F., & Mollon, J. D.** (1997). Computerized simulation of color appearance for dichromats. *JOSA A*, 14(10), 2647–2655.
- **Viénot, F., Brettel, H., & Mollon, J. D.** (1999). Digital video colourmaps for checking the legibility of displays by dichromats. *Color Research & Application*, 24(4), 243–252.
- **Hardy, L. H., Rand, G., & Rittler, M. C.** (1945). Tests for detection and analysis of color blindness. *JOSA*, 35(4), 268–275.
- **Fidaner, I. B., Ozguven, N., & Cemgil, T.** (2005). Analysis of color blindness. Technical report.
- **Gobira, M. et al.** (2025). Assessing the accuracy of a digital color vision test. *Arch Soc Esp Oftalmol*, 100(12), 781–787.

---

## Licence

MIT — see [LICENCE](LICENCE) for details.

---

## Contributing

1. Fork the repository
2. Create a feature branch
3. Submit a pull request with a clear description

Especially welcome:
- Confusion-line-aware colour calibration (CIE chromaticity mapping)
- Adaptive staircase severity measurement
- Additional plate types (arrangement, D-15-style)
- Multilingual support

---

## Changelog

### [2.0.0] — 2026-02-16

**Test Engine**
- Added **tritan (blue-yellow) plates** — tests the S-cone / blue-yellow confusion axis
- Added **hidden-digit (reverse) plates** — CVD observers see a number that normal vision cannot
- Expanded from 10 to **14 plates**: 2 demo, 2 R/G vanishing, 2 R/G desaturated, 2 brightness traps, 2 tritan, 2 hidden-digit, 2 noise traps
- Updated scoring engine: tritan classification, hidden-digit confirmation, severity differentiation
- Added **visual progress bar** (`sui-progress`) showing plate N / 14

**SUI Migration**
- Header → `sui-topbar` + `sui-brand` + `sui-brand-mark` + `sui-brand-name` + `sui-version-pill`
- Navigation → `sui-topbar-actions` (Test, Simulator, GitHub right-aligned)
- Footer → `sui-footer` + `sui-footer-links` + `sui-shield` version badge
- Results → `sui-card` + `sui-card-lg` + `sui-stat` + `sui-kpi-value` + `sui-kpi-label`
- Info box → `sui-alert` + `sui-alert-info` (shown only for Protan/Deutan/Tritan results)
- Icons: pinned Lucide with `onload` callback — fixes missing header icons
- Simulator: replaced overflow-clip approach with `clip-path: inset()` for pixel-perfect image alignment
- Brand logo: green accent on "Color" restored

**Mobile / iPad / iPhone**
- `viewport-fit=cover` + `env(safe-area-inset-*)` for notch/Dynamic Island
- Apple meta tags: `apple-mobile-web-app-capable`, `apple-mobile-web-app-status-bar-style`, `theme-color`
- All touch targets maintain 44px minimum (`--sui-touch-target`)
- Portrait + landscape verified for iPad and iPhone

**SEO**
- `lang="en-CA"` (Canadian spelling throughout)
- Open Graph tags (title, description, url, type, locale)
- Twitter Card tags (summary)
- `<link rel="canonical">`
- JSON-LD structured data: `WebApplication` + `FAQPage`
- Optimised title and meta description targeting "colour blind test" keywords

**Content**
- Fixed disclaimer grammar: "This is an educational tool, not a medical device. It does not replace professional diagnosis."
- Removed internal plate type labels from test UI (Control, Noise Trap, etc. no longer visible to users)

### [1.0.0] — 2026-02-16

- Initial versioned release
- Procedural Ishihara plate generator (5 types)
- Three-option response system (number / nothing / unsure)
- Scoring engine: screen validation → guessing detection → type classification → severity estimation
- Qualia Validator: draggable split-screen simulator with Machado (2009) SVG filters
- Daltonisation-based reveal mode (Fidaner et al. 2005)
- Viewport-safe layout for 1080p displays at 100% zoom
- Speyer UI v2.1.2 design system

---

Made in Canada with ❤️
