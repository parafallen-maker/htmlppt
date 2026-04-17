---
name: htmlppt
description: 生成可分享、可导出 PDF 的 HTML 演示文稿。适用于从零创建演示、把 PPT/PPTX 转成网页幻灯片，或增强现有 HTML 演示。
version: 1.0.0
metadata: { "openclaw": { "emoji": "🖼️" } }
---

# htmlppt

这是 `htmlppt` 的 OpenClaw 友好分发包。

核心工作流与仓库根目录的 `SKILL.md` 保持一致，差异只有两点：

1. 增加了 `metadata.openclaw`
2. 如果当前宿主不支持结构化提问，就把需求收集问题合并成一条简洁消息来问

执行时请同时读取以下同目录文件：

- `STYLE_PRESETS.md`
- `viewport-base.css`
- `html-template.md`
- `animation-patterns.md`
- `scripts/extract-pptx.py`
- `scripts/deploy.sh`
- `scripts/export-pdf.sh`

完整规则见仓库根目录 `SKILL.md`。
