# Monochrome-Green Low-Interference Spatial HUD

> **中文：单绿低干扰空间 HUD。** A placement and attention profile for transparent AI glasses.

This directory extends the parent [monochrome-green system](../design-system-green.md).
It does **not** introduce a color variant, replace the base component tokens, or
create a separate AIUI Skill. Its job is to define where a transparent-glasses
HUD belongs, when it should appear, and how much visual attention it may use.

## Files

| File | Audience | Purpose |
| --- | --- | --- |
| [design-system-green-spatial-hud.md](./design-system-green-spatial-hud.md) | AI agents and maintainers | Normative, diff-friendly source of truth: scope, placement modes, anchors, attention budget, runtime guardrails, and device review. |
| [preview-green-spatial-hud.html](./preview-green-spatial-hud.html) | Designers and developers | Self-contained interactive design page. Open it directly in a browser; it needs no build step or external asset. |

## Core decision

**The glasses are a window, not a phone screen.** The `480 × 352` reference
canvas is a coordinate and safety boundary, not a visual-fill target.

For an eligible full-screen Page, select a placement mode before selecting
components:

- `quiet`: no HUD while no information needs action.
- `bottom-primary`: lower rail plus footer for persistent prompt, status, or one action.
- `lower-center-moment`: a temporary code, confirmation, or singular datum.
- `top-transient`: a short, noncritical state.
- `center-task`: bounded reading, object, or character content.
- `immersive-content`: full canvas only when the task itself is game, camera, media, or spatial content.

The default is `bottom-primary`. Top, center, and full-canvas use need a task
reason. A normal stable state has one primary attention peak, at most one strong
outline, and no large filled surface.

## AIUI skill integration

The existing [`aiui-dev`](../../../skills/aiui-dev/SKILL.md) Skill loads a
byte-identical mirror only for eligible full-screen monochrome-green Pages:

- [AI-readable reference mirror](../../../skills/aiui-dev/references/design/green-spatial-hud.md)
- [delivery checklist guardrail](../../../skills/aiui-dev/references/checklist.md)

Do not add a standalone Skill for this profile. The base green visual language
must be loaded first; this profile adds spatial placement and attention rules.

## Public documentation

- [中文：低干扰空间 HUD](../../../documentation/6-design/visual/glasses-hud.md)
- [English: Low-Interference Spatial HUD](../../../documentation/6-design/visual/glasses-hud.en-US.md)
- [Base Monochrome documentation](../../../documentation/6-design/visual/monochrome.en-US.md)

## Validation boundary

The HTML preview is an explanation and a browser-only interaction demo. It does
not prove target-device rendering. Validate every final Page on target hardware
against dark, bright, and visually busy real-world backgrounds while static,
walking, and turning the head.
