---
description: 获取迪拜的天气数据并创建SVG天气卡
model: haiku
---

# 天气编排命令

获取迪拜阿联酋的当前温度并创建视觉SVG天气卡。

## 工作流

### 第1步：询问用户偏好

使用AskUserQuestion工具询问用户是否想要摄氏度或华氏度的温度。

### 第2步：获取天气数据

使用Task工具调用天气代理：
- subagent_type: weather-agent
- description: 获取迪拜天气数据
- prompt: 获取迪拜阿联酋的当前温度 [用户请求的单位]。返回数值温度值和单位。该代理有一个预加载的技能 (weather-fetcher) 提供详细说明。
- model: haiku

等待代理完成并捕获返回的温度值和单位。

### 第3步：创建SVG天气卡

使用Skill工具调用weather-svg-creator技能：
- skill: weather-svg-creator

技能将使用第2步的温度值和单位 (在当前上下文中可用) 创建SVG卡并写入输出文件。

## 关键需求

1. **使用Task工具用于代理**: 不使用bash命令调用代理。您必须使用Task工具。
2. **使用Skill工具用于SVG创建器**: 通过Skill工具调用SVG创建器，不是Task工具。
3. **传递用户偏好**: 在调用代理时包括用户的温度单位偏好。
4. **顺序流**: 在移动到下一个之前完成每个步骤。

## 输出摘要

为用户提供清晰的摘要显示：
- 请求的温度单位
- 从迪拜获取的温度
- 在 `orchestration-workflow/weather.svg` 创建的SVG卡
- 摘要写入 `orchestration-workflow/output.md`
