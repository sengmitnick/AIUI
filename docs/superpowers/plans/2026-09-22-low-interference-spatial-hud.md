# Low-Interference Spatial HUD Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use box:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add an AI-recognizable, human-readable low-interference spatial HUD profile for AIUI monochrome-green transparent glasses without changing upstream code or creating a new standalone skill.

**Architecture:** Keep the normative source of truth under `design/monochrome/glasses-hud/`, mirror the same rules into the existing `skills/aiui-dev` on-demand reference folder, and make the skill loader name the profile explicitly. Publish concise Chinese and English guides through the existing `documentation/6-design/visual/` navigation, while the self-contained HTML preview demonstrates the spatial rules with CSS/SVG and no runtime dependency.

**Tech Stack:** Markdown, JSON navigation metadata, self-contained HTML/CSS/vanilla JavaScript, existing AIUI WXSS and component references.

---

## File map

| File | Responsibility |
| --- | --- |
| `design/monochrome/glasses-hud/design-system-green-spatial-hud.md` | Normative source: scope, placement modes, anchors, attention budget, runtime guardrails, and review protocol. |
| `design/monochrome/glasses-hud/README.md` | Short index that explains profile relationship, files, and audience. |
| `design/monochrome/glasses-hud/preview-green-spatial-hud.html` | Offline browsable demonstration of correct and incorrect HUD placement. |
| `skills/aiui-dev/references/design/green-spatial-hud.md` | Exact AI-consumable mirror of the normative source. |
| `skills/aiui-dev/SKILL.md` | On-demand loader instruction that tells an AI when to load the spatial profile. |
| `skills/aiui-dev/references/checklist.md` | Build/review check that stops eligible Pages from reverting to a phone-like shell. |
| `documentation/6-design/visual/glasses-hud*.md` | Public Chinese and English guides, with concise rules and links to the detailed source/preview. |
| `documentation/6-design/visual/{index.md,index.en-US.md}`, `documentation/toc.json`, and `documentation/toc.en-US.json` | Discoverable navigation for the new guides in both documentation languages. |
| `documentation/6-design/visual/monochrome*.md`, `design/README.md`, `design/monochrome/README.md` | Correct stale four-tier/card-first language and link the spatial profile. |
| `README.md`, `README.zh-CN.md`, and `documentation/7-tools/skills*.md` | Keep the repository and existing-skill descriptions aligned with the new profile. |

### Task 1: Make the existing AIUI skill recognize the profile

**Files:**
- Create: `skills/aiui-dev/references/design/green-spatial-hud.md`
- Modify: `skills/aiui-dev/SKILL.md`
- Modify: `skills/aiui-dev/references/checklist.md`
- Reference: `design/monochrome/glasses-hud/design-system-green-spatial-hud.md`

- [x] **Step 1: Copy the normative source without rewriting it.**

  The skill reference must be byte-identical to the design source so an agent can load either location without receiving two conflicting rules.

  Run:

  ```bash
  cp design/monochrome/glasses-hud/design-system-green-spatial-hud.md \
    skills/aiui-dev/references/design/green-spatial-hud.md
  cmp -s design/monochrome/glasses-hud/design-system-green-spatial-hud.md \
    skills/aiui-dev/references/design/green-spatial-hud.md
  ```

  Expected: `cmp` exits `0`.

- [x] **Step 2: Add an explicit on-demand loader rule.**

  Add a loader entry whose meaning is exactly:

  ```markdown
  - Low-interference spatial HUD: for an eligible full-screen monochrome-green
    Page that shows persistent or momentary transparent-glasses HUD information,
    read `references/design/green-spatial-hud.md` after
    `references/design/monochrome-green.md`. Do not load it for a dense
    conversation card, document reader, or reusable widget unless its product
    state explicitly uses the spatial HUD profile.
  ```

  The target/design-choice sections must name the same profile and state that
  it extends—not replaces—`monochrome-green`.

- [x] **Step 3: Add a final review guardrail.**

  Add this checklist condition beneath visual standards:

  ```markdown
  - For an eligible full-screen monochrome-green Page, apply the
    low-interference spatial HUD profile: justify top/center/full-canvas use,
    keep persistent chrome in the lower rail by default, and leave one primary
    attention peak.
  ```

