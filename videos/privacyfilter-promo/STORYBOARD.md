# Storyboard

**Format:** 1920x1080 landscape  
**Duration:** 20 seconds  
**Audio:** English TTS voiceover, restrained interface SFX, no music required for first preview  
**VO direction:** Clear, calm product demo voice. Deliberate pauses after "free forever" and "No upload. No tracking."  
**Style basis:** DESIGN.md, using the captured Privacy Filter UI and exact colors from the site.

## Asset Audit

| Asset | Type | Assign to Beat | Role |
| --- | --- | --- | --- |
| `capture/screenshots/scroll-000.png` | Product screenshot | Beat 1, Beat 2 | Hero interface, text detection console |
| `capture/screenshots/text-detection-workbench.png` | Product screenshot copy | Beat 2 | Duplicate-safe source for the close text-detection crop |
| `capture/screenshots/detected-full.png` | Product screenshot | Beat 1, Beat 2 | Full page after model ready and detection complete |
| `capture/screenshots/detected-text-panels.png` | Product screenshot | Beat 1, Beat 2 | Input and results panels after detection, used for aligned scan/result zoom |
| `capture/screenshots/scroll-038.png` | Product screenshot | Beat 3 | Image OCR workbench and trust bar |
| `capture/screenshots/public-image-scan-section.png` | Product screenshot | Beat 3 | Production privacyfilter.app image OCR section after scan complete, without local Astro toolbar |
| `capture/screenshots/scroll-077.png` | Product screenshot | SKIP | Replaced in Beat 3 to avoid local Astro toolbar in captured video |
| `capture/screenshots/scroll-100.png` | Product screenshot | Beat 4 | FAQ/footer GitHub proof |
| `capture/assets/svgs/icon-0.svg` | SVG icon | Beat 1, Beat 4 | GitHub/open-source accent |
| `capture/assets/svgs/icon-1.svg` | SVG icon | Beat 1, Beat 4 | GitHub/open-source accent |

## Beat 1 - Hook + Trust Setup (0:00-0:04.9)

**VO cue:** "Clean private data before you share it. Privacy Filter is open source and free forever."

**Concept:** The viewer lands inside the actual app, already close enough to read the problem: a text box contains names, emails, phone numbers, account numbers, and a token. The interface feels like a practical workbench, not a marketing page.

**Visual description:** `detected-text-panels.png` fills a large desktop frame, showing the input on the left and the completed results on the right. A crisp blue scanning line sweeps only inside the input text area while the headline appears left: "Clean private data before you share it." The PF mark sits in the top-left with small local-processing status chips drifting around the product frame. Subtle blue corner brackets draw attention to the input text box, not the whole screenshot.

**Assets:** `capture/screenshots/detected-text-panels.png`, `capture/assets/svgs/icon-0.svg`, `capture/assets/svgs/icon-1.svg`

**Animation choreography:** Screenshot slides in from the right and gently zooms. Headline types in by phrase. Scan line draws downward. PF mark stamps in, then holds. Status chips cascade in.

**Transition:** Velocity-matched upward blur into Beat 2.

**Depth layers:** Background `#F6F8FB` grid, midground product screenshot, foreground headline and scan brackets.

## Beat 2 - Local Text Detection (0:04.9-0:10.9)

**VO cue:** "It runs entirely in your browser, detecting names, emails, phone numbers, account numbers, URLs, and secrets."

**Concept:** The promo turns the flat app UI into a local inference path. Data starts in the input, moves through an in-browser model, and comes back as redacted output without leaving the frame.

**Visual description:** The same detected UI zooms closer to the text/results panels. After the input scan completes, the right Results panel enlarges and becomes the hero. Entity chips and blue mask bars align with the completed labeled/redacted output, not the idle state. A small "LOCAL ONLY" route loops from input to model to output; no arrow exits the browser frame.

**Assets:** `capture/screenshots/scroll-000.png`

**Animation choreography:** Entity chips cascade over the text. The local route draws as an SVG path. Redaction bars slide over sensitive snippets. "Open source" and "Free forever" counters snap in as square badges.

**Transition:** Whip pan to the image OCR workbench.

**Depth layers:** Cropped product UI below, SVG route line above, labels and redaction bars in foreground.

## Beat 3 - Text + Image Coverage (0:10.9-0:13.1)

**VO cue:** "Redact text or screenshots locally."

**Concept:** The feature set expands from text to screenshots. The visual world becomes a controlled product tour: categories, OCR, and masking all stay inside the same local browser surface.

**Visual description:** `public-image-scan-section.png` appears as the product panel. The image OCR result panel shows scan complete and correctly aligned blue masks over email, phone, and token lines. Six entity category cards fan into a clean grid: Name, Email, Phone, Account, URL, Secret.

**Assets:** `capture/screenshots/public-image-scan-section.png`

**Animation choreography:** The image workbench slides in. Mask rectangles fill from left to right. Entity cards cascade in a 4x2 rhythm. The screenshot slowly pans behind the cards.

**Transition:** Blur-through into final promise.

**Depth layers:** Screenshot panels in background/midground, entity cards and mask rectangles in foreground.

## Beat 4 - Promise + CTA (0:13.1-0:20.0)

**VO cue:** "No upload. No tracking. Your data stays on your device."

**Concept:** End with the trust promise, not a sales pitch. The interface collapses into three uncompromising claims and a small GitHub/source-code cue.

**Visual description:** The frame resolves to a clean `#F6F8FB` canvas with three square proof tiles: Open source, Free, Local only. Under them, a device-shaped outline contains the data; outbound upload arrows appear and are blocked by a blue shield line. The final lockup shows the PF mark, "Privacy Filter Online", and "privacyfilter.app".

**Assets:** `capture/screenshots/scroll-100.png`, `capture/assets/svgs/icon-0.svg`, `capture/assets/svgs/icon-1.svg`

**Animation choreography:** Proof tiles stamp in on the narration. Upload arrows attempt to move outward but stop at the device outline and fade. Final logo lockup rises into center with a soft blue pulse.

**Transition:** End on held frame.

**Depth layers:** Light background grid, trust tiles, device outline, final brand lockup.

## Production Architecture

```
videos/privacyfilter-promo/
├── index.html
├── DESIGN.md
├── SCRIPT.md
├── STORYBOARD.md
├── narration.wav
├── transcript.json
├── capture/
│   ├── screenshots/
│   ├── assets/
│   └── extracted/
└── snapshots/
```
