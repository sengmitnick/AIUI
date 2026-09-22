---
version: beta
name: Rokid AIUI Monochrome-Green Low-Interference Spatial HUD
name-zh: Rokid AIUI 单绿低干扰空间 HUD
extends: "../design-system-green.md"
target:
  devices: ["RokidGlasses1", "RokidGlasses2"]
  display: "single-green monochrome transparent display"
  reference-canvas: "480x352"
  page-profile: "full-screen Pages that present persistent or momentary HUD information"
source-of-truth: true
---

# Monochrome-Green Low-Interference Spatial HUD

> **中文名：单绿低干扰空间 HUD。** This is a spatial-placement profile for
> transparent AR glasses. It extends the base
> [`monochrome-green`](../design-system-green.md) visual language; it is not a
> new color theme and it does not replace the component system.

## 0. Core rule / 核心规则

**The glasses are a window, not a phone screen.** Treat `480 × 352` as the
coordinate and safety boundary for a transparent display—not as a rectangle
that must be filled.

Default to showing the smallest amount of information, in the least intrusive
location, for the shortest useful duration. The real world remains the primary
scene; the HUD earns every luminous pixel it adds.

This profile is intended for full-screen Page layouts such as a persistent
prompt, timer, pairing code, short decision, navigation cue, or game status.
It is **not** a requirement for dense conversation cards, document readers,
or reusable widgets whose product context needs another arrangement.

## 1. Non-negotiable constraints / 不可突破的约束

1. **No application-sized frame.** Do not draw a full-canvas card, border, or
   opaque background merely to make a page feel complete.
2. **Transparent by default.** `#000000` represents the transparent floor.
   Do not use a black panel to hide the physical scene.
3. **One attention peak.** A normal state has one primary datum, one primary
   action, or one strong focus outline—not all three competing at once.
4. **No repeated inactive chrome.** Do not give every menu option a bright
   rounded rectangle. A focus indicator belongs to the current target only.
5. **Lower placement is the default for persistent UI.** Keep the upper field
   open unless the task is explicitly transient, reading-oriented, or
   immersive content.
6. **Fill is an exception, never the layout.** Obey the base system's maximum
   local `primary-12` fill in normal selection states. Never use a large green
   slab as a default header, button row, or background.
7. **A center object must be task content.** A character, camera subject,
   picture, game world, code, or reading passage may be centered. Navigation
   chrome, status racks, and generic containers may not claim the center just
   because it is available.
8. **Every transient must resolve.** A cue, confirmation, or progress affordance
   disappears or reduces to a quieter state after it has done its job.

## 2. What this profile changes

The base monochrome system defines hue, luminance, type, borders, and component
chrome. This profile adds **where**, **when**, and **how much** UI should be
visible.

| Question an implementer must answer | Base `monochrome-green` | This spatial HUD profile |
| --- | --- | --- |
| How is hierarchy expressed? | One green channel, luminance, type, lines, whitespace | Preserve that hierarchy while limiting visual mass and attention peaks |
| What is a selected state? | Local `primary-12` tint and a strong line | Exactly one selected/focused target receives that treatment |
| Where does persistent UI live? | Inside the safe canvas | In the lower rail by default; top and center need a task reason |
| Can a Page use a panel? | Yes, when grouping materially helps | Yes, but no page-sized panel or decorative box around open HUD content |
| How does UI leave? | Base transition tokens | Return to a quieter state; do not leave a completed overlay glowing |

## 3. Placement modes / 布局模式

Choose one mode for a Page state before choosing components. Do not combine
multiple modes unless the task itself changes.

| Mode | Use when | Default placement | Must remain open | Examples |
| --- | --- | --- | --- | --- |
| `quiet` | No decision or live information needs display | No HUD | Entire field | idle, listening, waiting |
| `bottom-primary` | Persistent status, short prompt, one action, or a compact control rail | lower rail + footer | center and top | pronunciation cue, playback state, small menu |
| `lower-center-moment` | A brief code, single confirmation, or one time-sensitive datum needs direct attention | lower half, centered, no enclosing page card | top and side field | pairing code, one-shot confirmation |
| `top-transient` | A noncritical, short-lived state should be noticed without covering task content | top-safe strip | center/lower task space | sync indicator, brief connection cue |
| `center-task` | The content itself needs inspection or reading | bounded central content | surrounding field and exit affordance | short text, image, semantic object, character |
| `immersive-content` | The product is a game, camera, media, or spatial visualization | full canvas is the content | controls/status stay at edges or bottom | runner game, camera preview, map |

