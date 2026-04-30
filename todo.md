# TODO

## Agent URL Ingest via Chrome + Obsidian

需要进一步讨论并设计：当用户要求 agent ingest 一个 URL 时，Codex 如何操作 Chrome 生成 markdown，并保存到 Obsidian 的 `raw/` 扁平目录中。

待明确问题：

- 使用 Chrome 页面内容抓取、Obsidian Web Clipper，还是二者结合。
- Codex 如何触发保存：浏览器扩展、剪贴板、Obsidian URI、文件系统写入，或其他方式。
- 如何确定目标 vault 和 `raw/` 目录路径。
- 如何从网页提取 `title`、`source`、`author`、`published`、`created`、`description`、`tags` 属性。
- 如何处理登录态网页、动态渲染页面、PDF、图片和代码块。
- 如何避免覆盖用户已有 raw 文件，并处理同名 URL 的去重。
- 是否在保存 raw 后自动进入 wiki compile 流程。

目标 raw 属性模板：

```yaml
---
title: "【101】PCIe No Snoop、TLP TPH和intel DDIO"
source: "https://blog.csdn.net/linjiasen/article/details/144769127"
author:
  - "[[linjiasen]]"
published: 2024-12-27
created: 2026-04-09
description: ""
tags:
  - "raw"
---
```