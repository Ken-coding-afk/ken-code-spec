# ken-code-spec

本项目已初始化轻量协作入口、编码规则和文档目录。

## 协作入口

- Codex: `AGENTS.md`
- Claude Code: `CLAUDE.md`
- Cursor: `.cursor/rules/karpathy-guidelines.mdc`

## 开发方式

- 普通改动先在对话中说明怎么改、影响哪里、如何验证，确认后再执行。
- 大改动先解决关键疑问，再生成一份 `docs/plans/YYYY-MM-DD-主题-plan.md`，确认后再执行。
- AI 无法确定会影响结果的需求或技术选择时，必须提出问题并等待用户决定。
- 先完成整个任务的代码，再集中补充单元测试。
- 代码和测试完成后，只运行与本次改动相关的测试文件、用例或受影响 package/workspace；除非用户明确要求，否则不运行完整测试套件。

## 文档目录

项目文档统一放在 `docs/` 下。长期说明文档使用 `spec-*.md` 命名；大改动计划、决策记录和变更记录分别放在 `docs/plans/`、`docs/decisions/` 与 `docs/changes/` 下。