### 3.1 Default anchors for `bottom-primary`

The following tokens are a proven starting arrangement on the `480 × 352`
reference canvas. They are **layout anchors**, not a claim that every wearer has
identical comfort needs. Validate them on-device and tune only with a task
reason.

```yaml
spatial-tokens:
  canvas: { width: 480px, height: 352px }
  safe-inset: { x: 16px, y: 12px }
  hud-gutter-x: 28px
  lower-rail-bottom: 58px
  lower-rail-preferred-max-height: 100px
  footer-bottom: 14px
  footer-height: 18px
  primary-focus-count: 1
  normal-surface-fill-max: "primary-12"
```

The lower rail is for task-relevant information and the current action. The
footer is for terse persistent metadata (for example, elapsed time, a gesture
hint, or state label). If a screen cannot fit in the lower rail, first reduce
the number of items, split the flow into states, or move into `center-task`;
do not grow a generic full-screen card.

### 3.2 Compact coordinate recipe

Use a relative Page canvas and absolute child regions. `position: relative` and
`position: absolute` are supported in AIUI WXSS; this avoids treating a phone
viewport as the visual model.

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

This is a layout pattern, not a mandate to hard-code all content. Preserve the
base safe inset and put essential text inside it. Do not replace the pattern
with `100vh` plus generic vertical centering for a normal HUD.

## 4. Attention budget / 注意力预算

Think in luminous objects, not components. The table is a review checklist for
each stable Page state.

| Element | Normal allowance | Rule |
| --- | --- | --- |
| Bright (`primary`) value or label | 1 | Reserve for the thing the wearer must notice now |
| Strong outline (`line-strong`) | 1 | It identifies the focused menu item, active target, or bounded task—not every item |
| Filled local surface | 0–1 | Maximum `primary-12`; selection support only |
| Persistent text groups | 1 lower rail + 1 footer | Merge or sequence anything else |
| Decorative guides/dividers | Minimal | Remove them when they do not orient, separate, or communicate progress |
| Motion/transition | State change only | Never make ambient movement compete with the world |

### Luminance roles

Use the base token names exactly. Do not introduce a second semantic color.

| Role | Token | Spatial use |
| --- | --- | --- |
| Current focus / key number | `primary` / `ink` | one target, short-lived completion, critical code digit group |
| Readable active copy | `primary-72` / `ink-primary` | title, action label, current guidance |
| Supporting information | `primary-48` / `ink-secondary` | status, secondary label, inactive text |
| Structural hint | `primary-24` / `line-muted` | divider, progress baseline, quiet boundary |
| Local selection support | `primary-12` / `surface-active` | a small focused region only |
| Decorative trace | `primary-06` / `line-trace` | rare and removable; never a layout substitute |

## 5. Focus and selection / 焦点与选择

Use the **single-focus framework** for lists, menus, and settings:

1. Render inactive options as text, an icon, or a quiet divider—not as a row of
   equally bright boxes.
2. Give the selected option the only strong outline, directional marker, and
   optional `primary-12` local fill.
3. Keep the item footprint stable while focus changes; do not make the layout
   jump or make surrounding objects flash.
4. When one choice is confirmed, remove the chooser or reduce it to a compact
   result. Do not keep all controls illuminated.

### Do / do not

| Do | Do not |
| --- | --- |
| Highlight one current setting, reveal adjacent choices quietly | Place every setting in a bright rounded rectangle across the field |
| Use a 1px timing/progress line plus a short label | Build a large bordered card solely to contain a timer |
| Put a current word, prompt, or action in the lower rail | Center a generic app shell because the canvas is available |
| Let a pet, game world, or inspected object own the center | Surround a center object with radar rings, scan lines, and unrelated system chrome |
| Use one state transition after a meaningful action | Run looping CSS animation as ambient decoration |

## 6. Task patterns / 常见任务模式

### A. Persistent pronunciation or playback cue

- **Mode:** `bottom-primary`.
- **Lower rail:** current word or title + one action / small state.
- **Footer:** elapsed time, gesture hint, or compact status.
- **Avoid:** top library counters, sync labels, and a large center frame when
  the word itself is the content.

### B. Pairing code or one-shot confirmation

- **Mode:** `lower-center-moment`.
- **Content:** code is the bright datum; expiry is a quiet line or label.
- **Exit:** auto-dismiss after success/timeout and return to `quiet` or the
  lower rail.
