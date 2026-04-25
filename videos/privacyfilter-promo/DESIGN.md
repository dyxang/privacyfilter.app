# Design System

## Overview

Privacy Filter Online is a light, utilitarian web tool for local PII detection and redaction. The visual identity is based on a pale application background, white panels, thin borders, square controls, and dense but readable information layouts. The product screen itself is the primary brand asset: two-column text detection, image OCR panels, trust badges, and entity category cards. The tone is pragmatic, trustworthy, and open-source rather than glossy or decorative.

## Colors

- **Page Background**: `#F6F8FB` — pale blue-gray surface behind all panels.
- **Panel Surface**: `#FFFFFF` — main card, toolbar, and section surface.
- **Input Surface**: `#FBFCFE` — slightly tinted text and image work areas.
- **Primary Text**: `#142033` — headings, body text, and product labels.
- **Muted Text**: `#66758A` — secondary copy, captions, and helper text.
- **Brand Accent**: `#1769E0` — PF mark, primary buttons, highlights, plus icons.
- **Accent Dark**: `#0D4FAF` — active/hover blue for button depth.
- **Border Line**: `#DBE2EC` — panel outlines, dividers, card borders.
- **Soft Code Chip**: `#EEF3FA` — entity code labels and subtle accent blocks.
- **Warning Accent**: `#B7791F` — model loading state and amber progress cues.

## Typography

- **Primary Font**: Inter (400, 500, 600, 700, 800, 900). Used for all UI, headings, labels, and body text.
- **Monospace Font**: SFMono-Regular, Consolas, Liberation Mono. Used for redacted output, tokens, and code-like snippets.
- **Heading Scale**: Hero heading is large and tightly set; section headings are compact, 18px/600 in the source UI.
- **Body Scale**: Product body copy sits around 15-17px with generous line-height for scanability.

## Elevation

The site avoids rounded cards and heavy shadows. Depth comes from thin `#DBE2EC` borders, panel-to-background contrast, white toolbars over tinted work areas, and small color-coded status accents. In the promo, use perspective, parallax, and light shadows only to make the captured UI cinematic while preserving the flat app identity.

## Components

- **Square PF Mark**: A compact blue `PF` block paired with the Privacy Filter wordmark.
- **Two-Column Detection Console**: Input text panel and Results panel with toolbar buttons, labeled source, and redacted output.
- **Demo Selector Strip**: Four compact language buttons with bold labels and muted entity hints.
- **Image OCR Workbench**: Side-by-side image input and result panels with Upload, Debug, Add mask, Download, and Scan image controls.
- **Trust Bar**: Four bordered columns: 100% local, Open model, WebGPU + WASM, Free forever.
- **How-It-Works Steps**: Three bordered process cells with small blue numbered badges.
- **Entity Grid**: Eight flat cards pairing human labels with code chips such as `private_email`, `account_number`, and `secret`.
- **FAQ and Footer CTA**: Minimal accordion rows and a small GitHub star button.

## Do's and Don'ts

### Do's

- Use `#F6F8FB`, `#FFFFFF`, and `#FBFCFE` as the dominant surfaces.
- Keep borders crisp and square with `#DBE2EC`.
- Use `#1769E0` sparingly for trust anchors, scan motion, CTA states, and the PF mark.
- Preserve the product's dense UI feel, but use camera motion and scale to make it readable in video.
- Use monospace snippets for redaction tokens, secrets, and local-processing proof.

### Don'ts

- Do not turn the promo into a dark cyber-security theme.
- Do not use rounded glass cards, gradient-orb backgrounds, or decorative bokeh.
- Do not replace the product UI with generic illustrations.
- Do not imply server processing, cloud upload, pricing tiers, accounts, or tracking.
- Do not use generic stock imagery; the captured interface is the hero visual.
