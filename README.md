# OpenColor Labs

**Open source, scientifically transparent colour vision analysis and simulation.**

[Live Demo](https://adrianspeyer.github.io/Open-Colour-Labs/) · [View Source](https://github.com/adrianspeyer/Open-Colour-Labs)

---

## About The Project

Most online colour blindness tests are marketing tools designed to sell expensive "correction" glasses. OpenColor Labs is different. It is a strictly mathematical, open-source tool designed to provide directional analysis and, more importantly, **validation**.

We use procedural generation to create Ishihara-style dot plates that cannot be memorised, and we provide a "Reality Simulator" to let users prove the diagnosis to themselves using their own **qualia** (subjective experience).

---

## Key Features

**Procedural Generation** — Test plates are generated in real-time using HTML5 Canvas. No static images means you cannot cheat by memorising answers. HSL colour jitter ensures no two plates are ever identical.

**The "Qualia" Validator** — A split-screen simulator that uses SVG colour matrices to simulate Protanopia, Deuteranopia, and Tritanopia. If you are colourblind, the "Original" and "Simulated" sides will look identical to you — providing definitive proof of the diagnosis.

**Scientifically Grounded Simulation** — The simulator uses the Machado, Oliveira & Fernandes (2009) severity=1.0 matrices, the gold standard for dichromacy simulation. These are validated against the Brettel, Viénot & Mollon (1997) model and operate correctly in the SVG `linearRGB` colour space.

**Three-Option Response System** — Users can choose between "I see [number]", "I see no number — just dots", and "I see something, but can't make it out." The distinction between seeing nothing and partial visibility is clinically meaningful: it helps estimate severity (dichromacy vs. anomalous trichromacy).

**Strict Scoring** — Includes "Trap" plates (random noise) to detect guessing and "Brightness" traps to distinguish between Protan (Red-Blind) and Deutan (Green-Blind) deficiencies.

**Reveal Mode** — A daltonisation filter ("Reveal Hidden Colours") shifts confused hues into the Blue/Yellow spectrum, allowing colourblind users to see the information they are missing.

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
| Design System | [Speyer UI](https://github.com/adrianspeyer/speyer-ui) | Accessible, colour-blind friendly interface with design tokens |
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

The Reality Simulator applies SVG `feColorMatrix` transforms to a reference image (`test-image.jpg`). The matrices are from Machado, Oliveira & Fernandes (2009), severity=1.0:

- **Protanopia** — L-cone (red) absent. Reds appear dark/black.
- **Deuteranopia** — M-cone (green) absent. Greens and reds collapse together.
- **Tritanopia** — S-cone (blue) absent. Blues and yellows become confused.
- **Reveal** — Custom daltonisation that shifts the red-green confusion axis into blue-yellow, making hidden information visible.

If you are colourblind and the "Original" and "Simulated" halves look identical, it confirms the diagnosis from the test.

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
├── test-image.jpg    # Reference image for the Reality Simulator
├── LICENSE           # MIT Licence
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

## Disclaimer

**This is not a medical device.** This tool generates synthetic approximations of clinical tests. Factors such as screen calibration, ambient lighting, and "Night Shift" modes can significantly affect results. The simulation matrices are educational approximations of dichromacy — individual variation means no simulation is perfectly accurate for every observer. Always consult an optometrist for a comprehensive exam and diagnosis.

---

## Licence

Distributed under the MIT Licence. See `LICENSE` for more information.

---

Built with [Speyer UI](https://github.com/adrianspeyer/speyer-ui). Made in Canada with love. 🇨🇦