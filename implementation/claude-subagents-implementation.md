# 子代理实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026_07%3A59_PM_PKT-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-agent"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

天气代理在此仓库中实现，作为**命令 → 代理 → 技能**架构模式的示例，演示两种不同的技能模式。

---

## 天气代理

**文件**: [`.claude/agents/weather-agent.md`](../.claude/agents/weather-agent.md)

```yaml
---
name: weather-agent
description: 当您需要为阿联酋迪拜获取天气数据时，请主动使用此代理。此代理使用其
  预加载的 weather-fetcher 技能从 Open-Meteo 获取实时温度。
tools: WebFetch, Read, Write, Edit
model: sonnet
color: green
maxTurns: 5
permissionMode: acceptEdits
memory: project
skills:
  - weather-fetcher
---

# 天气代理

您是一个专门的天气代理，为迪拜、阿联酋获取天气数据。

## 您的任务

按照预加载技能的说明执行天气工作流：

1. **获取**: 按照 `weather-fetcher` 技能说明从 Open-Meteo 获取当前温度
2. **报告**: 将温度值和单位返回给调用者
3. **记忆**: 使用读数详情更新代理记忆以进行历史跟踪

...
```

该代理有一个预加载的技能 (`weather-fetcher`)，提供从 Open-Meteo 获取的说明。它将温度值和单位返回给调用命令。

---

## ![如何使用](../!/tags/how-to-use.svg)

```bash
$ claude
> 迪拜的天气如何?
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

您可以使用 `/agents` 命令创建代理，
```bash
$ claude
> /agents
```

或请求 Claude 为您创建一个——它将在 `.claude/agents/<name>.md` 中生成带有 YAML frontmatter 和正文的 markdown 文件

---

<a href="https://github.com/shanraisshan/claude-code-best-practice#orchestration-workflow"><img src="../!/tags/orchestration-workflow-hd.svg" alt="Orchestration Workflow"></a>

天气代理是命令 → 代理 → 技能编排模式中的**代理**。它从 `/weather-orchestrator` 命令接收工作流，并使用其预加载的技能 (`weather-fetcher`) 获取温度。然后命令调用独立的 `weather-svg-creator` 技能来创建可视化输出。

<p align="center">
  <img src="../orchestration-workflow/orchestration-workflow.svg" alt="命令技能代理架构流程" width="100%">
</p>

| 组件 | 角色 | 此仓库 |
|-----------|------|-----------|
| **命令** | 入口点、用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **代理** | 使用预加载技能获取数据（代理技能） | [`weather-agent`](../.claude/agents/weather-agent.md) 与 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **技能** | 独立创建输出（技能） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |
