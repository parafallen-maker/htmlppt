# htmlppt

一个通用型的 HTML 演示文稿 skill，用来生成单文件、可分享、可导出 PDF 的网页幻灯片。它可以从零生成，也可以把现有的 `.pptx` 内容抽取后重组为 HTML 版演示。

这个仓库由 [`zarazhangrui/frontend-slides`](https://github.com/zarazhangrui/frontend-slides) 改造而来，重点补了三件事：

- PDF 导出脚本优先复用现有 Playwright，找不到时才安装
- 增加中文 / CJK 字体策略，避免中文演示退回到很丑的默认字体
- 改成宿主无关的 `htmlppt` skill，方便给 Codex、Claude Code、OpenClaw、Hermes 等环境复用

## 适合什么场景

- 路演、方案汇报、培训课件、产品介绍、发布会式网页 slides
- 需要输出 HTML 链接，同时顺手导成 PDF 的场景
- 想保留“单文件 HTML，后续可手改”的交付方式

## 核心特性

- 单文件 HTML 输出，CSS / JS 内联，无需构建工具
- 先看 3 个视觉预览，再定整套风格
- PPT 内容抽取，保留文本、图片、备注、顺序
- 视口锁定策略，每页强制适配 `100vh / 100dvh`
- 中文友好字体栈，支持 `Noto Sans SC` / `Noto Serif SC` / `Source Han` / 苹方 / 微软雅黑等回退
- 一键部署到 Vercel 或导出 PDF

## 目录结构

| 路径 | 作用 |
| --- | --- |
| `SKILL.md` | 通用 skill 工作流，宿主无关 |
| `STYLE_PRESETS.md` | 12 套风格预设和字体搭配 |
| `viewport-base.css` | 强制视口适配 CSS |
| `html-template.md` | HTML 模板和交互能力参考 |
| `animation-patterns.md` | 动效模式参考 |
| `scripts/extract-pptx.py` | PPT 内容抽取脚本 |
| `scripts/export-pdf.sh` | HTML 导出 PDF |
| `scripts/deploy.sh` | 部署到 Vercel |
| `plugins/htmlppt/` | Claude Code / 插件市场分发包 |
| `openclaw/skills/htmlppt/` | OpenClaw 友好分发包 |

## 安装

### 1. 通用手动安装

把整个仓库克隆到任意目录，然后复制 `htmlppt` 目录到你的宿主 skill 目录即可。**关键点是保留相对路径结构**，不要只复制 `SKILL.md`。

推荐目录：

- Codex: `~/.codex/skills/htmlppt`
- Claude Code: `~/.claude/skills/htmlppt`
- OpenClaw / Hermes / 其他代理宿主: 放到对应的 skills 目录中，目录名保持 `htmlppt`

示例：

```bash
git clone https://github.com/parafallen-maker/htmlppt.git

mkdir -p ~/.codex/skills
cp -R ./htmlppt ~/.codex/skills/htmlppt
```

### 2. Claude Code / 插件市场

如果你的宿主支持 Claude 插件市场格式，可以使用 `plugins/htmlppt/` 这一份分发包。

```bash
/plugin marketplace add parafallen-maker/htmlppt
/plugin install htmlppt@htmlppt
```

### 3. OpenClaw

OpenClaw 可以直接使用 `openclaw/skills/htmlppt/` 这份分发包，或者把仓库根目录作为 skill 包挂载进去。OpenClaw 变体额外加了 `metadata.openclaw`，但核心工作流和通用版一致。

## 使用方式

### 从零生成

```text
/htmlppt

> 我想做一份 AI 医疗产品介绍，12 页左右，风格克制但高级
```

### 转换 PPT

```text
/htmlppt

> 把 ./demo/presentation.pptx 转成 HTML 演示
```

### 增强已有 HTML

```text
/htmlppt

> 优化 ./deck/index.html，保留版式，补强动效和中文字体
```

## 脚本

### 导出 PDF

```bash
bash scripts/export-pdf.sh ./presentation.html
bash scripts/export-pdf.sh ./presentation.html ./presentation.pdf --compact
```

新版本会先检查本机已有的 Playwright 模块和 Chromium 浏览器，只有找不到时才做临时安装。

### 部署链接

```bash
bash scripts/deploy.sh ./presentation.html
bash scripts/deploy.sh ./my-deck/
```

### 抽取 PPT 内容

```bash
python3 scripts/extract-pptx.py ./presentation.pptx ./out
```

需要：

- Python 3
- `python-pptx`

## 中文 / CJK 字体策略

这个仓库不再把“永远不用系统字体”当成绝对规则。对中文演示，更重要的是覆盖完整和跨平台稳定。

推荐策略：

- 中文无衬线：`Noto Sans SC` + `Source Han Sans SC` + `PingFang SC` + `Hiragino Sans GB` + `Microsoft YaHei`
- 中文衬线：`Noto Serif SC` + `Source Han Serif SC` + `Songti SC` + `STSong` + `SimSun`
- 如果标题需要更强设计感，可以继续使用拉丁展示字体，但后面必须接中文 fallback

## 宿主兼容原则

`htmlppt` 的核心是“宿主无关的 skill 工作流”：

- 不依赖某一个 CLI 的私有插件系统
- 支持根目录直接安装
- 支持插件包分发
- 支持 OpenClaw 风格的 skill frontmatter 扩展
- 对不支持结构化提问的宿主，也能退化为普通聊天式收集信息

## 已知边界

- PPT 转 HTML 不是高保真复刻，更接近“抽内容后重新设计”
- 图表、复杂表格、SmartArt、动画时间轴不会被原样保留
- PDF 导出是静态截图，不保留动画

## 致谢

- 原始项目: [`zarazhangrui/frontend-slides`](https://github.com/zarazhangrui/frontend-slides)
- 许可证: MIT
