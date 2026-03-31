# 使用 Claude Code 的 10 个提示 — 来自 Claude Code 团队

Boris Cherny（[@bcherny](https://x.com/bcherny)）Claude Code 创建者于 2026 年 2 月 1 日分享的团队提示总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了 Claude Code 团队直接来源的提示。团队使用 Claude 的方式与 Boris 个人使用它的方式不同。记住：没有一种正确的方法来使用 Claude Code — 每个人的设置都不同。你应该进行实验以查看什么对你有效！

<a href="https://x.com/bcherny/status/2017742741636321619"><img src="assets/boris-1-feb-26/0.png" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ 并行做更多事

一次启动 3-5 个 git worktrees，每个都在自己的 Claude 会话中运行。这是单个最大的生产力解锁，也是团队的首要提示。个人上，Boris 使用多个 git 检出，但大多数 Claude Code 团队更喜欢 worktrees — 这是 `@amorisscode` 为 Claude Desktop 应用程序构建原生支持的原因！

一些人也为他们的 worktrees 命名并设置 shell 别名（`2a`、`2b`、`2c`），以便他们可以在一个按键中在它们之间跳跃。其他人有一个专门的"分析" worktree，仅用于读取日志和运行 BigQuery。

查看：[Worktrees 文档](https://code.claude.com/docs/en/common...)

<a href="https://x.com/bcherny/status/2017742743125299476"><img src="assets/boris-1-feb-26/1.png" alt="并行做更多" width="50%" /></a>

---

## 2/ 在计划模式下启动每个复杂任务

将你的精力投入到计划中，以便 Claude 可以一次性实现。

一个人有一个 Claude 编写计划，然后他们启动第二个 Claude 作为工作人员工程师审查它。

另一个说问题的时刻出现，他们切换回计划模式并重新计划。不要继续推动。他们也明确告诉 Claude 为验证步骤进入计划模式，不仅仅是构建。

<a href="https://x.com/bcherny/status/2017742745365057733"><img src="assets/boris-1-feb-26/2.png" alt="在计划模式下启动每个复杂任务" width="50%" /></a>

---

## 3/ 投入你的 CLAUDE.md

每次更正后，以"更新你的 CLAUDE.md，这样你就不会再犯那个错误"结束。Claude 在为自己编写规则方面非常出色。

无情地编辑你的 `CLAUDE.md` 。继续迭代直到 Claude 的错误率可测量地下降。

一个工程师告诉 Claude 为每个任务/项目维护一个笔记目录，每个 PR 后更新。然后他们指向 `CLAUDE.md`。

<a href="https://x.com/bcherny/status/2017742747067945390"><img src="assets/boris-1-feb-26/3.png" alt="投入你的 CLAUDE.md" width="50%" /></a>

---

## 4/ 创建你自己的技能并提交到 Git

跨每个项目重用。来自团队的提示：

- 如果你每天做超过一次，把它变成一个技能或命令
- 构建一个 `/techdebt` 斜杠命令，在每个会话末尾运行以查找和消除重复代码
- 设置一个将 7 天的 Slack、GDrive、Asana 和 GitHub 同步到一个上下文转储的斜杠命令
- 构建分析工程师风格的代理，编写 dbt 模型、审查代码和测试变化在开发中

查看：[使用技能扩展 Claude — Claude Code 文档](https://code.claude.com/docs/en/skills)

<a href="https://x.com/bcherny/status/2017742748984742078"><img src="assets/boris-1-feb-26/4.png" alt="创建你自己的技能" width="50%" /></a>

---

## 5/ Claude 通常自己修复大多数 bug

这是团队如何做到的：

启用 Slack MCP，然后将 Slack bug 线程粘贴到 Claude 中，仅说"修复"。零上下文切换需要。

或，仅说"去修复失败的 CI 测试。"不要微观管理如何。

指向 Claude 的 docker 日志来排除分布式系统 — 它在这方面出乎意料地能干。

<a href="https://x.com/bcherny/status/2017742750473720121"><img src="assets/boris-1-feb-26/5.png" alt="Claude 通常自己修复大多数 bug" width="50%" /></a>

---

## 6/ 提升你的提示

a. **挑战 Claude。** 说"考虑这些变化并在我通过你的测试前不要制作 PR。"让 Claude 成为你的审查者。或，说"向我证明这有效"，并在主分支和你的特性分支之间比较行为。

b. **在一个平庸的修复后，** 说："了解你现在知道的一切，废弃这个并实现优雅的解决方案。"

c. **编写详细的规范**并减少歧义，然后再交出工作。你越具体，输出就越好。

<a href="https://x.com/bcherny/status/2017742752566632544"><img src="assets/boris-1-feb-26/6.png" alt="提升你的提示" width="50%" /></a>

---

## 7/ 终端和环境设置

团队喜欢 Ghostty！多个人喜欢其同步渲染、24 位颜色和适当的 unicode 支持。

为了更容易地进行 Claude 杂耍，使用 `/statusline` 来自定义你的状态栏以始终显示上下文使用和当前 git 分支。许多人也色彩编码并命名他们的终端标签，有时使用 tmux — 每个标签一个任务/worktree。

使用语音听写。你说话速度比输入快 3 倍，你的提示因此变得更加详细。（在 macOS 上按两次 fn）

查看：[终端设置文档](https://code.claude.com/docs/en/termin...)

<a href="https://x.com/bcherny/status/2017742753971769626"><img src="assets/boris-1-feb-26/7.png" alt="终端和环境设置" width="50%" /></a>

---

## 8/ 使用子代理

a. 追加"使用子代理"到任何请求，其中你希望 Claude 对问题投入更多计算。

b. 将个别任务卸载到子代理，以保持主代理的上下文窗口清洁专注。

c. 通过钩子将权限请求路由到 Opus 4.5 — 让它扫描攻击并自动批准安全的。查看：[钩子文档](https://code.claude.com/docs/en/hooks#...)

<a href="https://x.com/bcherny/status/2017742755737555434"><img src="assets/boris-1-feb-26/8.png" alt="使用子代理" width="50%" /></a>

---

## 9/ 使用 Claude 用于数据和分析

要求 Claude Code 使用"bq"CLI 即时拉取和分析指标。团队有一个 BigQuery 技能检入代码库，每个人都用它来直接在 Claude Code 中进行分析查询。个人上，Boris 在 6 个多月没有写过一行 SQL。

这适用于任何具有 CLI、MCP 或 API 的数据库。

<a href="https://x.com/bcherny/status/2017742757666902374"><img src="assets/boris-1-feb-26/9.png" alt="使用 Claude 用于数据和分析" width="50%" /></a>

---

## 10/ 使用 Claude 学习

来自团队使用 Claude Code 学习的几个提示：

a. 在 `/config` 中启用"解释"或"学习"输出样式，使 Claude 解释其变化背后的"为什么"。

b. 让 Claude 生成一个视觉 HTML 演示来解释陌生代码。它制作出令人惊讶地好的幻灯片！

c. 要求 Claude 绘制新协议和代码库的 ASCII 图，以帮助你理解它们。

d. 构建一个间隔重复学习技能：你解释你的理解，Claude 提问以填补空白，存储结果。

<a href="https://x.com/bcherny/status/2017742759218794768"><img src="assets/boris-1-feb-26/10.png" alt="使用 Claude 学习" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) X — 2026 年 2 月 1 日](https://x.com/bcherny/status/2017742741636321619)
