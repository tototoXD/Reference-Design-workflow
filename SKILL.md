---
name: reference-design-workflow
description: Turn screenshots or screen recordings into a reusable AGENTS.md design brief, then route to image ideation, a verified HTML prototype, or editable Figma reconstruction. Use for end-to-end reference-based design, including restoring captured screens as native Figma layers.
---

# Reference Design Workflow

Run reference analysis and the selected output workflow continuously. Image and HTML outputs use Product Design; editable Figma reconstruction uses the Figma skills and tools. Do not make the user download, attach, or restate the generated design brief between stages.

## Resolve the deliverable

- Respect an explicit request for an image, HTML, website, interface, prototype, editable Figma design, or other format without asking again.
- Infer **Figma** for “还原到 Figma”, “可编辑设计稿”, or native Figma layers from screenshots or screen recordings. Figma prototype requests stay in this branch. A Figma URL used only as an HTML implementation reference does not select this branch.
- Infer **image** for static visual exploration, concept art, posters, key visuals, or a flattened UI mockup.
- Infer **HTML** for usable interfaces, responsive pages, interactive prototypes, or anything the user expects to open and operate in a browser.
- If the intended deliverable is still ambiguous, ask one concise question offering only the plausible choices: image, runnable HTML, or editable Figma. Do not ask when the user already selected a format.
- If the requested subject, product, or page is also missing, combine that missing detail into the same question. Do not conduct a long questionnaire when reasonable defaults are available.

## Stage 1: Build the design context

1. Confirm that at least one reference image or screen recording is attached or available at an accessible local path. If not, ask the user to attach it. For Figma, read [editable Figma reconstruction](references/figma-reconstruction.md) now, including recording preparation before visual analysis.
2. Inspect every relevant reference at useful detail. For recordings, extract and inspect representative stills with an available local video tool; retain timestamps and deduplicate repeated views. If extraction is unavailable, request key screenshots and report the limitation; do not pretend the recording was inspected.
3. Use `image-to-agents-md` to extract the shared visual language, including layout, hierarchy, typography, color, spacing, surfaces, components, imagery, interaction implications, responsive behavior, accessibility risks, and reference-specific anti-patterns.
4. Keep the context scoped to this task:
   - For an existing implementation project, update its root `AGENTS.md` while preserving unrelated instructions.
   - For image-only or Figma-only work, or a task without an existing project, create `design-runs/<descriptive-slug>/AGENTS.md` under the current workspace. Reuse that run directory for all artifacts from the task.
5. Label visual estimates and unseen behavior as proposed defaults. Do not claim exact tokens or pixel-perfect reconstruction from screenshots.
6. Read the resulting `AGENTS.md` before handing off. It is the operational style brief, but it does not replace the images: the selected downstream workflow must also receive and inspect the original images or extracted keyframes, not just the written brief.
7. Record the reference's icon character in `AGENTS.md`: outline or filled, geometric or organic, corner treatment, apparent grid, stroke weight, optical size, active state, and whether any marks are third-party brands.

If `image-to-agents-md` is unavailable, perform the same evidence-based extraction directly and write the scoped `AGENTS.md`; report the fallback at completion.

## Stage 2: Route to the selected output

For **Figma**, follow [editable Figma reconstruction](references/figma-reconstruction.md) after Stage 1. Default to faithful reconstruction of the captured screens; skip Product Design ideation, HTML generation, and hosting. Create new visual directions only if requested.

For **image or HTML**, continue with the Product Design handoff below.

### Hand off to Product Design

After `AGENTS.md` exists, explicitly load and follow `product-design:index`. Run its saved-user-context preflight and mandatory `get-context` brief gate. Treat the user's request, the generated `AGENTS.md`, and the original images as the starting brief so answered questions are not asked again.

Before the focused Product Design workflow begins, play back the target, intended user outcome, selected output format, and consequential defaults in one short note. Continue in the same turn unless Product Design requires the user to select a visual option.

Do not invoke `sites-building` while Product Design owns the task. Product Design controls ideation, template selection, implementation, browser verification, and design QA. Only route to sharing or hosting if the user explicitly requests it.

