# 命令实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-orchestrator"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

天气编排命令在此仓库中实现，作为**命令 → 代理 → 技能**架构模式的入口点，演示命令如何编排多步工作流。

---

## 天气编排器

**文件**: [`.claude/commands/weather-orchestrator.md`](../.claude/commands/weather-orchestrator.md)

```yaml
---
description: 获取迪拜的天气数据并创建 SVG 天气卡
model: haiku
---

# 天气编排器命令

获取阿联酋迪拜的当前温度，并创建视觉 SVG 天气卡。

## 工作流

### 步骤 1: 询问用户偏好
使用 AskUserQuestion 工具询问用户是否希望
以摄氏度或华氏度显示温度。

### 步骤 2: 获取天气数据
使用代理工具调用天气代理：
- subagent_type: weather-agent
- prompt: 获取阿联酋迪拜当前温度 [单位]...

### 步骤 3: 创建 SVG 天气卡
使用技能工具调用 weather-svg-creator 技能：
- skill: weather-svg-creator

...
```

命令编排整个工作流：要求用户输入温度单位偏好，通过代理工具调用 `weather-agent`，然后通过技能工具调用 `weather-svg-creator` 技能。

---

## ![如何使用](../!/tags/how-to-use.svg)

```bash
$ claude
> /weather-orchestrator
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

请求 Claude 为您创建一个——它将在 `.claude/commands/<name>.md` 中生成带有 YAML frontmatter 和正文的 markdown 文件

---

<a href="https://github.com/shanraisshan/claude-code-best-practice#orchestration-workflow"><img src="../!/tags/orchestration-workflow-hd.svg" alt="Orchestration Workflow"></a>

天气编排器是命令 → 代理 → 技能编排模式中的**命令**。它充当入口点——处理用户交互（温度单位偏好），将数据获取委托给 `weather-agent`，并调用 `weather-svg-creator` 技能进行可视化输出。

<p align="center">
  <img src="../orchestration-workflow/orchestration-workflow.svg" alt="命令技能代理架构流程" width="100%">
</p>

| 组件 | 角色 | 此仓库 |
|-----------|------|-----------|
| **命令** | 入口点、用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **代理** | 使用预加载技能获取数据（代理技能） | [`weather-agent`](../.claude/agents/weather-agent.md) 与 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **技能** | 独立创建输出（技能） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |
