# 技能实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-svg-creator"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

此仓库中实现了两项技能，作为**命令 → 代理 → 技能**架构模式的一部分，演示了两种不同的技能调用模式：**代理技能**（预加载）和**技能**（直接调用）。

---

## 天气 SVG 创建者（技能）

**文件**: [`.claude/skills/weather-svg-creator/SKILL.md`](../.claude/skills/weather-svg-creator/SKILL.md)

```yaml
---
name: weather-svg-creator
description: 创建显示迪拜当前温度的 SVG 天气卡。
  将 SVG 写入 orchestration-workflow/weather.svg 并更新
  orchestration-workflow/output.md。
---

# 天气 SVG 创建者技能

此技能创建可视化 SVG 天气卡并写入输出文件。

## 任务
创建显示阿联酋迪拜温度的 SVG 天气卡，
并将其与摘要一起写入输出文件。

## 说明
您将从调用上下文接收温度值和单位
(摄氏度或华氏度)。

### 1. 创建 SVG 天气卡
生成干净的 SVG 天气卡...

### 2. 写入 SVG 文件
将 SVG 内容写入 `orchestration-workflow/weather.svg`。

### 3. 写入输出摘要
写入 `orchestration-workflow/output.md`...

...
```

这是一项**技能**——由命令通过技能工具直接调用。它从对话上下文接收温度数据，并创建 SVG 天气卡和输出摘要。

---

## 天气获取器（代理技能）

**文件**: [`.claude/skills/weather-fetcher/SKILL.md`](../.claude/skills/weather-fetcher/SKILL.md)

```yaml
---
name: weather-fetcher
description: 从 Open-Meteo API 获取阿联酋迪拜当前天气温度数据的说明
user-invocable: false
---

# 天气获取器技能

此技能提供获取当前天气数据的说明。

## 任务
以请求的单位 (摄氏度或华氏度) 获取阿联酋迪拜的当前温度。

## 说明
1. 获取天气数据：使用 WebFetch 工具获取当前天气数据
   - 摄氏度 URL: https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=celsius
   - 华氏度 URL: https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=fahrenheit
2. 提取温度：从 JSON 响应中提取 `current.temperature_2m`
3. 返回结果：清晰地返回温度值和单位。

...
```

这是一项**代理技能**——在启动时通过 `skills:` frontmatter 字段预加载到 `weather-agent` 中。它不是直接调用的；相反，它充当注入代理上下文的域知识。注意 `user-invocable: false` 会从 `/` 命令菜单中隐藏它。

---

## 两种技能模式

| 模式 | 调用 | 示例 | 主要差异 |
|---------|-----------|---------|----------------|
| **技能** | `Skill(skill: "name")` | `weather-svg-creator` | 通过技能工具直接调用 |
| **代理技能** | 通过 `skills:` 字段预加载 | `weather-fetcher` | 在启动时注入代理上下文 |

---

## ![如何使用](../!/tags/how-to-use.svg)

**技能**——通过斜杠命令直接调用：
```bash
$ claude
> /weather-svg-creator
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

请求 Claude 为您创建一个——它将在 `.claude/skills/my-skill/SKILL.md` 中生成带有 YAML frontmatter 和正文的 markdown 文件

# 我的技能

关于此技能执行操作的说明。
```
