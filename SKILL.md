---
name: ken-code-spec
description: Use when initializing or refreshing a project with Ken-style lightweight agent entrypoints, cross-tool rules for Codex, Claude Code, and Cursor, and a docs skeleton with spec-* documentation files.
---

# Ken Code Spec

Ken Code Spec 初始化一个轻量项目协作基线：跨工具 Agent 入口、文档先行的项目说明骨架、以及可持续补齐的 `docs/` 目录。它只负责项目基线，不接管功能实现、调试、测试策略或代码审查。

## 使用边界

使用本 skill：
- 用户要求初始化新项目、补齐 Agent 入口、创建 `AGENTS.md` / `CLAUDE.md` / Cursor rules。
- 用户希望按 Ken Code Spec 的当前模板建立项目文档骨架。
- 目标项目缺少轻量协作入口或 `docs/` 基础结构。

不要使用本 skill：
- 单纯实现功能、修 bug、写测试或做代码审查。
- 用户只想讨论方案，不要求落文件。
- 目标仓库已有更具体的项目规则，且用户没有要求替换或对齐。

## 模板资源

模板在本 skill 的 `assets/project-template/` 下。使用时不要把模板内容重写成“语义等价”的版本；创建缺失文件时优先原样复制。

当前模板结构：

```text
assets/project-template/
├── AGENTS.md
├── CLAUDE.md
├── README.md
├── .cursor/
│   ├── mcp.json
│   └── rules/
│       └── karpathy-guidelines.mdc
└── docs/
    ├── README.md
    ├── spec-overview.md
    ├── spec-requirements.md
    ├── spec-design.md
    ├── spec-glossary.md
    ├── decisions/
    │   └── README.md
    └── changes/
        └── README.md
```

## 初始化流程

1. 扫描目标项目根目录，确认是否已有 `AGENTS.md`、`CLAUDE.md`、`.cursor/`、`README.md`、`docs/`。
2. 只创建缺失文件和缺失目录；不要覆盖用户已有内容。
3. 如果用户明确要求“按模板重置”或“同步原文”，先读现有文件，再按模板替换目标文件。
4. 复制模板时保留相对路径，尤其是 `.cursor/rules/karpathy-guidelines.mdc` 和 `.cursor/mcp.json`。
5. 新增或重命名文档后，同步更新目标项目的 `docs/README.md` 索引。

## 文档骨架

目标项目默认使用：

```text
docs/
├── README.md
├── spec-overview.md
├── spec-requirements.md
├── spec-design.md
├── decisions/
├── changes/
└── spec-glossary.md
```

占位文档保持简短，不要替用户编造项目事实。长期说明文档在 `docs/README.md` 中已有索引；除非用户要求，不要额外把文档目录约束写进 Agent 入口文件。

## 验证口径

- 检查目标项目是否存在三类入口：`AGENTS.md`、`CLAUDE.md`、`.cursor/rules/*.mdc`。
- 检查 `README.md` 和 `docs/` 是否按模板补齐。
- 检查新增或改名后的文档路径是否反映在 `docs/README.md`。
- 对文档类初始化，查看 diff 或文件列表即可；不要默认运行重型测试。

## 常见错误

| 错误 | 正确做法 |
|---|---|
| 覆盖已有项目规则 | 只补缺失；替换必须来自用户明确要求 |
| 只创建 `AGENTS.md` | 同时核对 Claude Code 和 Cursor 入口 |
| 把模板内容翻译或改写 | 缺失文件按模板原样复制 |
| 在根目录散落长期文档 | 长期文档放在 `docs/`，根目录保留入口文件 |
| 为项目编造需求或设计 | 只创建占位和索引，事实由用户或现有材料提供 |
