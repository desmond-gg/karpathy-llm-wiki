# chip-obsidian-wiki

面向数字芯片前端设计、数字芯片后端设计、CPU/GPU/DPU/TPU 架构、ARM/x86/AMD 相关 IP、协议/spec、系统工程和验证场景的 Obsidian 知识库 Agent Skill。

这个 skill 基于 LLM Wiki 思路：`raw/` 保存不可变原始资料，`wiki/` 保存由 agent 持续维护的专业知识页。知识页使用 Obsidian wikilink、Dataview frontmatter、版本范围和工程视角组织，适合长期维护数字前后端设计知识、协议/spec、IP、架构和实现经验。

## 核心能力

| Operation | What it does | Output |
|-----------|--------------|--------|
| Ingest | 将 raw 资料编译进专业 wiki，并级联更新相关页面 | 新增或更新 wiki 页面、索引、日志 |
| Query | 基于 wiki 回答芯片工程问题 | 带引用和版本范围的回答 |
| Lint | 检查索引、wikilink、raw 引用、frontmatter、版本范围和术语一致性 | 自动修复安全项并报告风险 |

## 目录模型

```text
your-vault/
├── raw/                  # 扁平目录，保存原始资料和 Web Clipper 笔记
│   └── 2026-04-09-pcie-no-snoop.md
├── wiki/                 # Agent 维护的知识库
│   ├── architecture/
│   │   ├── cpu/arm/
│   │   ├── cpu/x86/
│   │   ├── dpu/
│   │   └── tpu/
│   ├── frontend-design/
│   │   ├── rtl/
│   │   ├── cdc-rdc/
│   │   ├── low-power/
│   │   └── register-interface/
│   ├── backend-design/
│   │   ├── synthesis/
│   │   ├── floorplan/
│   │   ├── pnr/
│   │   ├── sta/
│   │   ├── cts/
│   │   ├── ir-em/
│   │   └── eco/
│   ├── dft/
│   │   ├── scan-atpg/
│   │   └── mbist-lbist/
│   ├── microarchitecture/
│   ├── specs/
│   │   └── amba-chi/
│   │       ├── index.md  # spec family page
│   │       └── issue-e.md
│   ├── protocols/
│   ├── ip/
│   ├── gpu/
│   ├── memory-system/
│   ├── verification/
│   ├── glossary/
│   ├── index.md
│   └── log.md
└── SKILL.md
```

`raw/` 不按 topic 分目录。`wiki/` 允许深层目录，以领域导航为主。

## Obsidian 约定

- wiki 内部知识页使用 `[[wikilink]]`。
- raw/source 引用使用 markdown 相对路径，保证溯源文件明确。
- wiki 页面使用完整 Dataview frontmatter，包括 `type`、`domain`、`version`、`applies_to`、`source_authority`、`source_language`、`updated` 等字段。
- `domain` 支持 `cpu`、`gpu`、`dpu`、`tpu`、`frontend-design`、`backend-design`、`dft`、`physical-design` 等维度，便于按工程职责和业务对象过滤。
- `type` 表达页面意图，例如 `spec-family`、`spec-version`、`ip-block`、`interface`、`register-model`、`frontend-note`、`backend-note`、`dft-note`、`timing-note`。
- `applies_to` 使用受控短语表达适用范围，例如 `ARMv9-A`、`x86-64`、`PCIe 5.0`、`AMBA CHI Issue E`、`AMD Zen`。
- `tags` 只作辅助，保留 `wiki`、`raw`、`archive` 等结构标签，不重复承担 `domain/type/applies_to` 的分类职责。
- 英文资料中的关键定义、规范语句和 `shall/must/may` 类约束可保留短句原文，并紧跟中文说明。
- 统一维护术语表，避免同一英文术语出现多个中文译法。

## Schema 治理

`SKILL.md` 是 schema 单一事实源。Agent 不应在 ingest、query、archive 或 lint 过程中临时新增 `domain` 或 `type`；如果现有枚举无法表达新的芯片设计类别，需要先报告缺口，再通过一次 schema 更新同步修改规范、模板和 README。

## Spec 版本模型

协议和 spec 使用 family + version 页面：

- Family page: `wiki/specs/<spec-family>/index.md`
- Version page: `wiki/specs/<spec-family>/<version-slug>.md`

例如：

- `wiki/specs/amba-chi/index.md`
- `wiki/specs/amba-chi/issue-e.md`

所有版本相关 claim 都必须标明适用版本或 `applies_to` 范围，避免把某个版本的行为误写成全局事实。

## Raw 属性模板

Agent 创建 raw URL 笔记时使用 Obsidian 属性格式：

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

用户也可以通过 Obsidian Web Clipper 手动保存 raw 文章，只要保持同一属性结构即可。

## Templates

完整规范见 [SKILL.md](SKILL.md)。模板位于 [references/](references/)：

- `raw-template.md` — raw 原始资料模板
- `article-template.md` — wiki 知识页模板
- `spec-family-template.md` — 协议/spec family 页面模板
- `spec-version-template.md` — 协议/spec version 页面模板
- `glossary-template.md` — 中英术语表页面模板
- `index-template.md` — 全局索引模板
- `archive-template.md` — 查询归档模板
- `workflow-checklist.md` — ingest/query/archive/lint 操作检查清单

## Pending Work

URL ingest 的 Chrome + Obsidian Web Clipper 自动化流程仍需单独设计，已记录在 [todo.md](todo.md)。
