# OpenColor Labs

**Open-source colour vision analysis** — transparent science, community driven, no paid diagnosis.

🔗 **Live Demo:** [adrianspeyer.github.io/Open-Colour-Labs](https://adrianspeyer.github.io/Open-Colour-Labs/)

## What It Does

OpenColor Labs is a free, browser-based colour blindness screening tool that uses **18 procedurally generated Ishihara-style plates** to detect and differentiate protan (red-weak), deutan (green-weak), and tritan (blue-yellow) colour vision deficiencies. It includes a **Reality Simulator** with split-screen comparison sliders, contextual narration referencing the actual test image, and daltonisation-based reveal filters.

## Features

### 18-Plate Colour Vision Test
- **Demo plates (2):** Screen calibration — if you can't see these, adjust your display
- **Red-green vanishing (3):** Standard protan/deutan detection
- **Red-green desaturated (2):** Catch subtle deficiencies
- **Brightness trap (2):** Protan-specific — exploits L-cone luminance loss
- **Transformation (2):** Protan reads one number, deutan reads a different number
- **Tritan (2):** Blue-yellow confusion axis
- **Hidden-digit (2):** CVD observers see the number, normal vision doesn't — positive confirmation
- **Noise trap (3):** Pure random dots — detects guessing

### Scoring Engine
- Protan vs deutan differentiation using transformation plates + brightness response
- Severity estimation: anomalous trichromacy (mild) vs dichromacy (severe)
- Hidden-digit confirmation when applicable
- Noise trap validation to flag unreliable results

### The "Qualia" Validator — Reality Simulator
- **3 simulation tabs:** Protanopia, Deuteranopia, Tritanopia — each with a draggable split-screen slider
- **Reveal tab:** Toggle between "Your Vision" and "Colours Revealed" using type-specific daltonisation filters
  - Protan reveal: shifts red luminance + R-G hue into blue channel
  - Deutan reveal: shifts R-G hue distinction into blue channel
  - Tritan reveal: shifts B-Y distinction into red-green channel
- **Contextual narration:** Each tab describes what to look for in the actual test image (fruits and vegetables)
- **Slider performance:** `requestAnimationFrame` for smooth 60fps dragging

### Results Detail Panel
- Collapsible "What does this mean?" section with:
  - Prevalence statistics (per-type)
  - Daily impact descriptions
  - Protan vs deutan luminance explanation
  - Career and occupational notes
  - Linked academic sources (Alhazmi 2025, Gobira et al. 2025, Global CVD Meta-Analysis 2025)

## Tech Stack

| Component | Technology |
|-----------|-----------|
| UI Framework | [Speyer UI v2.1.2](https://github.com/adrianspeyer/speyer-ui) |
| CVD Simulation | SVG `feColorMatrix` — Machado et al. (2009) severity=1.0 in linearRGB |
| Daltonisation | Custom SVG `feColorMatrix` filters per CVD type (Fidaner et al. 2005 inspired) |
| Plate Generation | Procedural canvas — randomised dot placement with HSL colour masks |
| Slider | CSS `clip-path: inset()` with `requestAnimationFrame` updates |
| Icons | Lucide (pinned with `onload` callback) |
| SEO | JSON-LD (WebApplication + FAQPage), Open Graph, Twitter Cards |
| Mobile | `viewport-fit=cover`, `safe-area-inset-*`, `apple-mobile-web-app-capable` |

## Scientific References

- **Machado, S., Oliveira, M., & Fernandes, L. (2009).** *A Physiologically-based Model for Simulation of Color Vision Deficiency.* IEEE TVCG, 15(6), 1291–1298.
- **Fidaner, I.B., Ozguven, N., & Sahin, U. (2005).** *Analysis of Color Blindness.* Istanbul — daltonisation algorithm basis.
- **Gobira, C. et al. (2025).** *Digital Ishihara Testing Validation.* 96.4% sensitivity, 99.3% specificity vs physical plates.
- **Alhazmi, A. (2025).** *A Global Perspective of Color Vision Deficiency: Awareness, Diagnosis, and Lived Experiences.* Healthcare, 13(16), 2031.
- **Global CVD Meta-Analysis (2025).** *Global Prevalence of Congenital Color Vision Deficiency among Children and Adolescents, 1932–2022.* Ophthalmology.

## Disclaimer

This is an **educational tool**, not a medical device. It does not replace professional diagnosis. Screen calibration, ambient lighting, and display settings affect results. Consult a qualified optometrist for a definitive colour vision assessment.

## Changelog

### v3.0.1 — QA & UX Fixes
- **Transform plate multi-select:** Options now include combo answers (e.g. "2 & 5") alongside singles, so users who see both numbers can select them naturally — without revealing that two numbers exist on the plate. Decoy combos prevent the correct combo from standing out.
- **Sim descriptions rewritten:** Narration now speaks directly to the CVD user ("This is for you") rather than describing the condition abstractly. Reveal descriptions clarify that someone without CVD would see no difference between views.
- **Reveal crossfade:** Toggle between "Your Vision" and "Colours Revealed" now uses an in-place opacity crossfade instead of a full DOM re-render — no image flicker.
- **Green header fix:** Added explicit CSS overrides (`.brand-green` class) so SUI topbar component styles no longer override the green accent colour on the logo and nav icons.
- **Diagnosis engine:** "Both" (combo) answers on transform plates are excluded from protan/deutan scoring — seeing both numbers indicates normal vision on that axis.

### v3.0.0 — Major Release
- **18 plates** (up from 14): added 2 transformation plates, 1 extra R/G vanishing, 1 extra noise trap
- **Transformation plates:** Protan sees digit A, deutan sees digit B — reliable differentiation
- **Tab-based simulator:** 4 tabs (Protanopia, Deuteranopia, Tritanopia, Reveal) replacing sidebar layout
- **Reveal mode redesign:** Toggle between "Your Vision" and "Colours Revealed" with type-specific daltonisation filters (separate protan, deutan, tritan reveal formulas)
- **Contextual narration:** Each sim tab describes what to look for referencing actual produce in the test image
- **Richer results:** Collapsible "What does this mean?" panel with prevalence, daily impact, luminance explanation, career notes, and linked academic sources
- **Severity gauge:** Visual progress bar with mild/moderate/severe scale based on response pattern
- **Score fix:** Excludes hidden-digit plates from score denominator (hidden plates have inverted correctness logic)
- **Nothing plate scoring:** "Nothing" is now correctly counted as correct for noise trap plates
- **Slider performance:** Wrapped in `requestAnimationFrame` for smooth 60fps drag
- **Green nav icons restored:** Eye, split, and GitHub icons use `--sui-success` colour
- **Version shield:** OpenColor Labs v3.0.1 shield in footer alongside SUI v2.1.2

### v2.0.0
- 14-plate test sequence (demo, R/G, brightness trap, tritan, hidden-digit, noise trap)
- SUI migration (topbar, cards, progress, shields, alerts, stats)
- Split-screen Reality Simulator with CSS `clip-path` slider
- Daltonisation reveal mode
- SEO: JSON-LD, Open Graph, Twitter Cards
- Mobile: viewport-fit, safe-area-inset, apple-mobile-web-app
- Disclaimer footer

### v1.0.0
- Initial release with 5 plate types
- Basic scoring engine
- SVG filter simulation
- Vanilla CSS dark theme

## Licence

MIT

---

Made in Canada with ❤️
