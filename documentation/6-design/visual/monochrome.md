# 单色显示（Monochrome）

`Monochrome` 描述 AIUI 在单色透明显示设备上的视觉语言。当前公开变体为面向 RokidGlasses1 / RokidGlasses2 的 `Green`：所有可见像素都是同一绿色通道在不同亮度下的表达，纯黑代表透明显示底层。

单色规范解决“**看起来如何**”：颜色、文字、线条、间距和组件外观。对于全屏 Page 的“**放在哪里、何时出现、占多少注意力**”，继续阅读 [低干扰空间 HUD](/AIUI/design/visual-glasses-hud)。

## 设计原则

- 不引入第二种颜色表达状态。状态还必须使用文字、图标/形状、线条样式或交互状态说明。
- 透明底层不是黑色遮罩；不要用不透明黑色大面板遮住真实环境。
- 以描边、亮度层级和稳定留白组织信息；不要依赖模糊阴影。
- 面板是必要分组时的局部工具，不是每个页面都要有的应用外壳。
- 关键内容必须保留在 `480 × 352` 参考画布的安全区域内；参考安全边距为横向 `16px`、纵向 `12px`。
- 页面模式和组件选择以任务为先：导航用途的 `<card>` 不应被用作通用背景容器。

## 颜色与亮度角色

这不是多色板，而是一条单绿色亮度阶梯。名称与源规范保持一致：

| Token | 典型用途 | 值 |
| --- | --- | --- |
| `primary` / `ink` | 当前焦点、关键数字、最高强调 | `#40ff5e` |
| `primary-72` / `ink-primary` | 可读主文字、活动结构线 | `rgba(64,255,94,0.72)` |
| `primary-48` / `ink-secondary` | 次级文字、普通边界 | `rgba(64,255,94,0.48)` |
| `primary-24` / `line-muted` | 分隔线、安静结构提示 | `rgba(64,255,94,0.24)` |
| `primary-12` / `surface-active` | 局部选中/聚焦支撑填充 | `rgba(64,255,94,0.12)` |
| `primary-06` / `line-trace` | 极少量装饰性痕迹或微弱 surface | `rgba(64,255,94,0.06)` |
| `background` / `surface` | 透明显示底层 | `#000000` |

常规状态下，局部填充最高使用 `primary-12`；不要用绿色大面积填充做标题、默认按钮或页面背景。

## 排版、间距和边界

| 类别 | 当前基线 |
| --- | --- |
| 最高层级文字 | `display`：`22px`、500、sans-serif |
| 标题 | `heading`：`16px`、500、sans-serif |
| 正文 | `body`：`14px`、400、sans-serif |
| 标签 / 注释 | `label`：`11px`；`caption`：`10px` |
| 数据 | `data`：`13px`、500、monospace |
| 间距 | `2 / 4 / 8 / 12 / 16 / 24 / 32px` |
| 圆角 | `0 / 2 / 4 / 6px`，仅在局部分组需要时使用 |
| 默认描边 | `1px` |
| 焦点描边 | `2px`，仅用于当前焦点或活动目标 |

使用“默认 1px、焦点 2px”的层次，而不是默认 2px、强调 4px。强轮廓的数量同样受到 [低干扰空间 HUD](/AIUI/design/visual-glasses-hud) 的单一注意力峰值约束。

## 组件基线

| 组件 | 默认表达 |
| --- | --- |
| `panel` / `card` | 透明或黑色底层 + `line-muted` 的 1px 局部边界 + 最多 6px 圆角；只在分组确实提升理解时出现 |
| `card-highlight` | 局部 `primary-12` 填充 + 强边界；不是整页高亮方式 |
| `button` | 紧凑描边操作；实体绿色填充只留给不可逆或关键确认时刻 |
| `text-input` / `textarea` | `primary-06` 低填充 + 默认边界 |
| `list-row` | 开放行 + 细分隔；不要为每一行叠卡片 |
| `status` / `error-state` | 绿色文字结合标签、图标/形状或线型；错误态不切换为红色 |

## Do

- 用 token、文字层级、留白和细线建立信息层级。
- 将填充和强描边限制在当前局部任务或焦点。
- 让无关信息在没有任务时消失，而不是常驻占位。
- 在目标设备的深色、明亮和视觉杂乱背景上测试可读性。
- 对全屏 Page 先选择空间 HUD 模式，再选择组件。

## Don't

- 不要引入红、蓝或第二种颜色，也不要仅靠绿色亮度表达语义。
- 不要用整页卡片、大片绿色填充、阴影或装饰性边框制造“应用感”。
- 不要把每个菜单项、设置项都画成同样强的圆角框。
- 不要假设浏览器的 CSS 或字体行为在 AIUI WXSS 中可用。
- 不要把 `480 × 352` 理解成应该被 UI 填满的矩形。

## 规范与预览

- [基础单绿源规范](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/design-system-green.md)
- [基础单绿可视预览](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/preview-green.html)
- [低干扰空间 HUD 源规范](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/glasses-hud/design-system-green-spatial-hud.md)
- [低干扰空间 HUD 交互预览](https://github.com/sengmitnick/AIUI/blob/main/design/monochrome/glasses-hud/preview-green-spatial-hud.html)
