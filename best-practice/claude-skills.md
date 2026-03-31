# 技能最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2028%2C%202026%205%3A59%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-skills-implementation.md)

Claude Code 技能 — 前言字段和官方打包技能。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 前言字段（13个）

| 字段 | 类型 | 必需 | 说明 |
|-------|------|----------|-------------|
| `name` | string | 否 | 显示名称和 `/斜杠命令` 标识符。如果省略，默认为目录名 |
| `description` | string | 推荐 | 技能的功能。显示在自动完成中，由 Claude 用于自动发现 |
| `argument-hint` | string | 否 | 自动完成时显示的提示（例如 `[issue-number]`、`[filename]`） |
| `disable-model-invocation` | boolean | 否 | 设置 `true` 以防止 Claude 自动调用此技能 |
| `user-invocable` | boolean | 否 | 设置 `false` 以从 `/` 菜单中隐藏 — 技能仅成为背景知识，用于代理预加载 |
| `allowed-tools` | string | 否 | 此技能活跃时无需权限提示即可使用的工具 |
| `model` | string | 否 | 此技能运行时使用的模型（例如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | 否 | 调用时覆盖模型效能级别（`low`、`medium`、`high`、`max`） |
| `context` | string | 否 | 设置为 `fork` 以在隔离的子代理上下文中运行技能 |
| `agent` | string | 否 | 设置 `context: fork` 时的子代理类型（默认：`general-purpose`） |
| `hooks` | object | 否 | 此技能范围内的生命周期钩子 |
| `paths` | string/list | 否 | Glob 模式，限制何时技能自动激活。接受逗号分隔的字符串或 YAML 列表 — Claude 仅在处理匹配模式的文件时加载技能 |
| `shell` | string | 否 | `` !`command` `` 块的 Shell — `bash`（默认）或 `powershell`。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |

---

## ![Official](../!/tags/official.svg) **(5)**

| # | 技能 | 说明 |
|---|-------|-------------|
| 1 | `simplify` | 审查变更代码以查找重用、质量和效率 — 重构以消除重复 |
| 2 | `batch` | 跨多个文件批量运行命令 |
| 3 | `debug` | 调试失败命令或代码问题 |
| 4 | `loop` | 在循环间隔（最多 3 天）上运行提示或斜杠命令 |
| 5 | `claude-api` | 使用 Claude API 或 Anthropic SDK 构建应用 — 触发 `anthropic` / `@anthropic-ai/sdk` 导入 |

另见：[官方技能仓库](https://github.com/anthropics/skills/tree/main/skills)用于社区维护的可安装技能。

---

## 来源

- [Claude Code 技能 — 文档](https://code.claude.com/docs/en/skills)
- [Monorepo 中的技能发现](../reports/claude-skills-for-larger-mono-repos.md)
- [Claude Code 更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
