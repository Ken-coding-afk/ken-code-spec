# ken-code-spec

本项目已初始化轻量协作入口、编码规则和文档目录。

## 协作入口

- Codex: `AGENTS.md`
- Claude Code: `CLAUDE.md`
- Cursor: `.cursor/rules/karpathy-guidelines.mdc`

## 开发方式

- 实际操作前先给出 3～6 条简略方案。
- 普通改动直接执行，不生成正式方案文档。
- 大改动先在 `docs/plans/` 创建方案文档和执行文档，确认后再编码。
- 先完成整个任务的代码，再集中补充单元测试。
- 代码和测试完成后，统一运行模块测试、完整测试套件和适用检查。

## 文档目录

项目文档统一放在 `docs/` 下。长期说明文档使用 `spec-*.md` 命名；大改动计划、决策记录和变更记录分别放在 `docs/plans/`、`docs/decisions/` 与 `docs/changes/` 下。
