# **OpenColor Labs**

**Open-source colour vision analysis** — transparent science, community driven, no paid diagnosis.

🔗 **Live Demo:** [adrianspeyer.github.io/Open-Colour-Labs](https://adrianspeyer.github.io/Open-Colour-Labs/)

## **What It Does**

OpenColor Labs is a free, browser-based colour blindness screening tool that uses **22 procedurally generated Ishihara-style plates** to detect and differentiate protan (red-weak), deutan (green-weak), and tritan (blue-yellow) colour vision deficiencies. It includes a **Reality Simulator** with split-screen comparison sliders, contextual narration referencing the actual test image, and daltonisation-based reveal filters.

## **Plate Breakdown**

| Type | Count | In Score | Purpose |
| :---- | :---- | :---- | :---- |
| Demo | 2 | Yes | Screen calibration — if you can't see these, adjust your display |
| Red-green vanishing | 4 | Yes | Standard protan/deutan detection |
| Red-green desaturated | 3 | Yes | Catches subtle/mild deficiencies |
| Brightness trap | 3 | Yes | Protan-specific — exploits L-cone luminance loss |
| Transformation | 3 | Yes | Protan reads one number, deutan reads a different number, normal vision sees both |
| Tritan | 2 | Yes | Blue-yellow confusion axis |
| Hidden-digit | 2 | No | CVD observers see the number, normal vision doesn't — positive confirmation |
| Noise trap | 3 | Yes | Pure random dots — detects guessing |

**Scoring:** 20 plates contribute to the displayed score. Hidden-digit plates are excluded because their correctness logic is inverted (seeing the number \= CVD confirmation, not a correct answer in the traditional sense). Transformation plates contribute to the score and provide separate protan vs deutan signals based on which number(s) the user identified.

## **Features**

### **Scoring Engine**

* Protan vs deutan differentiation using transformation plates \+ brightness response  
* Severity estimation: anomalous trichromacy (mild) vs dichromacy (severe)  
* Hidden-digit confirmation when applicable  
* Noise trap validation to flag unreliable results

### **The "Qualia" Validator — Reality Simulator**

* **Dynamic Severity Interpolation:** Maps your test diagnosis severity to a matrix modifier, with manual 0% to 100% sliding adjustments.  
* **3 simulation tabs:** Protanopia, Deuteranopia, Tritanopia — each with a draggable split-screen slider  
* **Reveal tab:** Toggle between "Your Vision" and "Colours Revealed" using type-specific daltonisation filters  
  * Protan reveal: shifts red luminance \+ R-G hue into blue channel  
  * Deutan reveal: shifts R-G hue distinction into blue channel  
  * Tritan reveal: shifts B-Y distinction into red-green channel  
* **Contextual narration:** Each tab speaks directly to the CVD user about what they'll see in the actual test image  
* **Slider performance:** 120fps smooth scrolling utilizing requestAnimationFrame, will-change CSS hardware acceleration, and touch-action overrides.

### **Results Detail Panel**

* Collapsible "What does this mean?" section with:  
  * Prevalence statistics (per-type)  
  * Daily impact descriptions  
  * Protan vs deutan luminance explanation  
  * Career and occupational notes  
  * Linked academic sources (Alhazmi 2025, Gobira et al. 2025, Global CVD Meta-Analysis 2025\)

## **Tech Stack**

| Component | Technology |
| :---- | :---- |
| UI Framework | [Speyer UI v2.1.2](https://github.com/adrianspeyer/speyer-ui) |
| CVD Simulation | SVG feColorMatrix — Machado et al. (2009) interpolated in linearRGB |
| Daltonisation | Custom SVG feColorMatrix filters per CVD type (Fidaner et al. 2005 inspired) |
| Plate Generation | Procedural canvas — Ishihara mechanism with mixed-hue backgrounds and ±12 lightness jitter |
| Slider | CSS clip-path: inset() hardware-accelerated with GPU Compositing overrides |
| Icons | Lucide (pinned with onload callback) |
| SEO | JSON-LD (WebApplication \+ FAQPage), Open Graph, Twitter Cards |
| Mobile | viewport-fit=cover, safe-area-inset-\*, apple-mobile-web-app-capable |

## **Scientific References**

* **Machado, S., Oliveira, M., & Fernandes, L. (2009).** *A Physiologically-based Model for Simulation of Color Vision Deficiency.* IEEE TVCG, 15(6), 1291–1298.  
* **Fidaner, I.B., Ozguven, N., & Sahin, U. (2005).** *Analysis of Color Blindness.* Istanbul — daltonisation algorithm basis.  
* **Gobira, C. et al. (2025).** *Digital Ishihara Testing Validation.* 96.4% sensitivity, 99.3% specificity vs physical plates.  
* **Alhazmi, A. (2025).** *A Global Perspective of Color Vision Deficiency: Awareness, Diagnosis, and Lived Experiences.* Healthcare, 13(16), 2031\.  
* **Global CVD Meta-Analysis (2025).** *Global Prevalence of Congenital Color Vision Deficiency among Children and Adolescents, 1932–2022.* Ophthalmology.

## **Disclaimer**

This is an **educational tool**, not a medical device. It does not replace professional diagnosis. Screen calibration, ambient lighting, and display settings affect results. Consult a qualified optometrist for a definitive colour vision assessment.

## **Changelog**

### **v3.1.2 — Calibration Narration Update**

* **Copy Rewrite:** Updated the simulator descriptions to lean into the mathematical finding that CVD users will perceive "no difference" exactly when the slider matrix severity matches their physical physiological severity. The narrative now guides users to use the slider as a literal calibration tool to find their precise deficiency percentage.

### **v3.1.1 — Hardware-Accelerated Slider**

* **GPU Compositing:** Added the CSS will-change: clip-path and will-change: left properties to the .cmp-top image and .cmp-divider handles. This simple declaration tells the browser's rendering engine to place these animated elements on their own compositor layer, bypassing layout-thrashing on the main thread and delivering a perfectly smooth 60fps/120fps sliding experience.

### **v3.1.0 — Dynamic Severity Engine**

* **Test-linked matrix interpolation:** The simulator now dynamically blends the Machado matrices in real-time. If a user scores "Mild," the simulator automatically loads a mathematically interpolated 50% severity matrix.  
* **Interactive severity slider:** Added an inline range slider granting manual control (0% to 100%) to match the exact severity of the visual perception gap.  
* **Contextual copy overhaul:** Corrected narrative assumptions about CVD. Text now explicitly outlines what severe vs. mild anomalous trichromats will experience while testing the simulator, addressing the "muddy" double-filtered effect for mild cases.

### **v3.0.5 — Mobile Slider QA Fix**

* **Mobile touch interaction:** Added native touch-action: none and \-webkit-user-drag: none to the comparison slider to reliably prevent the browser from interpreting horizontal drags as swipe-to-go-back or page scroll gestures.  
* **Simulator Accuracy Verified:** Rejected a dev patch that attempted to invert the simulation filters. The application maintains its scientifically accurate rendering where "Normal Vision" is strictly unfiltered and "Simulation" applies the matrix. Double-filtering for CVD observers is correctly avoided.

### **v3.0.4 — Hidden Plate Fix & Results Reorder**

* **Hidden plate fix:** Background now includes red+green dots alongside blue/yellow/purple so normal vision can't spot the figure by hue family alone. Previously the figure (red+green only) was trivially visible against a background with zero red or green.  
* **Results flow reordered:** "What does this mean?" detail panel now appears before the Simulator CTA — users get context on their result before being asked to verify it.

### **v3.0.3 — Expanded Plate Sequence**

* **22 plates** (up from 18): added 1 red-green, 1 red-green desaturated, 1 brightness trap, and 1 transformation plate for stronger diagnostic confidence  
* **Diagnostic coverage raised:** 13 scoring plates plus 3 transform signals — reduces false negatives and provides more data for protan vs deutan differentiation  
* **Ishihara mechanism implemented:** Red-green plates use mixed red+green backgrounds at 50–65% saturation with ±12 lightness jitter across all dots to eliminate luminance cues — only hue distinguishes figure from ground  
* **Tritan plates cleaned:** Use only blue (230°) and yellow (55°) hues with no red or green involvement to prevent false tritan flags on red-green CVD users  
* **Dark mode locked:** Triple-set data-theme="dark" to prevent SUI JS from switching to system light preference

### **v3.0.2 — Plate Calibration & Scoring Fix**

* **Critical scoring bug fixed:** Transform plate signals were inverted — seeing the red number (numP) was incorrectly counted as a protan signal when it actually indicates deutan (can see red \= green-blind). Now correctly mapped.  
* **Hardened plate colours:** All plate types use precisely matched saturation and lightness between figure and ground. Only hue differs on the target confusion axis.  
* **Transform plate overhaul:** Background saturation raised, digit saturation lowered, dot density increased from 2,200 to 2,800  
* **Tighter jitter:** Lightness jitter reduced from ±8 to ±5, saturation jitter from ±15 to ±10 to prevent random luminance edges at number boundaries  
* **Combo button order:** Transform combo shows digits in visual order (left & right as drawn on plate) instead of numerically sorted

### **v3.0.1 — QA & UX Fixes**

* **Transform plate multi-select:** Options include combo answers (e.g. "5 & 2") alongside singles with decoy combos, so users who see both numbers can select them naturally without revealing that two numbers exist  
* **Sim descriptions rewritten:** Narration speaks directly to the CVD user ("This is for you") with explicit note that someone without CVD sees no difference  
* **Reveal crossfade:** In-place opacity crossfade instead of full DOM re-render — no image flicker  
* **Green header fix:** .brand-green class \+ sui-brand-mark background override so SUI topbar styles don't swallow the green accent  
* **Diagnosis engine:** "Both" answers on transform plates excluded from protan/deutan scoring — seeing both numbers indicates normal vision

### **v3.0.0 — Major Release**

* **18 plates** (up from 14): added 2 transformation plates, 1 extra R/G vanishing, 1 extra noise trap  
* **Transformation plates:** Protan sees digit A, deutan sees digit B — reliable differentiation  
* **Tab-based simulator:** 4 tabs (Protanopia, Deuteranopia, Tritanopia, Reveal) replacing sidebar layout  
* **Reveal mode redesign:** Toggle between "Your Vision" and "Colours Revealed" with type-specific daltonisation filters  
* **Contextual narration:** Each sim tab describes what to look for referencing actual produce in the test image  
* **Richer results:** Collapsible "What does this mean?" panel with prevalence, daily impact, luminance explanation, career notes, and linked academic sources  
* **Severity gauge:** Visual progress bar with mild/moderate/severe scale based on response pattern  
* **Score fix:** Excludes hidden-digit plates from score denominator  
* **Nothing plate scoring:** "Nothing" correctly counted as correct for noise trap plates  
* **Slider performance:** requestAnimationFrame for smooth 60fps drag  
* **Green nav icons:** Eye, split, and GitHub icons use \--sui-success colour  
* **Version shield:** OpenColor Labs \+ SUI shields in footer

### **v2.0.0**

* 14-plate test sequence (demo, R/G, brightness trap, tritan, hidden-digit, noise trap)  
* SUI migration (topbar, cards, progress, shields, alerts, stats)  
* Split-screen Reality Simulator with CSS clip-path slider  
* Daltonisation reveal mode  
* SEO: JSON-LD, Open Graph, Twitter Cards  
* Mobile: viewport-fit, safe-area-inset, apple-mobile-web-app  
* Disclaimer footer

### **v1.0.0**

* Initial release with 5 plate types  
* Basic scoring engine  
* SVG filter simulation  
* Vanilla CSS dark theme

## **Licence**

MIT

Made in Canada with ❤️