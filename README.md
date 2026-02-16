# OpenColor Labs

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![Speyer UI](https://img.shields.io/badge/Speyer_UI-v2.1.2-green)
![Licence](https://img.shields.io/badge/licence-MIT-lightgrey)

**Open source, scientifically transparent colour vision analysis and simulation.**

[Live Demo](https://adrianspeyer.github.io/Open-Colour-Labs/) · [View Source](https://github.com/adrianspeyer/Open-Colour-Labs)

---

## About The Project

Most online colour blindness tests are marketing tools designed to sell expensive "correction" glasses. OpenColor Labs is different. It is a strictly mathematical, open-source tool designed to provide directional analysis and, more importantly, **validation**.

We use procedural generation to create Ishihara-style dot plates that cannot be memorised, and we provide a "Qualia Validator" to let users prove the diagnosis to themselves using their own **qualia** (subjective experience).

---

## Key Features

**Procedural Generation** — Test plates are generated in real-time using HTML5 Canvas. No static images means you cannot cheat by memorising answers. HSL colour jitter ensures no two plates are ever identical.

**The "Qualia" Validator** — A draggable split-screen simulator that uses SVG colour matrices to simulate Protanopia, Deuteranopia, and Tritanopia. If you are colourblind, the "Original" and "Simulated" sides will look identical to you — providing definitive proof of the diagnosis. Drag the divider to compare precisely.

**Scientifically Grounded Simulation** — The simulator uses the Machado, Oliveira & Fernandes (2009) severity=1.0 matrices, the gold standard for dichromacy simulation. These are validated against the Brettel, Viénot & Mollon (1997) model and operate correctly in the SVG `linearRGB` colour space.

**Three-Option Response System** — Users can choose between "I see [number]", "I see no number — just dots", and "I see something, but can't make it out." The distinction between seeing nothing and partial visibility is clinically meaningful: it helps estimate severity (dichromacy vs. anomalous trichromacy).

**Strict Scoring** — Includes "Trap" plates (random noise) to detect guessing and "Brightness" traps to distinguish between Protan (Red-Blind) and Deutan (Green-Blind) deficiencies.

**Reveal Mode** — A daltonisation filter ("Reveal What You're Missing") shifts confused hues into the Blue/Yellow spectrum, allowing colourblind users to see the information they are missing. For normal-vision observers, both sides appear nearly identical — confirming the shift is targeted to the CVD channel.

---

## Usage

This project is a single-file application. No build steps, no servers, no dependencies.

1. Download `index.html` and `test-image.jpg`.
2. Open `index.html` in any modern web browser.
3. That's it.

---

## Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Rendering | HTML5 Canvas | Pixel-perfect, randomised dot generation |
| Simulation | SVG `feColorMatrix` Filters | Machado et al. (2009) colour vision simulation matrices |
| Daltonisation | SVG `feColorMatrix` Filter | R-G opponent signal shifted into Blue channel for Reveal mode |
| Comparison | Draggable Slider | Mouse + touch clip-path comparison with divider handle |
| Design System | [Speyer UI v2.1.2](https://github.com/adrianspeyer/speyer-ui) | Accessible, colour-blind friendly interface with design tokens |
| Icons | [Lucide](https://lucide.dev/) | Lightweight iconography |
| Logic | Vanilla JavaScript | State management and view rendering — zero frameworks |

---

## How It Works

### The Test (10 Plates)

The test sequence includes five plate types, each serving a diagnostic purpose based on pseudoisochromatic plate (PIP) design principles (Hardy, Rand & Rittler, 1945):

| Plate Type | Count | PIP Classification | Purpose |
|-----------|-------|--------------------|---------|
| **Demo (Control)** | 2 | Demonstration | High-contrast plates everyone should see. Validates screen calibration. |
| **Red/Green Confusion** | 2 | Vanishing | Standard Ishihara confusion axis. The number vanishes for both Protan and Deutan observers. |
| **Red/Green Hard** | 2 | Vanishing (desaturated) | Low-saturation variant (~15%). Catches mild anomalous trichromacy that standard plates miss. |
| **Brightness Trap** | 2 | Diagnostic | Dark red on dark grey-green. Protan-specific — protanopes perceive red as very dark due to reduced L-cone luminance sensitivity. |
| **Noise Trap** | 2 | Trap | Pure random noise with no hidden number. Detects guessing. |

### Scoring Logic

The engine uses a strict pipeline with severity estimation:

1. **Invalid (Screen):** Failed demo plates → screen calibration or Night Mode issue.
2. **Invalid (Guessing):** Claimed to see numbers in noise plates.
3. **Normal:** All diagnostic plates correct.
4. **Protan:** Missed brightness trap plates (red appears dark/black).
5. **Deutan:** Missed R/G confusion plates but passed brightness traps.
6. **Severity:** "Nothing" responses suggest dichromacy (severe); "Unsure/partial" responses suggest anomalous trichromacy (mild-moderate).

### The Simulator

The Qualia Validator applies SVG `feColorMatrix` transforms to a reference image (`test-image.jpg`) via a draggable split-screen comparison. The simulation matrices are from Machado, Oliveira & Fernandes (2009), severity=1.0:

- **Protanopia** — L-cone (red) absent. Reds appear dark/black.
- **Deuteranopia** — M-cone (green) absent. Greens and reds collapse together.
- **Tritanopia** — S-cone (blue) absent. Blues and yellows become confused.
- **Reveal** — Custom daltonisation that shifts the red-green confusion axis into blue-yellow, making hidden information visible. Matrix: `B' = 0.5R − 0.4G + B`.

**For colourblind users:** If the "Original" and "Simulated" halves look identical, it confirms the diagnosis. Switch to "Reveal" to see what you've been missing — reds shift blue-ish, greens shift yellow-ish.

**For normal-vision observers:** The simulation side looks dramatically different to you. In Reveal mode, both sides look nearly identical — because the shift targets a channel you already perceive.

---

## Scientific References

This project is grounded in peer-reviewed colour vision research:

| Reference | Used For |
|-----------|----------|
| Machado, G.M., Oliveira, M.M., & Fernandes, L.A.F. (2009). [A Physiologically-based Model for Simulation of Color Vision Deficiency](https://www.inf.ufrgs.br/~oliveira/pubs_files/CVD_Simulation/CVD_Simulation.html). *IEEE Transactions on Visualization and Computer Graphics*, 15(6), 1291–1298. | SVG simulation matrices (severity=1.0 dichromacy) |
| Brettel, H., Viénot, F., & Mollon, J.D. (1997). [Computerized Simulation of Color Appearance for Dichromats](https://pubmed.ncbi.nlm.nih.gov/9316278/). *Journal of the Optical Society of America A*, 14(10), 2647–2655. | Foundational LMS-based simulation model; Machado validates against this |
| Viénot, F., Brettel, H., & Mollon, J.D. (1999). Digital Video Colourmaps for Checking the Legibility of Displays by Dichromats. *Color Research & Application*, 24(4), 243–252. | Simplified protanopia/deuteranopia simulation method |
| Hardy, L.H., Rand, G., & Rittler, M.C. (1945). Tests for the Detection and Analysis of Color Blindness. *Journal of the Optical Society of America*, 35(4), 268–275. | Classification of pseudoisochromatic plate types (vanishing, diagnostic, transformation, hidden-digit) |
| DaltonLens Project. (2021). [Review of Open Source Color Blindness Simulations](https://daltonlens.org/opensource-cvd-simulation/). | Comparative analysis of simulation accuracy across implementations |

---

## Project Structure
```
Open-Colour-Labs/
├── index.html        # Complete application (HTML + CSS + JS)
├── test-image.jpg    # Reference image for the Qualia Validator
├── LICENSE           # MIT Licence
├── CONTRIBUTING.md   # Contribution guidelines
└── README.md         # This file
```

---

## Contributing

We welcome contributions! Specifically, we are looking for:

**Algorithm Improvements** — Tuning the hue and saturation jitter on the confusion plates to better match clinical anomaloscope readings.

**New Plate Types** — Implementing "hidden-digit" (reverse) plates where colourblind users see a number that normal vision cannot, and "transformation" plates where different observers see different numbers.

**Severity Slider for Simulator** — The Machado model provides matrices for severities 0.0–1.0 in 0.1 steps. A slider could let users explore the full range from mild anomalous trichromacy to full dichromacy.

**Matrix Accuracy** — Validating the SVG `linearRGB` pipeline against reference implementations like [DaltonLens-Python](https://daltonlens.org/) or [jsColorblindSimulator](https://github.com/MaPePeR/jsColorblindSimulator).

**Accessibility** — Improving keyboard navigation and screen reader support.

### How to Contribute

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## Changelog

### [1.0.0] — 2026-02-16

**Initial versioned release.** Everything prior was unversioned iteration.

#### Added
- **Procedural Ishihara Plate Generator** — 5 plate types (demo, R/G confusion, R/G hard, brightness trap, noise trap) with 2,000 dots per plate, HSL colour jitter (hue ±8°, sat ±15%, lum ±8°), and rejection sampling for circular dot placement.
- **Three-Option Response System** — Number selection (4 randomised choices), "I see no number — just dots", and "I see something, but can't make it out." Severity estimation from Nothing vs Unsure ratio.
- **Scoring Engine** — Strict pipeline: screen validation → guessing detection → type classification (Normal / Protan / Deutan / Indeterminate) → severity estimation (dichromacy vs anomalous trichromacy).
- **Qualia Validator (Simulator)** — Draggable split-screen comparison with mouse + touch support. Bottom layer: filtered image. Top layer: original image clipped to slider position. Divider handle with `backdrop-filter: blur(4px)`.
- **CVD Simulation Filters** — Machado et al. (2009) severity=1.0 matrices for Protanopia, Deuteranopia, and Tritanopia. SVG `feColorMatrix` in `linearRGB` colour space.
- **Reveal Mode (Daltonisation)** — Custom filter shifting R-G opponent signal into Blue channel (`B' = 0.5R − 0.4G + B`). Colourblind users see colour differences for the first time; normal-vision observers see near-identical sides.
- **Results → Simulator Bridge** — "Launch Verification Sim" button on results page pre-selects the diagnosed type in the simulator.
- **Viewport-Safe Test Layout** — Plate uses `max-height: min(380px, 40vh)` so the entire test view (plate + all 6 buttons) fits on a 1080p screen at 100% zoom without scrolling. Compact button sizing (44px number buttons, 36px special buttons) with reduced spacing.

#### Tech
- Single-file architecture: `index.html` + `test-image.jpg`, zero build steps.
- Speyer UI v2.1.2 for design tokens and components.
- Lucide icons. Vanilla JavaScript with class-based state management.
- Accessible: skip link, ARIA labels, `role="img"`, `aria-pressed`, `focus-visible` outlines, `prefers-reduced-motion` support.

---

## Disclaimer

**This is not a medical device.** This tool generates synthetic approximations of clinical tests. Factors such as screen calibration, ambient lighting, and "Night Shift" modes can significantly affect results. The simulation matrices are educational approximations of dichromacy — individual variation means no simulation is perfectly accurate for every observer. Always consult an optometrist for a comprehensive exam and diagnosis.

---

## Licence

Distributed under the MIT Licence. See `LICENSE` for more information.

---

Built with [Speyer UI](https://github.com/adrianspeyer/speyer-ui). Made in Canada with love. 🇨🇦
