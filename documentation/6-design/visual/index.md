# 视觉设计

本目录用于承接仓库根目录 `design/` 下的视觉设计语言内容，并按显示类型组织 AIUI 的视觉规范。

AIUI 运行在显示能力差异很大的设备上，因此视觉规范不是单一的一篇文档，而是一组按显示类型拆分的设计语言文档。

## 目录结构

```text
visual/
├── index.md
├── monochrome.md
└── glasses-hud.md
```

当前文档结构与仓库中的 `design/` 保持对应关系：

- `design/README.md` -> 本页
- `design/monochrome/README.md` + `design/monochrome/design-system-green.md` -> `Monochrome`
- `design/monochrome/glasses-hud/design-system-green-spatial-hud.md` -> `低干扰空间 HUD`

## 显示类型

| 类型 | 说明 | 状态 |
| --- | --- | --- |
| `Monochrome` | 单色显示，只能在纯黑背景上渲染单一发光通道 | 已启用 |
| `Full Color` | 全彩显示能力 | 规划中 |

当前只有 `Monochrome` 分支处于启用状态，面向 Rokid Glasses 的单绿色显示硬件。

## 当前生效规范

当前主规范是 `Monochrome`，其中包含当前启用的 `Green` 变体，用于 RokidGlasses1 / RokidGlasses2：

- 单一绿色发光通道
- 纯黑背景
- 以描边、透明填充和稳定留白构成 HUD 风格界面
- 以 Design Tokens 作为宿主注入和应用覆盖的统一接口

对于会展示常驻或瞬时信息的全屏 Page，`Monochrome` 还提供 `低干扰空间 HUD` profile：它把下方主栏设为默认、将中部留给任务内容，并限制每个稳定状态的注意力峰值。它扩展单绿 token，不取代单色规范，也不自动应用到对话卡片或 Widget。

## 与仓库的关系

| 使用对象 | 入口 |
| --- | --- |
| 人类开发者 | 本文档目录 |
| 设计源文件维护者 | 仓库根目录 `design/` |
| AI 代码生成与辅助工具 | 与设计源同步的规范副本 |

这组文档的目标不是替代 `design/` 目录，而是把其中的设计语言整理进对外文档体系，便于导航、引用和后续扩展。

## 下一步阅读

- [Monochrome](/AIUI/design/visual-monochrome)
- [低干扰空间 HUD](/AIUI/design/visual-glasses-hud)
