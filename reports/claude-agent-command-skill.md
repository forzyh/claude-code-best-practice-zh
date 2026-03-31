# 代理 vs 命令 vs 技能 — 何时使用什么

Claude Code 中三种扩展机制的比较：子代理、命令和技能。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

![斜杠菜单显示 time-skill、time-command 和 time-agent](assets/agent-command-skill-1.jpg)

---

## 一目了然

| | 代理 | 命令 | 技能 |
|---|---|---|---|
| **位置** | `.claude/agents/<name>.md` | `.claude/commands/<name>.md` | `.claude/skills/<name>/SKILL.md` |
| **上下文** | 独立子代理进程 | 内联（主对话） | 内联（主对话） |
| **用户可调用** | 否 — 无 `/` 菜单 — 通过 Claude 或代理工具调用 | 是 — `/command-name` | 是 — `/skill-name`（除非 `user-invocable: false`） |
| **Claude 自动调用** | 是 — 通过 `description` 字段（使用"PROACTIVELY"鼓励） | 否 | 是 — 通过 `description` 字段（除非 `disable-model-invocation: true`） |
| **接受参数** | 通过 `prompt` 参数 | `$ARGUMENTS`、`$0`、`$1` | `$ARGUMENTS`、`$0`、`$1` |
| **动态上下文注入** | 否 | 是 — `` !`command` `` | 是 — `` !`command` `` |
| **自己的上下文窗口** | 是 — 隔离 | 否 — 共享主 | 否 — 共享主（除非 `context: fork`） |
| **模型覆盖** | `model:` 前置 | `model:` 前置 | `model:` 前置 |
| **工具限制** | `tools:` / `disallowedTools:` | `allowed-tools:` | `allowed-tools:` |
| **钩子** | `hooks:` 前置 | — | `hooks:` 前置 |
| **内存** | `memory:` 前置（user/project/local） | — | — |
| **可预加载技能** | 是 — `skills:` 前置 | — | — |
| **MCP 服务器** | `mcpServers:` 前置 | — | — |

---

## 何时使用各种机制

### 使用代理当：

- 任务**自主且多步** — 代理需要探索、决策和行动，无需持续指导
- 需要**上下文隔离** — 工作不应污染主对话窗口
- 代理需要**跨会话持久内存**（例如，学习模式的代码审查员）
- 希望**预加载域知识**通过技能而不杂乱主上下文
- 任务受益于**在后台运行**或在 **git worktree 中运行**
- 需要**工具限制**或**不同权限模式**（例如 `acceptEdits`、`plan`）

**示例**：`weather-agent` — 自主获取天气数据使用其预加载的 `weather-fetcher` 技能，在具有受限工具的独立上下文中运行。

### 使用命令当：

- 需要**用户启动的入口点** — 用户明确触发的工作流
- 工作流涉及**编排**其他代理或技能
- 希望**保持上下文精简** — 命令内容在用户触发前不会注入到会话上下文中

**示例**：`weather-orchestrator` — 用户触发它，它询问 C/F 偏好，调用代理，然后调用 SVG 技能。

### 使用技能当：

- 希望 **Claude 自动调用**基于用户意图 — 技能描述被注入到会话上下文中进行语义匹配
- 任务是**可重用的过程**可从多个地方调用（命令、代理或 Claude 本身）
- 需要**代理预加载** — 在启动时将域知识烘焙到特定代理中

**示例**：`weather-svg-creator` — 当用户要求天气卡时 Claude 自动调用它；也可从命令调用。

---

## 命令 → 代理 → 技能架构

此存储库演示了分层编排模式：

```
用户触发 /command
    ↓
命令编排工作流
    ↓
命令调用代理（独立上下文，自主）
    ↓
代理使用预加载技能（域知识）
    ↓
命令调用技能（内联，用于输出生成）
```

**具体示例** — 天气系统：

```
/weather-orchestrator（命令 — 入口点，询问 C/F）
    ↓
weather-agent（代理 — 自主获取温度）
    ├── weather-fetcher（代理技能 — 预加载 API 指令）
    ↓
weather-svg-creator（技能 — 内联创建 SVG）
```

---

## 前置比较

### 代理前置

