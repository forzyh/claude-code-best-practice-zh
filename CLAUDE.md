# CLAUDE.md

本文件为使用本仓库中 Claude Code（claude.ai/code）提供指导。

## 仓库概览

这是一个 Claude Code 配置最佳实践仓库，展示了技能（skills）、代理（subagents）、钩子（hooks）和命令（commands）的模式。它是一个参考实现，而不是应用程序代码库。

## 主要组件

### 天气系统（示例工作流）
通过 **命令 → 代理 → 技能** 架构展示两种不同的技能模式：
- `/weather-orchestrator` 命令（`.claude/commands/weather-orchestrator.md`）：入口点 — 询问用户摄氏度/华氏度，调用代理，然后调用 SVG 技能
- `weather-agent` 代理（`.claude/agents/weather-agent.md`）：使用其预加载的 `weather-fetcher` 技能（代理技能模式）获取温度
- `weather-fetcher` 技能（`.claude/skills/weather-fetcher/SKILL.md`）：预加载到代理中 — 从 Open-Meteo 获取温度的指导
- `weather-svg-creator` 技能（`.claude/skills/weather-svg-creator/SKILL.md`）：技能 — 创建 SVG 天气卡片，写入 `orchestration-workflow/weather.svg` 和 `orchestration-workflow/output.md`

两种技能模式：代理技能（通过 `skills:` 字段预加载）与技能（通过 `Skill` 工具调用）。完整流程图见 `orchestration-workflow/orchestration-workflow.md`。

### 技能定义结构
`.claude/skills/<name>/SKILL.md` 中的技能使用 YAML frontmatter：
- `name`：显示名称和 `/斜杠命令`（默认为目录名）
- `description`：何时调用（推荐用于自动发现）
- `argument-hint`：自动完成提示（例如 `[issue-number]`）
- `disable-model-invocation`：设置 `true` 以防止自动调用
- `user-invocable`：设置 `false` 以从 `/` 菜单中隐藏（仅背景知识）
- `allowed-tools`：技能激活时允许的工具
- `model`：技能激活时使用的模型
- `context`：设置为 `fork` 以在隔离的代理上下文中运行
- `agent`：用于 `context: fork` 的代理类型（默认：`general-purpose`）
- `hooks`：限定于此技能的生命周期钩子

### 演示系统
见 `.claude/rules/presentation.md` — 所有演示工作都委派给 `presentation-curator` 代理。

### 钩子系统
`.claude/hooks/` 中的跨平台声音通知系统：
- `scripts/hooks.py`：Claude Code 钩子事件的主处理程序
- `config/hooks-config.json`：共享团队配置
- `config/hooks-config.local.json`：个人覆盖（git 忽略）
- `sounds/`：按钩子事件组织的音频文件（通过 ElevenLabs TTS 生成）

在 `.claude/settings.json` 中配置的钩子事件：PreToolUse、PostToolUse、UserPromptSubmit、Notification、Stop、SubagentStart、SubagentStop、PreCompact、SessionStart、SessionEnd、Setup、PermissionRequest、TeammateIdle、TaskCompleted、ConfigChange。

特殊处理：git 提交触发 `pretooluse-git-committing` 声音。

## 关键模式

### 代理编排
代理**不能**通过 bash 命令调用其他代理。使用 Agent 工具（在 v2.1.63 中从 Task 重命名；`Task(...)` 仍作为别名工作）：
```
Agent(subagent_type="agent-name", description="...", prompt="...", model="haiku")
```

在代理定义中明确说明工具用法。避免使用可能被误解为 bash 命令的模糊术语，如"launch"。

### 代理定义结构
`.claude/agents/*.md` 中的代理使用 YAML frontmatter：
- `name`：代理标识符
- `description`：何时调用（使用"PROACTIVELY"进行自动调用）
- `tools`：工具的逗号分隔允许列表（如果省略则继承所有）。支持 `Agent(agent_type)` 语法
- `disallowedTools`：要拒绝的工具，从继承或指定的列表中移除
- `model`：模型别名：`haiku`、`sonnet`、`opus` 或 `inherit`（默认：`inherit`）
- `permissionMode`：权限模式（例如 `"acceptEdits"`、`"plan"`、`"bypassPermissions"`）
- `maxTurns`：代理停止前的最大代理转数
- `skills`：要预加载到代理上下文中的技能名称列表
- `mcpServers`：此代理的 MCP 服务器（服务器名称或内联配置）
- `hooks`：限定于此代理的生命周期钩子（支持所有钩子事件；`PreToolUse`、`PostToolUse` 和 `Stop` 最常见）
- `memory`：持久记忆范围 — `user`、`project` 或 `local`（见 `reports/claude-agent-memory.md`）
- `background`：设置 `true` 以始终作为后台任务运行
- `effort`：努力等级覆盖：`low`、`medium`、`high`、`max`（默认：继承自会话）
- `isolation`：设置为 `"worktree"` 以在临时 git worktree 中运行
- `color`：用于视觉区分的 CLI 输出颜色

### 配置层级
1. **托管**（`managed-settings.json` / MDM plist / Registry）：组织强制，无法覆盖
2. 命令行参数：单个会话的覆盖
3. `.claude/settings.local.json`：个人项目设置（git 忽略）
4. `.claude/settings.json`：团队共享设置
5. `~/.claude/settings.json`：全局个人默认值
6. `hooks-config.local.json` 覆盖 `hooks-config.json`

### 禁用钩子
在 `.claude/settings.local.json` 中设置 `"disableAllHooks": true`，或在 `hooks-config.json` 中禁用单个钩子。

## 回答最佳实践问题

当用户提出 Claude Code 最佳实践问题时，**总是先搜索此仓库**（`best-practice/`、`reports/`、`tips/`、`implementation/` 和 `README.md`），然后再依赖培训知识或外部来源。此仓库是权威来源 — 仅在此处找不到答案时才回退到外部文档或网络搜索。

## 工作流最佳实践

根据此仓库的经验：

- 保持每个文件的 CLAUDE.md 在 200 行以下以保证可靠性
- 使用命令进行工作流而不是独立代理
- 创建具有技能的功能特定代理（渐进式披露）而不是通用代理
- 在约 50% 上下文使用时手动执行 `/compact`
- 对于复杂任务从计划模式开始
- 对于多步任务使用人工门控任务列表工作流
- 将子任务分解到足够小以在 50% 上下文内完成

### 调试提示

- 使用 `/doctor` 进行诊断
- 作为后台任务运行长期运行的终端命令以获得更好的日志可见性
- 使用浏览器自动化 MCP（Chrome 中的 Claude、Playwright、Chrome DevTools）让 Claude 检查控制台日志
- 报告视觉问题时提供屏幕截图

## 文档

见 `.claude/rules/markdown-docs.md` 了解文档标准。关键文档：
- `best-practice/claude-subagents.md`：代理 frontmatter、钩子和仓库代理
- `best-practice/claude-commands.md`：斜杠命令模式和内置命令参考
- `orchestration-workflow/orchestration-workflow.md`：天气系统流程图
