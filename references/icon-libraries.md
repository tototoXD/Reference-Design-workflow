# Icon library selection

Choose by visual and product fit, not GitHub stars alone. Stars below are approximate discovery signals observed on 2026-09-12; verify current documentation and license before shipping a redistributed icon set.

## Preferred libraries

| Library | Approx. GitHub stars | License | Best fit | Typical React package |
|---|---:|---|---|---|
| [Material Symbols](https://github.com/google/material-design-icons) | 53.8k | Apache-2.0 | Material products, dense application UI, variable fill/weight/grade | Use the project's supported Material Symbols integration or the official Google Fonts path |
| [Lucide](https://github.com/lucide-icons/lucide) | 23.8k | ISC | Restrained, minimal outline interfaces | `lucide-react` |
| [Heroicons](https://github.com/tailwindlabs/heroicons) | 23.8k | MIT | Tailwind-oriented products needing outline and solid states | `@heroicons/react` |
| [Tabler Icons](https://github.com/tabler/tabler-icons) | 21.2k | MIT | Geometric dashboards and tool-heavy interfaces; large catalog | `@tabler/icons-react` |
| [Phosphor](https://github.com/phosphor-icons) | 7.2k homepage; 1.7k React | MIT | Expressive interfaces needing thin, regular, bold, fill, or duotone weights | `@phosphor-icons/react` |
| [Simple Icons](https://github.com/simple-icons/simple-icons) | 25.8k | CC0 project; individual brand rights may apply | Third-party brand and service marks only | `simple-icons` |

Do not use Remix Icon as a default shared-library recommendation. Its 2026 custom license permits ordinary product use but adds restrictions for redistribution, competing icon libraries, and brand identity use. Select it only when the project explicitly accepts that license.

## Selection rules

1. Inspect the reference before choosing. Match grid, stroke, terminal shape, corner radius, fill behavior, and visual density.
2. Reuse an existing project icon library when it already matches reasonably well. Do not add a second family for a marginal visual improvement.
3. Otherwise choose one primary family:
   - Material-like or adjustable filled UI: Material Symbols.
   - Neutral geometric dashboards with broad coverage: Tabler.
   - Tailwind UI with clear outline/solid state pairs: Heroicons.
   - Expressive consumer UI or duotone needs: Phosphor.
   - Sparse minimalist outline UI: Lucide, only when it is the closest match rather than as a default.
4. Use Simple Icons only for the represented third-party brand. Do not use a brand glyph as the product's own logo, and check the brand owner's trademark rules.
5. Import only named icons used by the interface. Do not import an entire icon namespace when tree-shaking or compile performance would suffer.
6. Preserve the chosen family's native geometry. Adjust size, color, and supported weight properties; do not distort paths or combine pieces from different libraries to imitate a new icon.

## Implementation and QA

- Establish a small semantic icon map such as `navigation.home`, `action.search`, and `status.warning` so the same meaning uses the same glyph everywhere.
- Use a consistent optical size, normally 20 or 24 CSS pixels for primary UI controls, unless the reference clearly supports another scale.
- Use `currentColor` or the library's standard color API. Do not hardcode unrelated colors inside individual glyphs.
- Maintain at least a 44×44 CSS-pixel pointer target for primary touch controls when the surrounding product rules require touch accessibility; the glyph itself can remain smaller.
- Decorative icons should be hidden from assistive technology. Icon-only controls require an accessible name and, when the metaphor is unfamiliar, a visible label or tooltip.
- QA the actual rendered library icons in the browser at the target viewport. Reject mixed stroke weights, inconsistent fill states, poor baseline alignment, unclear metaphors, and icons that are visibly softer or denser than the reference.
