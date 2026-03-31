---
name: time-svg-creator
description: 为迪拜创建一个显示当前时间的 SVG 时间卡。将 SVG 写入 agent-teams/output/dubai-time.svg 并更新 agent-teams/output/output.md。
allowed-tools: Write, Read
---

# 时间 SVG Creator 技能

为迪拜、阿联酋创建一个可视 SVG 时间卡并写入输出文件。

## 任务

你将从调用上下文接收三个字段：`time`、`timezone` 和 `formatted`。创建一个 SVG 时间卡并写入两个输出文件。

## 说明

1. **创建 SVG** — 使用 [reference.md](reference.md) 中的 SVG 模板，用实际值替换占位符
2. **写 SVG 文件** — 写入 `agent-teams/output/dubai-time.svg`
3. **写摘要** — 写入 `agent-teams/output/output.md` 使用 [reference.md](reference.md) 中的 markdown 模板

## 规则

- 使用提供的精确时间值 — 永远不要重新获取或重新计算
- SVG 必须是自包含和有效的
- 两个输出文件都进入 `agent-teams/output/` 目录

## 其他资源

- 对于 SVG 模板、输出模板和设计规格，请参阅 [reference.md](reference.md)
- 对于示例输入/输出对，请参阅 [examples.md](examples.md)