- [x] **Step 4: Verify recognition and mirror integrity.**

  Run:

  ```bash
  cmp -s design/monochrome/glasses-hud/design-system-green-spatial-hud.md \
    skills/aiui-dev/references/design/green-spatial-hud.md
  rg -n "Low-interference spatial HUD|green-spatial-hud|one primary attention peak" \
    skills/aiui-dev
  ```

  Expected: `cmp` exits `0`; the ripgrep output shows the loader and checklist.

### Task 2: Publish a navigable bilingual design guide

**Files:**
- Create: `documentation/6-design/visual/glasses-hud.md`
- Create: `documentation/6-design/visual/glasses-hud.en-US.md`
- Modify: `documentation/6-design/visual/index.md`
- Modify: `documentation/6-design/visual/index.en-US.md`
- Modify: `documentation/toc.json`
- Modify: `documentation/toc.en-US.json`
- Modify: `documentation/6-design/visual/monochrome.md`
- Modify: `documentation/6-design/visual/monochrome.en-US.md`

- [x] **Step 1: Write the Chinese guide around decisions, not decorative prose.**

  Include: the window-not-screen thesis; a six-row placement-mode table; the
  `28 / 58 / 14` lower-rail starter anchors marked as tunable defaults; a
  one-attention-peak budget; correct/wrong patterns; a hardware/background/motion
  review checklist; and direct links to the source spec and offline preview.

  Use this table vocabulary exactly so the public guide maps to the source:

  ```markdown
  | 模式 | 适用任务 | 默认位置 | 禁止用法 |
  | --- | --- | --- | --- |
  | `quiet` | 无待办信息 | 不显示 HUD | 用常驻装饰填满画面 |
  | `bottom-primary` | 常驻提示、短操作 | 下方主栏 + 页脚 | 用中部大卡片承载通用 chrome |
  | `lower-center-moment` | 配对码、一次确认 | 下半部居中 | 常驻整页边框 |
  | `top-transient` | 非关键短状态 | 顶部安全条 | 遮挡中心任务内容 |
  | `center-task` | 阅读、对象、角色 | 有边界的中央内容 | 放通用状态栏或菜单墙 |
  | `immersive-content` | 游戏、相机、媒体 | 全画布任务内容 | 把全屏当应用外壳 |
  ```

- [x] **Step 2: Write the English guide as an equivalent page, not an opaque translation stub.**

  Preserve the same six mode identifiers, implementation guardrails, examples,
  and local links. State that lower placement is an observed product default to
  validate with on-device use, not a universal physiological assertion.

- [x] **Step 3: Correct stale public monochrome guidance.**

  Replace the obsolete “four opacity tiers”, universal card styling, 2px default
  border, 12px radius, and 40% highlight guidance with the current six
  luminance roles, 1px default/2px focused outline, 2/4/6px radii, and local
  `primary-12` maximum selection fill. Link to the spatial HUD guide for Page
  placement decisions.

- [x] **Step 4: Add navigation entries following the existing JSON and Markdown conventions.**

  Add Chinese `低干扰空间 HUD` and English `Low-Interference Spatial HUD` next
  to the visual-design entries in their matching index and TOC files. Use only
  repository-relative links and keep both JSON files syntactically valid.

- [x] **Step 5: Validate discoverability.**

  Run:

  ```bash
  node -e "for (const f of ['documentation/toc.json','documentation/toc.en-US.json']) JSON.parse(require('fs').readFileSync(f,'utf8')); console.log('toc files valid')"
  rg -n "低干扰空间 HUD|Low-Interference Spatial HUD|primary-12" \
    documentation/6-design/visual documentation/toc.json
  ```

  Expected: `toc files valid`; both language names and the token correction are
  present.

### Task 3: Build the offline visual design page and source indexes

**Files:**
- Create: `design/monochrome/glasses-hud/README.md`
- Create: `design/monochrome/glasses-hud/preview-green-spatial-hud.html`
- Modify: `design/README.md`
- Modify: `design/monochrome/README.md`
- Modify: `README.md`
- Modify: `README.zh-CN.md`
- Modify: `documentation/7-tools/skills.md`
- Modify: `documentation/7-tools/skills.en-US.md`

- [x] **Step 1: Write the profile index.**

  Explain that `design-system-green-spatial-hud.md` is normative, the HTML file
  is a visual explanation, and the profile extends the base green token system.
  Link to the two public guides and state that no build step is required to open
  the preview.

