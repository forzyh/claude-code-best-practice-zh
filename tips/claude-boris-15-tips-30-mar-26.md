# Claude Code 中的 15 个隐藏和未充分利用特性 — 来自 Boris Cherny

Boris Cherny（[@bcherny](https://x.com/bcherny)）Claude Code 创建者于 2026 年 3 月 30 日分享的提示总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了他最喜欢的 Claude Code 中隐藏和未充分利用的特性，专注于他最常使用的那些。

<a href="https://x.com/bcherny/status/2038454336355999749"><img src="assets/boris-30-mar-26/0.png" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ Claude Code 有一个移动应用

你知道 Claude Code 有一个移动应用吗？Boris 从 iOS 应用编写很多他的代码 — 这是一个便利的方式来在不打开笔记本电脑的情况下进行变化。

- 下载 iOS/Android 的 Claude 应用
- 导航到左侧的**代码**标签
- 你可以审查变化、批准 PR 并直接从你的手机编写代码

---

## 2/ 在移动/Web/桌面和终端之间移动会话

运行 `claude --teleport` 或 `/teleport` 在你的机器上继续云会话。或运行 `/remote-control` 从你的手机/Web 控制本地运行的会话。

- **传送**：将云会话拉下到你的本地终端
- **远程控制**：让你从任何设备控制本地会话
- Boris 在他的 `/config` 中设置了**为所有会话启用远程控制**

---

## 3/ /loop 和 /schedule — 两个最强大的特性

使用这些在设定的间隔内自动运行 Claude，时间长达一周。Boris 有一堆在本地运行的循环：

- `/loop 5m /babysit` — 自动地址代码审查、自动变基和将 PR 牧放到生产
- `/loop 30m /slack-feedback` — 每 30 分钟自动提交处理 Slack 反馈的 PR
- `/loop /post-merge-sweeper` — 提交 PR 来处理他错过的代码审查评论
- `/loop 1h /pr-pruner` — 关闭过时且不再需要的 PR

...和很多更多！

---

## 4/ 使用钩子确定性运行逻辑

使用钩子作为代理生命周期的一部分运行逻辑。例如：

- **动态加载**在每次启动 Claude 时进入上下文（`SessionStart`）
- **记录每个 bash 命令**模型运行（`PreToolUse`）
- **路由权限提示**到 WhatsApp 供你批准/拒绝（`PermissionRequest`）
- **戳 Claude**当它停止时继续（`Stop`）

---

## 5/ Cowork Dispatch

Boris 每天使用 Dispatch 来跟上 Slack 和电子邮件、管理文件和做东西在他的笔记本电脑上当他不在计算机上时。当他不编码时，他在调度。

- Dispatch 是 Claude Desktop 应用的**安全远程控制**
- 它可以使用你的 MCP、浏览器和计算机，得到你的许可
- 想象它作为一种方式来委托非编码任务给 Claude 从任何地方

---

## 6/ 为前端工作使用 Chrome 扩展

对于使用 Claude Code 最重要的提示：**给 Claude 一种验证其输出的方法。** 一旦你做到，Claude 将迭代直到结果很好。

- 想象要求某人构建一个网站，但他们不被允许使用浏览器 — 结果可能看起来不好
- 给 Claude 一个浏览器，它将编写代码并迭代直到它看起来好
- Boris 在处理 Web 代码时每次使用 Chrome 扩展 — 它往往比其他类似 MCP 更可靠地工作

---

## 7/ 使用 Claude Desktop 应用自动启动和测试 Web 服务器

沿同一静脉，Desktop 应用捆绑了 Claude **自动运行你的 Web 服务器的能力，甚至在内置浏览器中测试它。**

- 你可以在 CLI 或 VSCode 中使用 Chrome 扩展设置类似的东西
- 或仅为集成体验使用 Desktop 应用

---

## 8/ 分叉你的会话

人们经常问如何分叉现有会话。两种方式：

1. 从你的会话运行 `/branch`
2. 从 CLI，运行 `claude --resume <session-id> --fork-session`

`/branch` 创建一个分支对话 — 你现在在分支中。为了恢复原始，使用 `claude -r <original-session-id>`。

---

## 9/ 为侧查询使用 /btw

Boris 一直用这个来在代理工作时回答快速问题。`/btw` 让你提出侧问题而不中断代理的当前任务。

---

## 10/ 使用 Git Worktrees

Claude Code 附带了 git worktrees 的深度支持。Worktrees 对于在同一存储库中做很多并行工作是必需的。Boris 有**数十个 Claude 在任何时候运行**，这是他如何做到的。

- 使用 `claude -w` 在 worktree 中启动新会话
- 或在 Claude Desktop 应用中击中 **"worktree" 复选框**
- 对于非 git VCS 用户，使用 `WorktreeCreate` 钩子为 worktree 创建添加你自己的逻辑

---

更多提示... [查看原始文件获取完整内容]

---

## 来源

- [Boris Cherny (@bcherny) X — 2026 年 3 月 30 日](https://x.com/bcherny/status/2038454336355999749)
