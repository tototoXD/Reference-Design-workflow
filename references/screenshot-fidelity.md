# Screenshot fidelity protocol

Read this file only when a supplied screenshot or mockup is the exact target for HTML, or when the user explicitly asks for 1:1, pixel-accurate, or measured fidelity. Skip it for style inspiration and image ideation.

## Choose the fidelity mode

- **Faithful reconstruction** is the default exact-target mode. Match composition, hierarchy, geometry, typography, color, assets, states, and responsive behavior, with browser-based visual QA.
- **Pixel-validated clone** adds normalized measurement, repeatable screenshots, region comparisons, and recorded residual differences. Use it only when explicitly requested.

Do not promise access to the source design's original tokens. A screenshot exposes rendered pixels after scaling, rasterization, compression, color management, and device chrome.

## Normalize the reference

Preserve the highest-resolution source. Before measuring, distinguish app-owned content from template-owned material such as device frames, status bars, browser bars, watermarks, captions, or publisher footers. Crop only the material that is outside the requested implementation scope.

Record a `capture.json` beside the run artifacts:

```json
{
  "source": "reference.png",
  "source_pixels": [1179, 2556],
  "crop": [0, 0, 1179, 2556],
  "target_css_viewport": [393, 852],
  "capture_scale": 3,
  "color_space": "sRGB",
  "state": "attachment sheet open"
}
```

Use `null` for unknown values and state the assumption in `design-qa.md`. Cross-check the width and height scale independently; inconsistent ratios can reveal cropping, browser chrome, or a wrong viewport. Inspect or normalize color space when the reference and browser render show a systematic color shift.

## Keep evidence separate from design language

`AGENTS.md` remains the semantic design brief: visual principles, component rules, behavior, and accessibility. Store capture facts in `capture.json`. When numeric sampling or bounds detection is used, store reproducible probes in `probes.json`:

```json
[
  {
    "id": "sheet-fill",
    "image": "reference.png",
    "method": "flat-fill-sample",
    "region": [48, 1080, 1080, 900],
    "sanity": "Inside the sheet; excludes text, dividers, and shadow"
  }
]
```

Every measured value must identify its image, region or coordinates, method, and a sanity note. Treat sampled values as evidence for the rendered target, not automatically as reusable source tokens.

## Measure in visual-impact order

1. Crop, viewport, density, and UI state.
2. Large surfaces and overlays.
3. Major component bounds and anchors.
4. Grid, gaps, padding, dividers, and radii.
5. Typography, line height, weight, and exact wrapping.
6. Icons, images, and brand assets.
7. Shadows, subtle borders, antialiasing, and other polish.

Build one implementation token block from the evidence, ordered as surfaces, lines, ink, accents, radii, typography, then layout metrics. Reuse those values consistently. In an existing codebase, translate them into the project's established tokens and component conventions; do not introduce a parallel styling architecture merely for the clone.

## Reconstruct assets responsibly

- Rebuild interface geometry with HTML and CSS.
- Reuse original or user-supplied brand assets when authorized.
- Use cropped raster assets only for genuine photography, illustration, texture, or similarly irreducible content—not for whole interface regions.
- Use the selected packaged icon family for functional icons. If an exact proprietary glyph is unavailable, choose the closest legitimate asset and record the residual mismatch instead of tracing a copied icon or inventing an inconsistent one.

## Render and compare

Render the implementation at the recorded CSS viewport, state, and density. If the implementation environment adds its own device chrome, compare the app-owned content region separately rather than penalizing unrelated chrome.

For each iteration:

1. Capture the full viewport and check clipping, overflow, scroll position, and text wrapping.
2. Compare the full view for global alignment.
3. Compare named high-value regions independently, such as header, hero, card grid, modal, navigation, or footer.
4. Fix in the same geometry-first order used for measurement.
5. Re-render after every meaningful batch of changes.

Pixel diff, MAE, match rate, edge comparison, and overlays are useful evidence when available, but no single metric is a universal pass/fail gate. Large flat backgrounds can dominate a score while icons or text remain visibly wrong. Pair numerical evidence with visual inspection and region-level checks.

If `refkit` is available, use its grid, sample, bounding-box, scan, font, screenshot, diff, and token-report capabilities where appropriate. Otherwise use browser screenshots plus available local image inspection and comparison tools; the workflow must remain usable without `refkit`.

## Stop condition

The exact-target run is complete when:

- the intended viewport and state are reproducible;
- no actionable high- or medium-severity visual mismatch remains;
- important regions align visually, not only statistically;
- exact copy and intended line wrapping match;
- there is no unintended clipping, overflow, or missing interaction state;
- `design-qa.md` links the reference and rendered captures, records the comparison conditions, and lists unavoidable asset, font, platform, or rasterization differences.

Call the result **pixel-validated** only when this normalized evidence exists. Otherwise describe it as a verified faithful reconstruction.