## Product Design routing

### Icon system

For HTML work, read [icon library selection](references/icon-libraries.md) before implementation. Choose one primary functional icon family that best matches the reference and use its real packaged components or SVG assets. Do not mix functional icon families within one interface; Simple Icons may be added only for third-party brand marks.

For image ideation, select the same target family as the iconography direction and name it in the Product Design brief. When locally available and exact icon shapes materially affect the concept, attach a compact sheet of only the relevant library icons as an additional reference. ImageGen output remains conceptual: never claim its rendered glyphs are exact library assets. If the concept is later built as HTML, replace every functional glyph with the actual library component.

If the requested final deliverable is a flattened UI image and icon fidelity matters, use the high-fidelity render path: follow the HTML workflow, implement actual library components, pass browser design QA, then capture the verified target viewport and return that capture as the image deliverable. Use pure ImageGen image mode for concept exploration, not for promising exact third-party glyphs.

### Motion for HTML

Before HTML implementation, read [motion selection and transitions](references/motion-libraries.md). Choose appropriate transitions for navigation and state changes, reuse the project's animation system, and record proposed motion in AGENTS.md. Use Motion, Anime.js, or Swup only when the interaction warrants that dependency. Include reduced-motion support and interaction testing in Product Design QA.

### Image mode

1. Load and follow `product-design:ideate`; do not call `imagegen` as an unrelated parallel workflow.
2. Pass the original images as actual Image Gen references and incorporate the actionable `AGENTS.md` rules, requested content, dimensions, exact copy, hard constraints, and avoid rules into the Product Design brief.
3. Include the chosen icon family, size, weight, fill/outline behavior, and consistency requirements in every Product Design image prompt. Prefer a small number of familiar symbols with text labels over dense icon-only controls.
4. Product Design generates three independent visual directions by default. If the user requested a different number, that count overrides the default.
5. Present the generated images in the order required by Product Design and stop for selection or refinement. Image mode is complete when the requested visual outputs are visible; do not start an HTML build unless the user asks for one.

### HTML mode

1. Decide whether the supplied image is the exact visual target or only style inspiration.
2. If it is the exact screen to reproduce, load and follow `product-design:image-to-code` directly, using the reference image as the selected visual target and `AGENTS.md` as supporting implementation guidance.
3. If the user wants a new screen or product in the reference style, first load and follow `product-design:ideate`. Show its visual directions and wait for the user to select one; after selection, automatically continue with `product-design:image-to-code` using that displayed result as the exact target.
4. Follow Product Design's prototype initialization, asset generation, interaction, browser-capture, and blocking design-QA requirements. A build is not complete from source inspection or compilation alone.
5. Use the selected icon library's components with explicit imports so unused icons can be removed by the bundler. Apply consistent size, weight, color, alignment, accessible naming, and button hit areas; use text labels for unfamiliar or ambiguous actions.
6. During design QA, compare icon metaphor, family, weight, size, baseline, active state, and visual density against the target—not merely whether an icon is present.
7. Keep the verified local preview open. Do not deploy or publish unless the user explicitly requests sharing.

## Boundaries

- Derive reusable visual principles; do not copy proprietary logos, protected copy, or distinctive assets unless the user supplied them for authorized reuse.
- Existing product requirements and explicit user instructions outrank style inferences.
- Produce only the requested deliverables among image, HTML, and Figma. Internal reference captures and QA artifacts are allowed; do not treat them as extra requested products.
- Do not overwrite unrelated `AGENTS.md` content or existing output files.
- Never claim a downstream workflow received a reference unless the actual image or a readable local image path was passed to it.

## Completion

Report the selected route, the design-brief path, the generated options, verified prototype, or editable Figma link, and only the consequential assumptions that remain. For HTML, completion requires Product Design's `design-qa.md` to pass; for image ideation, completion requires all requested options to be visible and ready for selection.

For Figma, completion requires visual comparison and structural editability checks recorded in `figma-qa.md`, plus a verified file/frame link. Report remaining font, asset, and interaction approximations. A script, whole-screen bitmap, or unverified write is not a completed editable reconstruction.