```yaml
---
name: my-agent
description: PROACTIVELY 在...时使用此代理
tools: Read, Write, Edit, Bash
model: sonnet
maxTurns: 10
permissionMode: acceptEdits
memory: user
skills:
  - my-skill
---
```

### 命令前置

```yaml
---
description: 做有用的事
argument-hint: [issue-number]
allowed-tools: Read, Edit, Bash(gh *)
model: sonnet
---
```

### 技能前置

```yaml
---
name: my-skill
description: 当用户要求...时做某事
argument-hint: [file-path]
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Glob
model: sonnet
context: fork
agent: general-purpose
---
```

---

## 关键区别

### 自动调用

| 机制 | Claude 能自动调用吗？ | 如何防止 |
|-----------|------------------------|----------------|
| 代理 | 是 — 通过 `description`（使用"PROACTIVELY"鼓励） | 移除或软化描述 |
| 命令 | 否 — 始终通过 `/` 用户启动 | N/A |
| 技能 | 是 — 通过 `description` | 设置 `disable-model-invocation: true` |

### `/` 菜单中的可见性

| 机制 | 出现在 `/` 菜单中？ | 如何隐藏 |
|-----------|---------------------|-------------|
| 代理 | 否 | N/A |
| 命令 | 是 — 始终 | 无法隐藏 |
| 技能 | 是 — 默认 | 设置 `user-invocable: false` |

### 上下文隔离

| 机制 | 在自己的上下文中运行吗？ | 如何配置 |
|-----------|---------------------|-----------------|
| 代理 | 始终 | 内置行为 |
| 命令 | 从不 | N/A |
| 技能 | 可选 | 设置 `context: fork` |

---

## 实际工作示例："现在是几点？"

此存储库对同一任务定义了所有三种机制 — 以 PKT 显示当前时间。以下是当用户输入**"现在是几点？"**而不显式调用任何 `/` 命令时发生的情况：

| 机制 | 会触发吗？ | 为什么 / 为什么不 |
|-----------|--------------|---------------|
| `time-command` | 否 | 命令**从不自动调用**。用户需要显式键入 `/time-command` 才能运行。命令没有自动发现路径 — 它们严格用户启动。 |
| `time-agent` | **可能是** | 代理的 `description` 说*"使用此代理以巴基斯坦标准时间显示当前时间"*。Claude 将其与用户意图匹配，可能通过代理工具生成它。然而，代理在**独立上下文窗口**中运行，对于这个简单任务来说太重。 |
| `time-skill` | **最可能是** | 技能的 `description` 说*"以巴基斯坦标准时间 (PKT, UTC+5) 显示当前时间。当用户要求当前时间、巴基斯坦时间或 PKT 时使用。"* Claude 将其匹配并通过技能工具调用。由于它**内联运行**无上下文开销，它是最有效的匹配。 |

### 解析顺序

当多个机制匹配同一意图时，Claude 优先选择**最轻量级的选项**满足请求：

```
1. 技能（内联，无上下文开销）     ← 首选
2. 代理（独立上下文，自主）    ← 如果技能不可用或任务复杂则使用
3. 命令（从不 — 需要显式 /）   ← 仅当用户键入 /time-command 时
```

### 如果在技能上设置 `disable-model-invocation: true` 会怎样？

然后 Claude **无法**自动调用技能。代理成为唯一自动调用选项，所以 Claude 将生成 `time-agent` — 为了单行 bash 命令的代价而使用独立上下文窗口。

### 如果技能和代理都禁用了自动调用会怎样？

然后**什么都不会自动触发**。Claude 会降级到其自己的通用知识，可能直接运行 `TZ='Asia/Karachi' date` — 不涉及扩展机制。用户需要显式键入 `/time-command` 或 `/time-skill` 来使用其中一个。

![Claude 当用户问"现在是几点？"时自动调用 time-skill](assets/agent-command-skill-2.png)

---

## 来源

- [Claude Code 技能 — 文档](https://code.claude.com/docs/en/skills)
- [Claude Code 子代理 — 文档](https://code.claude.com/docs/en/sub-agents)
- [Claude Code 斜杠命令 — 文档](https://code.claude.com/docs/en/slash-commands)
- [技能最佳实践](../best-practice/claude-skills.md)
- [命令最佳实践](../best-practice/claude-commands.md)
- [子代理最佳实践](../best-practice/claude-subagents.md)
