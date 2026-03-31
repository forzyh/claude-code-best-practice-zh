---
model: haiku
---

# 时间协调员命令

获取迪拜的当前时间（Asia/Dubai，UTC+4）并创建一个可视 SVG 时间卡。

## 工作流

### 第 1 步：获取当前迪拜时间

使用代理工具调用时间代理：
- subagent_type：time-agent
- description：获取当前迪拜时间
- prompt：获取迪拜当前时间（Asia/Dubai，UTC+4）。精确返回三个字段：`time`（时间部分，例如 "14:30:45"）、`timezone`（"GST (UTC+4)"）和 `formatted`（完整格式化字符串，例如 "2026-03-12 14:30:45 +04"）。代理具有预加载的技能 (time-fetcher)，提供详细说明。
- model：haiku

等待代理完成并捕获返回的时间数据。

### 数据契约

time-agent 必须返回这三个字段：
- **time**：时间部分（例如 "14:30:45"）
- **timezone**："GST (UTC+4)"
- **formatted**：完整格式化字符串（例如 "2026-03-12 14:30:45 +04"）

### 第 2 步：创建 SVG 时间卡

使用技能工具调用 time-svg-creator 技能：
- skill：time-svg-creator
- args：传递来自第 1 步的时间数据 — 包括 `time`、`timezone` 和 `formatted` 值

技能将使用来自第 1 步的时间数据（在当前上下文中可用）创建 SVG 卡并写入输出文件。

## 关键要求

1. **对 time-agent 使用代理工具**：不要使用 bash 命令调用代理。你必须使用代理工具 subagent_type："time-agent"。
2. **对 SVG Creator 使用技能工具**：使用技能工具 skill："time-svg-creator" 调用 SVG creator，不是代理工具。
3. **顺序流**：代理必须完成并返回时间数据后，才能调用技能。不要并行运行它们。
4. **数据传递**：确保来自代理响应的所有三个字段（time、timezone、formatted）在调用技能时在上下文中可用。

## 输出摘要

两个步骤都完成后，向用户提供清晰摘要显示：
- 获取的当前迪拜时间
- 时区：GST (UTC+4)
- 完整格式化时间戳
- 在 `agent-teams/output/dubai-time.svg` 创建的 SVG 卡
- 写入 `agent-teams/output/output.md` 的摘要
