# 代码审查和测试时间计算 — 来自 Boris Cherny 的提示

Boris Cherny（[@bcherny](https://x.com/bcherny)）Claude Code 创建者于 2026 年 3 月 10 日分享的见解总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1/ 介绍代码审查

Claude Code 中的新功能：**代码审查**。代理团队在每个 PR 上运行深度审查。

- 首先为 Anthropic 自己的团队构建 — 每个工程师的代码输出**今年上升 200%**，审查是瓶颈
- Boris 已使用它几周并发现它捕获许多他本来不会注意到的真实错误
- 当 PR 打开时，Claude 调派代理团队寻找错误

<a href="https://x.com/bcherny/status/2031089411820228645"><img src="assets/boris-10-mar-26/0.png" alt="Boris Cherny 宣布代码审查" width="50%" /></a>

---

## 2/ 测试时间计算和多个上下文窗口

粗略地，你对编码问题投入的令牌越多，结果越好。Boris 称这为**测试时间计算**。

- 使用**单独的上下文窗口**使结果更好 — 这是让子代理工作的原因，以及为什么一个代理可导致错误而另一个（使用完全相同的模型）可找到它们
- 类似于工程团队：如果 Boris 导致一个错误，他的同事审查代码可能比他更可靠地找到它
- 在极限中，代理可能会编写完美的无错代码 — 直到那时，**多个不相关的上下文窗口**往往是一个好方法

<a href="https://x.com/bcherny/status/2031151689219321886"><img src="assets/boris-10-mar-26/1.png" alt="Boris Cherny 在测试时间计算上" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) X — 2026 年 3 月 10 日](https://x.com/bcherny)
