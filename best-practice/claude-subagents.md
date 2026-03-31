# 子代理最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2028%2C%202026%206%3A00%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-subagents-implementation.md)

Claude Code 子代理 — 前言字段和官方内置代理类型。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 前言字段（16个）

| 字段 | 类型 | 必需 | 说明 |
|-------|------|----------|-------------|
| `name` | string | 是 | 使用小写字母和连字符的唯一标识符 |
| `description` | string | 是 | 何时调用。使用 `"PROACTIVELY"` 以由 Claude 自动调用 |
| `tools` | string/list | 否 | 逗号分隔的工具允许列表（例如 `Read, Write, Edit, Bash`）。如果省略，继承所有工具。支持 `Agent(agent_type)` 语法限制可衍生的子代理；较旧的 `Task(agent_type)` 别名仍然有效 |
| `disallowedTools` | string/list | 否 | 工具被拒绝，从继承或指定列表中移除 |
| `model` | string | 否 | 模型别名：`haiku`、`sonnet`、`opus` 或 `inherit`（默认：`inherit`） |
| `permissionMode` | string | 否 | 权限模式：`default`、`acceptEdits`、`dontAsk`、`bypassPermissions` 或 `plan` |
| `maxTurns` | integer | 否 | 子代理停止前的最大代理转数 |
| `skills` | list | 否 | 在启动时预加载到代理上下文的技能名称（完整内容注入，不仅仅是可用） |
| `mcpServers` | list | 否 | 此子代理的 MCP 服务器 — 服务器名称字符串或内联 `{name: config}` 对象 |
| `hooks` | object | 否 | 此子代理范围内的生命周期钩子。所有钩子事件都被支持；`PreToolUse`、`PostToolUse` 和 `Stop` 是最常见的 |
| `memory` | string | 否 | 持久内存范围：`user`、`project` 或 `local` |
| `background` | boolean | 否 | 设置为 `true` 以始终作为后台任务运行（默认：`false`） |
| `effort` | string | 否 | 当此子代理活跃时的效能级别覆盖：`low`、`medium`、`high`、`max`。默认：继承自会话 |
| `isolation` | string | 否 | 设置为 `"worktree"` 以在临时 git worktree 中运行（无更改时自动清理） |
| `initialPrompt` | string | 否 | 当此代理作为主会话代理运行时自动提交为第一个用户转（通过 `--agent` 或 `agent` 设置）。命令和技能被处理。前缀为任何用户提供的提示 |
| `color` | string | 否 | CLI 输出颜色以视觉区分（例如 `green`、`magenta`）。功能但不在官方前言表中 — 仅在交互式快速启动中记录 |

---

## ![Official](../!/tags/official.svg) **(6)**

| # | 代理 | 模型 | 工具 | 说明 |
|---|-------|-------|-------|-------------|
| 1 | `general-purpose` | inherit | 全部 | 复杂多步任务 — 默认代理类型，用于研究、代码搜索和自主工作 |
| 2 | `Explore` | haiku | 只读（无 Write、Edit） | 快速代码库搜索和探索 — 优化文件查找、代码搜索和回答代码库问题 |
| 3 | `Plan` | inherit | 只读（无 Write、Edit） | 在计划模式中的预计划研究 — 探索代码库并在编写代码前设计实现方法 |
| 4 | `Bash` | inherit | Bash | 在单独上下文中运行终端命令 |
| 5 | `statusline-setup` | sonnet | Read、Edit | 配置用户的 Claude Code 状态行设置 |
| 6 | `claude-code-guide` | haiku | Glob、Grep、Read、WebFetch、WebSearch | 回答关于 Claude Code 功能、Agent SDK 和 Claude API 的问题 |

---

## 来源

- [创建自定义子代理 — Claude Code 文档](https://code.claude.com/docs/en/sub-agents)
- [CLI 参考 — Claude Code 文档](https://code.claude.com/docs/en/cli-reference)
- [Claude Code 更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
