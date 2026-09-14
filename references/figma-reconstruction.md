# Editable Figma reconstruction

Use this branch for screenshot or screen-recording reconstruction as native Figma design nodes. The reference is the visual target. Preserve its content and layout unless the user requests changes; record possible UX improvements separately rather than applying a redesign during reconstruction.

## Prepare evidence

- For screenshots, record source dimensions, crop boundaries, and the intended viewport. Distinguish image pixels from logical design dimensions: use known device/viewport evidence when available, otherwise state the chosen scale. Do not assume a 2x or 3x capture is a desktop-sized layout.
- For recordings, inspect metadata and extract an overview with an available local video tool such as ffmpeg, then extract sharper frames near meaningful changes. Inspect the frames visually. Select stable endpoints rather than blurred transition frames for reconstruction.
- Keep a compact `reference-map.md` in the task run directory: source path, timestamp when applicable, screen/state name, viewport/crop, observed action, and uncertainties. Deduplicate repeated views of the same state. Distinguish scrolling within a screen from navigation to another screen; do not stitch fixed headers repeatedly or invent off-screen content.
- Retain enough before/after evidence to describe visible dialogs, menus, expanded sections, tabs, and navigation. For long recordings, identify the covered screens from an overview; clarify scope only if it materially changes the work.
- Use keyframes as the visual references for Stage 1. In `AGENTS.md`, separate observed properties from estimated values and proposed layout/component relationships. OCR text is provisional until checked against the source; flag unreadable content instead of silently inventing copy.
- Use the shared Stage 1 design brief and the same task directory for `reference-map.md`, extracted frames, asset notes, and QA. Do not require the user to reattach intermediate artifacts.

## Resolve destination and tools

1. Discover current Figma tools and load `figma:figma-use` before any `use_figma` call. Follow its API documentation, font-loading rules, incremental writes, and error recovery. Discover asset-upload support when images are needed; use its current schema rather than embedding an assumed transport recipe.
2. Reuse the Figma file/page explicitly selected by the user or established task context. When no destination is established, load `figma:figma-create-new-file` before using `create_new_file` to create a reconstruction file. Do not guess a file key or choose an unrelated recent file.
3. Inspect the target's existing pages, components, styles, and variables before writing. Place the reconstruction in a dedicated page or free area unless the user requested an in-place update. Retain returned node IDs in local task notes for subsequent edits and retries.
4. For composed screens, load `figma:figma-generate-design`. Reuse matching accessible library assets when its design-system prerequisites hold. For a blank file without a published design system, record that fact and build local foundations using `figma:figma-use` plus `figma:figma-generate-library`; do not require a library publication just to reconstruct a screen. Load `figma:figma-generate-library` whenever creating reusable components, variants, or token foundations.
5. If tools, authorization, or destination access are unavailable, finish independent reference analysis and report the precise blocker. Do not present a prepared script or HTML substitute as a completed Figma file. Reuse an existing successful file creation on retries.

This branch builds directly from the visual evidence. It does not require HTML, Product Design's ideation gate, or a web-capture tool. If the user also requested HTML, share the reference analysis but verify each requested deliverable separately.

## Rebuild native structure

- Create screen/state frames at the documented logical dimensions. Name layers by function and use a clear hierarchy of screen, region, component, and content.
- Keep ordinary interface text in TEXT nodes with editable content. Verify available font family/style before using it. When the exact font is unavailable, use a suitable available substitute and report it; inspect Chinese glyph coverage, line breaks, width, and line height.
- Build surfaces, borders, dividers, buttons, inputs, and simple graphics as native shapes or vectors. Use Auto Layout for related rows/columns and appropriate fixed, hug, fill, and constraint behavior. Preserve deliberate overlays and free placement where the reference calls for them. A screenshot alone does not prove responsive behavior.
- Reuse compatible components from the target file/library. Build repeated UI as local components and instances when needed, with variants for observed meaningful states. Keep the component set proportional to the requested screens; avoid constructing a speculative full design system.
- Reuse matching tokens/styles where available; otherwise establish the small set needed for consistent colors, typography, spacing, and effects. Label inferred values as reconstruction estimates rather than recovered original tokens.
- Prefer supplied original assets. Use consistent vector icons or faithful simple vector reconstruction. Record approximate replacements when exact assets are unavailable. Do not replace interface text or entire cards with raster crops to disguise missing editability.
- Photos, textures, and complex illustrations can remain replaceable image fills. If only the screenshot is available, crop the necessary asset and disclose its resolution/occlusion limits. Upload using current supported Figma asset tools and check that the fills actually render. Do not use ImageGen to reinterpret a faithful-reconstruction target unless the user requests replacement art.
- Whole-screen reference images may be kept in a clearly labeled reference area. They must be separate from the editable result and excluded from the editability claim.
- Build incrementally, preserving IDs and taking Figma screenshots after major sections. On errors, inspect current state and follow the loaded skill's recovery process before retrying; avoid duplicating successful sections.

## Recording states and interactions

Reconstruct distinct visible states as named frames or suitable component variants. By default, a recording supplies evidence of screens and states; it does not automatically request a full animated prototype.

When the user requests interaction reproduction, connect observed transitions with supported Figma prototype features. Record source and destination states, visible trigger, and any timing estimates. Treat invisible triggers as uncertain. Verify API support before writing reactions; if interactive playback cannot be tested, say so explicitly. Load `figma:figma-use-motion` alongside `figma:figma-use` when implementing or inspecting timeline animation. Do not promise exact recovery of easing, spring physics, hidden states, or app logic from video.

## Validate and deliver

Write a concise `figma-qa.md` in the task run directory. Include destination and frame IDs, source/keyframe mapping, evidence paths, pass/fail/blocked results, and remaining approximations.

**Visual checks:** export or capture the actual Figma frames and inspect them against the matching references at equivalent viewport/crop/scale. Check overall geometry, spacing, text and wrapping, font weight, icon shape, imagery, colors, borders, shadows, clipping, and state coverage. Correct material mismatches before completion. A successful tool call alone is not visual verification.

**Structural checks:** inspect the actual node tree for editable TEXT nodes, meaningful frame hierarchy, appropriate Auto Layout/constraints, reusable component instances and their main components, valid style/variable bindings, and replaceable image fills. For representative repeated components, use a temporary duplicate to check a longer text value or a size change where adaptability is intended; remove the temporary test nodes afterward. Confirm no whole-screen raster is serving as the final design, no content is unexpectedly clipped, and no unfinished placeholders remain.

**Interaction checks, when requested:** verify reaction destinations and exercise supported prototype paths when playback is available. Distinguish structural wiring from tested playback; label untested or unsupported behavior.

Completion requires passed visual and structural checks for the requested screens. If a required check is blocked or a material mismatch remains, describe the result as partial with the outstanding work. Deliver a verified Figma file/frame link, the local QA/brief links, and only consequential limitations. Do not claim a downloadable `.fig` file was produced unless it was actually exported; the normal deliverable is an editable Figma document link.
