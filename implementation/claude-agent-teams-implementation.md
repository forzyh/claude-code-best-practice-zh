# Agent Teams 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_12%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#time-orchestration"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

<p align="center">
  <img src="assets/impl-agent-teams.png" alt="Agent Teams 在行动中——tmux 分屏模式" width="100%">
</p>

Agent Teams 生成**多个独立的 Claude Code 会话**，通过共享任务列表进行协调。与子代理（单个会话内的隔离上下文分支）不同，每个团队成员都获得自己的完整上下文窗口，并自动加载 CLAUDE.md、MCP 服务器和技能。

---

## ![如何使用](../!/tags/how-to-use.svg)

时间编排工作流完全由代理团队构建。要运行成品：

```bash
cd agent-teams
claude
/time-orchestrator
```

这调用**命令 → 代理 → 技能**流水线：代理获取迪拜的当前时间，技能将 SVG 时间卡渲染到 `agent-teams/output/dubai-time.svg`。

---

## ![如何实现](../!/tags/how-to-implement.svg)

您可以使用代理团队创建天气编排工作流的副本——在这个示例中，时间编排工作流完全由代理团队构建。

### 1. 安装 [iTerm2](https://iterm2.com/) 和 tmux

```bash
brew install --cask iterm2
brew install tmux
```

### 2. 启动 iTerm2 → tmux → Claude

```bash
tmux new -s dev
CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1 claude
```

### 3. 使用团队结构提示

<a id="time-orchestration"></a>

将此提示粘贴到 Claude 中，以启动使用代理团队的完整时间编排工作流：

主提示：**[agent-teams-prompt.md](../agent-teams/agent-teams-prompt.md)**

### 团队协调流程

```
┌──────────────────────────────────────────────────────────────┐
│                         领导 (您)                             │
│         "创建代理团队来构建时间编排"                         │
└──────────────────────────┬───────────────────────────────────┘
                           │ 生成团队 (全部并行)
              ┌────────────┼────────────┐
              ▼            ▼            ▼
   ┌────────────────┐ ┌──────────┐ ┌──────────────┐
   │ 命令           │ │ 代理     │ │ 技能         │
   │ 架构师         │ │ 工程师   │ │ 设计师       │
   │                │ │          │ │              │
   │ agent-teams/   │ │ agent-   │ │ agent-teams/ │
   │ .claude/       │ │ teams/   │ │ .claude/     │
   │ commands/      │ │ .claude/ │ │ skills/      │
   │ time-          │ │ agents/  │ │ time-svg-    │
   │ orchestrator.md│ │ time-    │ │ creator/     │
   │                │ │ agent.md │ │              │
   └───────┬────────┘ └────┬─────┘ └──────┬───────┘
           │               │              │
           ▼               ▼              ▼
   ┌──────────────────────────────────────────────────┐
   │            共享任务列表                          │
   │  ☐ 同意数据契约: {time, tz, formatted}          │
   │  ☐ 命令使用代理工具 (不是 bash)                 │
   │  ☐ 代理预加载 time-fetcher 技能                 │
   │  ☐ 技能从上下文读取时间 (不重新获取)            │
   │  ☐ 所有文件在 agent-teams/.claude/ 内           │
   └──────────────────────────────────────────────────┘
                       │
                       ▼
          ┌──────────────────────────────┐
          │  cd agent-teams && claude    │
          │    /time-orchestrator        │
          │   命令 → 代理 → 技能         │
          └──────────────────────────────┘
```

