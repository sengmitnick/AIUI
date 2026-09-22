# AIUI Visual Design Language

This directory holds AIUI's visual design language specs. Each spec is scoped
to a specific **display type**, because AIUI runs on hardware with very
different color reproduction capabilities.

## Layout

```text
design/
├── monochrome/          # single-color display specs (one channel on pure black)
│   ├── design-system-green.md
│   ├── preview-green.html
│   └── glasses-hud/     # placement and attention profile for transparent glasses Pages
│       ├── design-system-green-spatial-hud.md
│       └── preview-green-spatial-hud.html
└── fullcolor/           # planned — full-RGB display specs (not yet authored)
```

Today only the **monochrome-green** variant exists; the full-color area is
reserved for future hardware.

| Subdir | Display type | Status |
|--------|--------------|--------|
| [`monochrome/`](./monochrome/) | Single-color display (one channel on pure black) | Active — `green` hue variant available |
| `fullcolor/` | Full-RGB color display | Planned — not yet authored |

## Active spec

The monochrome-green system — for RokidGlasses1 / RokidGlasses2, whose
hardware can only reproduce one luminous green channel over pure black —
lives at:

- [`monochrome/design-system-green.md`](./monochrome/design-system-green.md) — full token spec
- [`monochrome/preview-green.html`](./monochrome/preview-green.html) — browsable visual showcase
- [`monochrome/glasses-hud/`](./monochrome/glasses-hud/) — low-interference spatial HUD profile for eligible full-screen transparent-glasses Pages
  - [`design-system-green-spatial-hud.md`](./monochrome/glasses-hud/design-system-green-spatial-hud.md) — placement, attention, runtime, and on-device review rules
  - [`preview-green-spatial-hud.html`](./monochrome/glasses-hud/preview-green-spatial-hud.html) — self-contained interactive placement guide

See [`monochrome/README.md`](./monochrome/README.md) for details.

## How it fits into the repository

| Audience | Where you meet the design language |
|----------|------------------------------------|
| AI agents (Claude Code, Cursor, Codex, …) | Bundled as on-demand `skills/aiui-dev/references/design/` references via `npx skills add` |
| Human developers | This directory, linked from the root [`README.md`](../README.md) |
| Sample authors | Mirror these tokens under [`samples/`](../samples/) |

The AI-readable copies are intentionally identical to their corresponding
source documents here, so agents can align generated code without fetching
remote URLs. When you edit a source spec, update its matching reference under
`skills/aiui-dev/references/design/` in the same change. The low-interference
spatial HUD profile extends the base green system; it is not a new color variant
or standalone Skill.

## License

Apache License 2.0, inherited from the repository root.