- **Avoid:** a page-sized login card, permanent border, or competing actions.

### C. Menu or settings

- **Mode:** normally `bottom-primary`; escalate to `center-task` only when the
  user explicitly enters a deeper configuration task.
- **Content:** one active row receives the focus frame; surrounding choices are
  open and quiet.
- **Avoid:** horizontally packing many bordered tiles in the central field.

### D. Character / cyber pet

- **Mode:** `center-task`.
- **Content:** the pet may own the central visual attention because it is the
  product's semantic object.
- **Chrome:** commands and status remain in a lower rail; idle mode returns to
  `quiet` or a minimal state.
- **Avoid:** turning the character scene into a dashboard of ornamental
  scanners, rings, meters, and frames.

### E. Game, media, camera, or spatial visualization

- **Mode:** `immersive-content`.
- **Content:** full canvas is allowed only because it is task content.
- **Chrome:** score, volume, instructions, life, and controls should be terse,
  edge-aligned, and preferably lower or on demand.
- **Avoid:** treating an immersive scene as permission to restore phone-like
  headers, status bars, and app-sized panels.

## 7. Component choice / 组件选择

Use the AIUI component that describes the job; do not select a visual component
to manufacture chrome.

| Need | Preferred primitive | Notes |
| --- | --- | --- |
| Open HUD grouping / anchored region | `<view>` | Use transparent layout grouping; add only necessary line treatment |
| Navigation destination / list tile | `<card>` when its navigation semantics fit | Do not use cards as universal Page background containers |
| One compact action | `<button>` or focusable `<view>` | Outlined by default; fill remains rare and local |
| Static reading value | `<text>` | Make the content hierarchy, not a surrounding rectangle, carry priority |
| Timing / progress | thin `<view>` line + label | Keep it subordinate to the primary datum |

## 8. Runtime guardrails / AIUI 实现边界

- Use AIUI's supported WXSS layout primitives (`flex`, `grid`, `relative`,
  `absolute`, `fixed`) according to the current AIUI reference. This profile's
  default recipe uses `relative` + `absolute` because it makes the HUD anchors
  explicit.
- Use `transition` for a meaningful state change. Do **not** prescribe CSS
  `animation`, keyframes, sticky layout, or unsupported text-overflow behavior
  as a spatial-HUD technique.
- Keep state in the page/component data model. A visual reduction after success
  should be a deliberate render state, not an ornamental effect.
- Test on the target hardware; browser black is only a transparent-display
  approximation.

## 9. Review protocol / 设计验收

Review each stable state, not only the happiest screenshot.

1. **State:** Is the UI quiet when no decision is pending? Can the wearer name
   the one thing that deserves attention?
2. **Placement:** Why is this at the top, center, or full canvas rather than
   the lower rail? If there is no task reason, move it down or remove it.
3. **Mass:** Is there a large fill, a page-sized border, or more than one strong
   outline? Remove or demote it.
4. **Focus:** Does only the active control receive the strong treatment? Does
   focus change without layout shift?
5. **Environment:** Check against dark, bright, and visually busy real-world
   backgrounds while static, walking, and turning the head.
6. **Readability:** Check essential text and icons inside the safe field. Do
   not rely on green hue alone for meaning.
7. **Exit:** After confirmation, timeout, or task completion, does the overlay
   disappear or become quieter?

## 10. Rationale and evidence / 设计依据

This profile is derived from iteration patterns in AIUI applications, not from
a claim that one coordinate fits every wearer:

- early full-screen app shells and large green panels made the display compete
  with the physical scene;
- later pronunciation and pairing flows succeeded by moving persistent content
  to the lower field, using a small footer, and making timing a thin line;
- menu iterations improved when one focused item replaced a row of equally
  strong framed controls;
- game and character experiences remained expressive when central pixels were
  reserved for actual content while product chrome moved to the edge.

The lower-field default reflects these observed product iterations and should
be validated with wearer comfort and task success in each release. It is a
design default, not a physiological universal.

## 11. Linked human-facing materials

- [Browsable spatial HUD design page](./preview-green-spatial-hud.html)
- [Chinese public documentation](../../../documentation/6-design/visual/glasses-hud.md)
- [English public documentation](../../../documentation/6-design/visual/glasses-hud.en-US.md)
- [Base monochrome-green token system](../design-system-green.md)
