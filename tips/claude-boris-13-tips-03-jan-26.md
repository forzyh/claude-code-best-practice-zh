# 我如何使用 Claude Code — Boris Cherny 的 13 个提示

Boris Cherny（[@bcherny](https://x.com/bcherny)）Claude Code 创建者于 2026 年 1 月 3 日分享的设置提示总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了他个人的 Claude Code 设置，注意到它"令人惊讶地普通" — Claude Code 开箱即用工作很好，所以他没有自定义太多。没有一种正确的方法来使用它：团队有意构建它，所以你可以随意使用、自定义和破解它。Claude Code 团队上的每个人都非常不同地使用它。

<a href="https://x.com/bcherny/status/2007179832300581177"><img src="assets/boris-3-jan-26/0.png" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ 并行运行 5 个 Claude

在你的终端中并行运行 5 个 Claude。编号你的标签 1-5，使用系统通知了解何时 Claude 需要输入。

查看：[终端设置文档](https://code.claude.com/docs/en/terminal)

<a href="https://x.com/bcherny/status/2007179833990885678"><img src="assets/boris-3-jan-26/1.png" alt="并行运行 5 个 Claude" width="50%" /></a>

---

## 2/ 使用 claude.ai/code 获得更多并行性

在 claude.ai/code 上并行运行 5-10 个 Claude 与你的本地 Claude。使用 `claude.ai/code` 将本地会话切换到 Web 会话，在 Chrome 中手动启动会话，并来回传送。

<a href="https://x.com/bcherny/status/2007179836704600237"><img src="assets/boris-3-jan-26/2.png" alt="claude.ai/code 并行性" width="50%" /></a>

---

## 3/ 为一切使用 Opus 和 Thinking

为一切使用 Opus 4.5 with thinking。它是 Boris 使用过的最好的编码模型 — 尽管它比 Sonnet 更大更慢，由于你需要它少得多，它在工具使用中更好，它几乎总是比最后使用更小的模型更快。

<a href="https://x.com/bcherny/status/2007179838864666847"><img src="assets/boris-3-jan-26/3.png" alt="Opus with thinking" width="50%" /></a>

---

## 4/ 与你的团队分享单个 CLAUDE.md

为存储库分享单个 `CLAUDE.md`。检入 git，并让整个团队每周贡献多次。任何时候 Claude 做错什么，将其添加到 `CLAUDE.md`，这样 Claude 下次就知道不要做。

<a href="https://x.com/bcherny/status/2007179840848597422"><img src="assets/boris-3-jan-26/4.png" alt="共享 CLAUDE.md" width="50%" /></a>

---

## 5/ 在 PR 上标记 @claude 更新 CLAUDE.md

在代码审查期间，在你同事的 PR 上标记 `@claude` 以在 PR 中添加到 `CLAUDE.md`。使用 Claude Code GitHub 操作（[install-@hub-action](https://github.com/apps/claude)）— 这是 Boris 的复合工程版本。

<a href="https://x.com/bcherny/status/2007179842928947333"><img src="assets/boris-3-jan-26/5.png" alt="在 PR 上标记 @claude" width="50%" /></a>

---

## 6/ 在计划模式下启动大多数会话

在计划模式下启动大多数会话（shift+tab 两次）。如果目标是编写拉取请求，使用计划模式并与 Claude 来回，直到你喜欢它的计划。从那里，切换到自动接受编辑模式，Claude 通常可以一次性做到。一个好计划非常重要。

<a href="https://x.com/bcherny/status/2007179845336527000"><img src="assets/boris-3-jan-26/6.png" alt="计划模式" width="50%" /></a>

---

## 7/ 为内循环工作流使用斜杠命令

为每个你每天做很多次的"内循环"工作流使用斜杠命令。这省去你重复提示，使 Claude 也能够使用这些工作流。命令被检入 git 并在 `.claude/commands/` 中生活。

示例：`/commit-push-pr` — 提交、推送和打开 PR。

<a href="https://x.com/bcherny/status/2007179847949500714"><img src="assets/boris-3-jan-26/7.png" alt="斜杠命令" width="50%" /></a>

---

## 8/ 使用子代理自动化常见工作流

定期使用几个子代理：`code-simplifier` 在 Claude 完成工作后简化代码，`verify-app` 有详细说明用于端到端测试 Claude Code，等等。想象子代理自动化最常见的工作流 — 类似于斜杠命令。

子代理在 `.claude/agents/` 中生活。

<a href="https://x.com/bcherny/status/2007179850139000872"><img src="assets/boris-3-jan-26/8.png" alt="子代理" width="50%" /></a>

---

## 9/ 使用 PostToolUse 钩子自动格式化代码

使用 `PostToolUse` 钩子格式化 Claude 的代码。Claude 通常开箱即用生成格式良好的代码，钩子处理最后 10% 以避免稍后在 CI 中格式化错误。

```json
"PostToolUse": [
  {
    "matcher": "Write|Edit",
    "hooks": [
      {
        "type": "command",
        "command": "bun run format || true"
      }
    ]
  }
]
```

<a href="https://x.com/bcherny/status/2007179852047335529"><img src="assets/boris-3-jan-26/9.png" alt="PostToolUse 钩子格式化" width="50%" /></a>

---

## 10/ 预允许权限而不是 --dangerously-skip-permissions

不使用 `--dangerously-skip-permissions`。相反，使用 `/permissions` 预允许你知道在你的环境中安全的常见 bash 命令，以避免不必要的权限提示。大多数这些被检入 `.claude/settings.json` 并与团队共享。

<a href="https://x.com/bcherny/status/2007179854077407667"><img src="assets/boris-3-jan-26/10.png" alt="预允许权限" width="50%" /></a>

---

## 11/ 通过 MCP 让 Claude 使用你的所有工具

Claude Code 使用你的所有工具。它经常搜索和发布到 Slack（通过 MCP 服务器），运行 BigQuery 查询来回答分析问题（使用 `bq` CLI），从 Sentry 抓取错误日志，等等。Slack MCP 配置被检入 `.mcp.json` 并与团队共享。

<a href="https://x.com/bcherny/status/2007179856266789204"><img src="assets/boris-3-jan-26/11.png" alt="MCP 工具" width="50%" /></a>

---

## 12/ 用后台代理验证长运行任务

对于非常长运行的任务，要么 (a) 提示 Claude 在完成时使用后台代理验证其工作，(b) 使用代理停止钩子更确定性地做到，或 (c) 使用 ralph-wiggum 插件（最初由 @GeoffreyHuntley 梦想）。

<a href="https://x.com/bcherny/status/2007179858435281082"><img src="assets/boris-3-jan-26/12.png" alt="长运行任务验证" width="50%" /></a>

---

## 13/ 给 Claude 一种验证其工作的方法

可能从 Claude Code 获得很好结果最重要的事情 — 给 Claude 一种验证其工作的方法。如果 Claude 拥有该反馈循环，它将使最终结果的质量增加 2-3 倍。

Claude 测试 Boris 降落的每一个变化。

<a href="https://x.com/bcherny/status/2007179861115511237"><img src="assets/boris-3-jan-26/13.png" alt="给 Claude 一种验证" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) X — 2026 年 1 月 3 日](https://x.com/bcherny/status/2007179832300581177)
