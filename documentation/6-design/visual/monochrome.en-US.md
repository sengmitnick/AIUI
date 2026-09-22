# Monochrome

`Monochrome` describes AIUI's visual language for transparent single-color display hardware. The current public variant is `Green` for RokidGlasses1 / RokidGlasses2: every visible pixel is one green channel at a different luminance, and pure black represents the transparent display floor.

The monochrome system answers **how UI looks**: color, type, line treatment, spacing, and component chrome. For **where it belongs, when it appears, and how much attention it may occupy** on a full-screen Page, read [Low-Interference Spatial HUD](/AIUI/design/visual-glasses-hud).

## Principles

- Do not introduce a second color to communicate state. Pair status with copy, icon/shape, line pattern, or interaction state.
- The transparent floor is not an opaque black mask; do not hide the physical environment behind large black panels.
- Use outlines, luminance hierarchy, and stable whitespace—not blurred shadows—to organize information.
- A panel is a local tool for a grouping that materially improves comprehension, not an app shell every page must have.
- Keep essential content inside the safe field of the `480 × 352` reference canvas: `16px` horizontal and `12px` vertical reference insets.
- Let task intent choose the Page mode and component. A navigation `<card>` must not become a universal background container.

## Color and luminance roles

This is not a multi-hue palette; it is one green luminance scale. Names stay aligned with the source spec:

| Token | Typical use | Value |
| --- | --- | --- |
| `primary` / `ink` | current focus, key data, maximum emphasis | `#40ff5e` |
| `primary-72` / `ink-primary` | readable primary copy and active structure | `rgba(64,255,94,0.72)` |
| `primary-48` / `ink-secondary` | secondary copy and normal boundaries | `rgba(64,255,94,0.48)` |
| `primary-24` / `line-muted` | dividers and quiet structural cues | `rgba(64,255,94,0.24)` |
| `primary-12` / `surface-active` | local selected/focused support fill | `rgba(64,255,94,0.12)` |
| `primary-06` / `line-trace` | rare atmospheric trace or subtle surface | `rgba(64,255,94,0.06)` |
| `background` / `surface` | transparent display floor | `#000000` |

In a normal state, local fill never exceeds `primary-12`. Do not use large green fills for headers, default buttons, or page backgrounds.

## Typography, spacing, and boundaries

| Category | Current baseline |
| --- | --- |
| Maximum hierarchy | `display`: `22px`, 500, sans-serif |
| Heading | `heading`: `16px`, 500, sans-serif |
| Body | `body`: `14px`, 400, sans-serif |
| Label / caption | `label`: `11px`; `caption`: `10px` |
| Data | `data`: `13px`, 500, monospace |
| Spacing | `2 / 4 / 8 / 12 / 16 / 24 / 32px` |
| Radius | `0 / 2 / 4 / 6px`, only where local grouping needs it |
| Default border | `1px` |
| Focus border | `2px`, only for the current focus or active target |

Use the “1px normal, 2px focused” hierarchy rather than 2px by default and 4px for emphasis. The number of strong outlines is also constrained by the [Low-Interference Spatial HUD](/AIUI/design/visual-glasses-hud) single-attention-peak rule.

## Component baselines

| Component | Default expression |
| --- | --- |
| `panel` / `card` | transparent or black floor + 1px `line-muted` local boundary + maximum 6px radius; only where grouping improves comprehension |
| `card-highlight` | local `primary-12` fill + strong boundary; never page-wide highlighting |
| `button` | compact outlined action; solid green fill is reserved for irreversible or critical confirmation |
| `text-input` / `textarea` | `primary-06` low fill + normal boundary |
| `list-row` | open row plus fine divider; do not stack every row as a card |
| `status` / `error-state` | green text paired with label, icon/shape, or line pattern; errors do not turn red |

## Do

- Use tokens, typographic hierarchy, whitespace, and fine lines to create hierarchy.
- Limit fill and strong outlines to the current local task or focus.
- Let irrelevant information disappear when it has no task, rather than reserving permanent space.
- Test readability against dark, bright, and visually busy backgrounds on target hardware.
- Choose a spatial HUD mode before choosing components for a full-screen Page.

## Don't

- Do not introduce red, blue, or a second color, and do not rely only on green luminance to convey meaning.
- Do not use page-sized cards, large green fills, shadows, or decorative borders to manufacture an “app” feel.
- Do not draw every menu and settings item as an equally strong rounded box.
- Do not assume browser CSS or font behavior is available in AIUI WXSS.
- Do not treat `480 × 352` as a rectangle UI should fill.

## Source and preview

- [Base monochrome-green source spec](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/design-system-green.md)
- [Base monochrome-green visual preview](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/preview-green.html)
- [Low-interference spatial HUD source spec](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/glasses-hud/design-system-green-spatial-hud.md)
- [Low-interference spatial HUD interactive preview](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/glasses-hud/preview-green-spatial-hud.html)
