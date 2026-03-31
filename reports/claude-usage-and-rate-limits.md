# Claude Code：使用、速率限制和额外使用

理解 Claude Code 中的使用限制如何工作，以及当达到限制时如何继续工作。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

订阅计划上的 Claude Code（Pro、Max 5x、Max 20x）有使用滚动窗口重置的限制。三个内置斜杠命令帮助你监控和管理使用：

| 命令 | 描述 | 适用于 |
|---------|-------------|--------------|
| `/usage` | 检查计划限制和速率限制状态 | Pro、Max 5x、Max 20x |
| `/extra-usage` | 配置达到限制时的按需付费溢出 | Pro、Max 5x、Max 20x |
| `/cost` | 显示当前会话的令牌使用和支出 | API 密钥用户 |

---

## `/usage` — 检查你的限制

显示当前计划的使用限制和速率限制状态。有用于检查在达到限制前还有多少容量。

---

## `/extra-usage` — 继续超出限制

`/extra-usage` 命令配置**按需付费溢出计费**，以便 Claude Code 在达到计划速率限制时继续无中断地工作，而不是阻止你。

### 工作原理

1. 你达到计划的速率限制（限制每 5 小时重置）
2. 如果启用了额外使用且有可用资金，Claude Code 继续无中断
3. 溢出令牌按**标准 API 率**计费，与订阅费分开

### 设置

CLI 中的 `/extra-usage` 命令会引导你完成配置。你也可以在 claude.ai 上的**设置 > 使用**配置：

1. 启用额外使用
2. 添加付款方式
3. 设置**月度支出上限**（或选择无限制）
4. 可选地添加**预付资金**，当余额下降到阈值以下时自动重新加载

### 关键详情

| 详情 | 值 |
|--------|-------|
| 日常赎回限制 | $2,000/日 |
| 计费 | 与订阅分开，按标准 API 率 |
| 限制重置窗口 | 每 5 小时 |

### 已知问题

截至 2026 年 2 月，`/extra-usage` CLI 命令[无文档](https://github.com/anthropics/claude-code/issues/12396)，可能会打开没有清晰配置选项的登录窗口。通过 **claude.ai Web 界面**配置是目前更可靠的路径。

---

## `/cost` — 会话支出（API 用户）

对于使用 API 密钥身份验证的用户（不是订阅计划），`/cost` 显示：

- 当前会话的总成本
- API 持续时间和实际时间
- 令牌使用分解
- 进行的代码更改

此命令与 Pro/Max 订阅用户无关。

---

## 快速模式和额外使用

快速模式（`/fast`）使用 Claude Opus 4.6 with faster output。它与额外使用有特殊的计费关系：

- 快速模式使用**始终从第一个令牌计费到额外使用**
- 即使你的订阅计划上有剩余使用
- 快速模式不消耗你计划的包含速率限制

这意味着你需要启用和资金充足的额外使用才能使用 `/fast`。

---

## CLI 启动标志

两个启动标志与使用预算相关（仅 API 密钥用户，打印模式）：

| 标志 | 描述 |
|------|-------------|
| `--max-budget-usd <AMOUNT>` | API 调用的最大美元金额后停止 |
| `--max-turns <NUMBER>` | 限制代理转数 |

查看 [CLI 启动标志参考](claude-cli-startup-flags.md) 获取完整列表。

---

## 来源

- [付费 Claude 计划的额外使用 — Claude 帮助中心](https://support.claude.com/en/articles/12429409-extra-usage-for-paid-claude-plans)
- [使用 Claude Code 与你的 Pro 或 Max 计划 — Claude 帮助中心](https://support.claude.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan)
- [/extra-usage 斜杠命令无文档 — GitHub 问题 #12396](https://github.com/anthropics/claude-code/issues/12396)
- [Claude Code CLI 参考](https://code.claude.com/docs/en/cli-reference)
