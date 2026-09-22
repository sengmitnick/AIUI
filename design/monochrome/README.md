# AIUI Monochrome Display Design Language

This subdirectory holds AIUI's design language for **single-color display**
hardware — panels that reproduce a single luminous channel over pure black.
Because the channel hue varies by device, monochrome specs are further
identified by their color (e.g. `green`, `amber`).

## Variants

| Variant | Target devices | Status |
|---------|----------------|--------|
| [`green`](./design-system-green.md) | RokidGlasses1 / RokidGlasses2 | Active |

> The `green` variant is currently the only monochrome spec. Files use a
> `-green` suffix so the current spec stays stable as the monochrome line
> evolves alongside the planned full-color variant.

## Active profiles

Profiles add task-specific rules to a color variant; they are not new hues.

| Profile | Extends | Status | Use |
|---------|---------|--------|-----|
| [`glasses-hud`](./glasses-hud/) | `green` | Active | Placement and attention rules for eligible full-screen transparent-glasses Pages |

## Files

| File | Purpose |
|------|---------|
| [`design-system-green.md`](./design-system-green.md) | Full token spec: colors, typography, spacing, radii, border widths, component chrome, and Do's & Don'ts. The diff-able source of truth. |
| [`preview-green.html`](./preview-green.html) | A self-contained, browsable visual showcase of every token and component. Open it directly in any browser — no build step. |
| [`glasses-hud/`](./glasses-hud/) | Low-interference spatial HUD profile: source rules plus an interactive, self-contained design page. |

## Why a separate monochrome system?

Single-color displays can't lean on hue to express hierarchy, error states,
or data visualization. The `green` variant responds with:

- **One hue, six luminance roles** — hierarchy is green opacity on black,
  never a second color.
- **Outlines, not shadows** — structure comes from clear borders and stable
  whitespace, because drop shadows read poorly on see-through displays.
- **Reference canvas** — `480 × 352` is the coordinate and safety boundary for
  the full-screen display. It is not a visual-fill target; content should use
  only the area its task requires.
- **Errors stay green** — error states use a muted border + faint fill rather
  than red, which the hardware cannot reproduce.

For a full-screen Page that presents persistent or momentary HUD information,
also apply the [`glasses-hud`](./glasses-hud/) profile. It treats the reference
canvas as a coordinate boundary rather than a visual-fill target, defaults
persistent chrome to the lower field, and reserves the center for task content.

See the [Do's & Don'ts](./design-system-green.md#dos-and-donts) section of
the spec for the full set of rules.

## License

Apache License 2.0, inherited from the repository root.
