# Low-Interference Spatial HUD

The `Low-Interference Spatial HUD` is an AIUI Page-layout profile for single-green transparent glasses. It builds on the color, typography, and component tokens in [Monochrome](/AIUI/design/visual-monochrome) and answers three additional questions: **where UI appears, when it appears, and how much attention it is allowed to occupy.**

It is not a new theme or a standalone Skill. It extends `monochrome-green` for full-screen Pages that present persistent or momentary HUD information.

## Treat the glasses as a window

Glasses are not a phone screen. `480 × 352` is a coordinate and safety boundary for the single-green display, not a rectangle an application must fill. The physical world remains the primary scene; the HUD earns every luminous pixel it adds.

The default is therefore not “make a full-screen app,” but:

- show no HUD while no information needs action;
- put persistent prompts, short actions, and status in the lower field;
- reserve the center for content that genuinely needs inspection;
- allow a full canvas only when game, camera, media, or spatial visualization is the content itself—not generic application chrome;
- remove or quiet the HUD after the task resolves.

## Scope

Use this profile for full-screen Page states such as a pronunciation cue, playback status, pairing code, brief confirmation, menu selection, navigation cue, game status, or spatial content.

Do not automatically use it for conversation-flow cards, document readers, reusable Widgets, or other dense components. They use it only when a particular product state explicitly selects a spatial HUD mode.

## Choose a mode before choosing components

Each stable Page state starts with one placement mode. A mode changes because the task changes, not because more information is available to place.

| Mode | Use when | Default placement | Do not use it to |
| --- | --- | --- | --- |
| `quiet` | No information needs action | no HUD | fill the field with persistent decoration |
| `bottom-primary` | persistent prompt or short action | lower rail + footer | house generic chrome in a center card |
| `lower-center-moment` | pairing code or one-shot confirmation | lower half, centered | leave a page-sized border on screen |
| `top-transient` | noncritical brief state | top-safe strip | cover central task content |
| `center-task` | reading, object, or character | bounded central content | hold a generic status bar or menu wall |
| `immersive-content` | game, camera, or media | full-canvas task content | turn the canvas into an app shell |

### Default: `bottom-primary`

The lower field is the default for persistent HUD. It leaves the primary physical view and central attention available to the real environment and task content, while forming a stable low visual anchor for status, prompts, and actions.

On the `480 × 352` reference canvas, these are iteration-derived **starting anchors**, not a physiological claim about every wearer. Validate them on target hardware, across backgrounds and real tasks, before tuning them:

| Token | Starting value | Purpose |
| --- | --- | --- |
| `hud-gutter-x` | `28px` | left/right inset for lower rail and footer |
| `lower-rail-bottom` | `58px` | lower-rail anchor from the bottom |
| `lower-rail-preferred-max-height` | `100px` | preferred maximum height for a normal stable rail |
| `footer-bottom` | `14px` | footer anchor from the bottom |
| `footer-height` | `18px` | compact footer height |
| `safe-inset` | `16px` horizontal, `12px` vertical | minimum safe field for essential content |

If content does not fit, reduce options, sequence the flow, or intentionally move to `center-task`. Do not grow a generic page-sized card to contain it.

## One attention peak at a time

Transparent monochrome hierarchy cannot rely on a second color; it uses luminance, type, line treatment, and whitespace. The overall amount matters more than any individual token: in a stable state, the wearer should immediately know the single thing worth noticing.

| Element | Normal allowance | Rule |
| --- | --- | --- |
| Bright text or value | 1 | Reserve it for the current key datum, prompt, or action |
| Strong outline | 1 | Identify only the focused item, active target, or bounded task content |
| Local fill | 0–1 | Selection support only; maximum `primary-12` |
| Persistent copy groups | 1 lower rail + 1 footer | Merge, sequence, or reveal other information on demand |
| Decorative guides | minimal | Remove any guide that does not orient, separate, or show progress |

Do not draw every button, option, or setting as an equally bright rounded rectangle. Strong outline, directional marker, and local fill belong to the **current focus**, not the default skin for a wall of options. Focus changes must not shift the layout.

## Good and bad trade-offs

| Situation | Recommended | Not recommended |
| --- | --- | --- |
| Pronunciation / playback | Put the current word and one action in the lower rail; keep time or gesture hint in the footer | Combine library, sync, and page counters at the top with a large center card |
| Pairing | Let the code be the single bright lower-center datum; reduce expiry to a thin line or quiet copy | Wrap one-shot information in a full login card, thick border, and competing actions |
| Menu / settings | Give only the selected item the focus frame or marker; leave other choices open and quiet | Place a row of equally bright boxed settings across the central field |
| Character / pet | A character may own the center because it is task content; keep state/actions below | Add scan rings, radar, gauges, and shell chrome until content becomes a dashboard |
| Game / camera | The scene may fill the canvas because it is content; keep score, volume, and return controls at an edge or bottom | Restore phone-like headers, status bars, and full-screen panels under the excuse of immersive content |

## AIUI implementation boundary

This profile requires no new runtime capability. For an eligible Page:

1. Load base `monochrome-green` tokens first.
2. Then load the profile's placement and attention rules.
3. Use transparent `<view>` groups for the lower rail and footer; use `<card>` only when its navigation semantics fit.
4. Express anchors with supported WXSS `position: relative` / `absolute`.
5. Use `transition` for meaningful state change; do not use CSS `animation`, keyframes, or ambient motion as a spatial-HUD technique.
6. Remove or quiet an overlay through intentional data state after success, timeout, or completion.

```wxss
.page {
  position: relative;
  width: 480px;
  height: 352px;
}

.lower-rail {
  position: absolute;
  left: 28px;
  right: 28px;
  bottom: 58px;
}

.hud-footer {
  position: absolute;
  left: 28px;
  right: 28px;
  bottom: 14px;
  height: 18px;
}
```

This is a clear anchor pattern, not a fixed-size rule for normal Widgets and not an invitation to substitute a phone-web `100vh` plus vertical-centering layout.

## Review on device

For every stable state, check against dark, bright, and visually busy real-world backgrounds while static, walking, and turning the head:

- Is the UI `quiet` when no action is pending?
- Can the wearer name the one current attention peak?
- Is there a task reason for top, center, or full-canvas use? If not, lower or remove it.
- Is there a large green fill, page-sized border, or more than one strong outline? Demote it.
- Is focus singular and stable without a layout jump?
- Are essential text/icons inside the safe field and meaningful without relying on green alone?
- Does the overlay disappear or become quieter after confirmation, timeout, or completion?

A browser's black background is only an approximation of transparent display; it does not replace on-device readability and comfort validation.

## Detailed spec and preview

- [Full source spec for AI and maintainers](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/glasses-hud/design-system-green-spatial-hud.md)
- [Self-contained interactive design page](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/glasses-hud/preview-green-spatial-hud.html)
- [Base monochrome-green system](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/design-system-green.md)
