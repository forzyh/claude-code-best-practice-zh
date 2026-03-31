---
name: weather-svg-creator
description: 创建显示迪拜当前温度的SVG天气卡。写入SVG到orchestration-workflow/weather.svg并更新orchestration-workflow/output.md。
---

# 天气SVG创建器技能

为迪拜阿联酋创建视觉SVG天气卡并写入输出文件。

## 任务

您将从调用上下文中接收温度值和单位 (摄氏度或华氏度)。创建SVG天气卡并写入SVG和markdown摘要。

## 说明

1. **创建SVG** — 使用 [reference.md](reference.md) 中的SVG模板，用实际值替换占位符
2. **写入SVG文件** — 读取然后写入 `orchestration-workflow/weather.svg`
3. **写入摘要** — 读取然后写入 `orchestration-workflow/output.md` 使用 [reference.md](reference.md) 中的markdown模板

## 规则

- 使用提供的准确温度值和单位 — 不重新获取或修改
- SVG必须是自包含的且有效
- 两个输出文件都放在 `orchestration-workflow/` 目录中

## 附加资源

- 关于SVG模板、输出模板和设计规格，参见 [reference.md](reference.md)
- 关于示例输入/输出对，参见 [examples.md](examples.md)