- [x] **Step 2: Build the standalone preview with a document, not app, layout.**

  The page must be self-contained and include no external assets, fonts, or
  build tooling. It needs a desktop documentation shell with a sticky side
  navigation, a readable content column, and interactive static simulations.
  Its central visual must make this contrast obvious:

  ```html
  <button data-scene="good-bottom">推荐：底部主栏</button>
  <button data-scene="bad-fullscreen">不推荐：整页应用卡片</button>
  <div class="lens-stage" data-environment="dark">
    <div class="hud lower-rail">...</div>
    <div class="hud footer">...</div>
  </div>
  ```

  Include scene controls for pronunciation, pairing, menu focus, character/game
  content, and the settings-wall counterexample. Include CSS-only environment
  choices for dark, bright, and visually busy backgrounds. The page's JavaScript
  may only switch scene/environment classes and update explanatory copy; it is
  documentation JavaScript, not AIUI runtime code.

- [x] **Step 3: Make the visual examples enforce the rules.**

  Correct examples use transparent space, one bright focus, a thin progress
  line, and the lower rail/footer anchors. Bad examples visibly demonstrate a
  large fill, page-sized border, centered generic shell, or repeated bright
  option boxes. Label counterexamples clearly as non-recommended.

- [x] **Step 4: Link the profile from both design indexes.**

  Update their file maps and “active spec” sections with a link to
  `monochrome/glasses-hud/`. Correct the old “four opacity tiers” wording to
  six luminance roles at the same time.

- [x] **Step 4a: Keep repository and skill descriptions synchronized.**

  Replace stale "four opacity tiers" wording in both root README files with the
  current six-role scale, add the spatial-HUD source and preview to their design
  trees, and state that the existing `aiui-dev` Skill can load the profile when
  a full-screen monochrome-green Page needs it. Add
  `green-spatial-hud.md` to the two existing skill reference lists; do not
  describe it as a separate Skill.

- [x] **Step 5: Verify the preview structurally and visually.**

  Run:

  ```bash
  node -e "const fs=require('fs'); const h=fs.readFileSync('design/monochrome/glasses-hud/preview-green-spatial-hud.html','utf8'); for (const s of ['data-scene','data-environment','lower-rail','settings-wall']) if (!h.includes(s)) throw new Error('missing '+s); console.log('preview structure valid')"
  git diff --check
  ```

  Expected: `preview structure valid` and no whitespace errors. Open the file in
  a browser, change every scene and background, and confirm that controls and
  explanatory text remain readable with no horizontal overflow.

### Task 4: Cross-check, commit, and publish the fork branch

**Files:**
- Verify all files from Tasks 1–3
- Modify: `docs/superpowers/plans/2026-09-22-low-interference-spatial-hud.md` (mark completed checks only after verification)

- [x] **Step 1: Verify local links and JSON in a read-only check.**

  Run a Node script that extracts Markdown links in the created/modified design
  documents, ignores HTTP anchors, and exits nonzero when a repository-relative
  file target is missing. Then parse `documentation/toc.json`.

- [x] **Step 2: Review the complete diff against the scope.**

  Run:

  ```bash
  git status --short
  git --no-pager diff --stat HEAD
  git --no-pager diff --check HEAD
  ```

  Expected: changes are limited to source docs, current-skill recognition,
  visual documentation/navigation, and this plan. No application runtime or
  unrelated sample source files change.

- [ ] **Step 3: Commit the cohesive documentation feature.**

  Run:

  ```bash
  git add design documentation skills docs/superpowers/plans/2026-09-22-low-interference-spatial-hud.md
  git commit -m "docs: publish low-interference HUD guide"
  ```

  Expected: one single-line conventional documentation commit.

- [ ] **Step 4: Push only the fork branch.**

  Run:

  ```bash
  git push -u origin docs/low-interference-spatial-hud
  ```

  Expected: the branch appears in `sengmitnick/AIUI`; no upstream remote is
  modified and no pull request is opened unless the user asks for one.

- [ ] **Step 5: Report the outcome.**

  Provide the fork branch URL, commit hashes, the key source/preview/public-doc
  links, verification results, and the deliberate scope boundary: no standalone
  skill package, application implementation, or upstream write.
