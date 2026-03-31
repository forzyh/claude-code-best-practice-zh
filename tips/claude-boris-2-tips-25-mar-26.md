# 压缩合并和 PR 大小分布 — 来自 Boris Cherny 的提示

Boris Cherny（[@bcherny](https://x.com/bcherny)）Claude Code 创建者于 2026 年 3 月 25 日分享的见解总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1/ 单日 266 个贡献 — 总是压缩

Boris 分享了他的 GitHub 贡献图显示**3 月 24 日 266 个贡献** — 来自**141 个 PR，总是压缩**，每个 PR 中位数为**118 行**。

- 压缩合并将所有分支提交合并到目标分支上的单个提交 — 保持历史清洁和线性
- 每个 PR = 一个提交使得易于恢复整个特性并简化 `git bisect`
- 在高速度 AI 辅助工作流（141 PR/天），压缩是实用的选择 — 分支内的单个"修复 lint"、"尝试这个"提交是噪音

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-25-mar-26/1.png" alt="Boris Cherny — 266 贡献，总是压缩" width="50%" /></a>

---

## 2/ PR 大小分布 — 保持 PR 小

Boris 分享了跨那 141 个 PR 的大小分布，总计**45,032 行改变**（添加 + 删除）：

| 指标 | 行数（添加+删除） | 含义 |
|--------|---------------:|---------|
| **p50** | **118** | 中位 PR 大小 — 一半的 PR 是 118 行或更少 |
| p90 | 498 | 90% PR 在 500 行以下 |
| **p99** | **2,978** | 仅 ~1 PR 超过 ~3K 行 |
| min | 2 | 最小 PR — 快速 2 行修复 |
| max | 10,459 | 最大单个 PR — 可能是迁移或生成代码 |

- **中位数 118 行**意味着大多数 PR 在 141 PR/天时是聚焦和可审查的
- 分布严重右倾 — 偶尔的大 PR 不可避免（批量重命名、迁移），但规范是紧的
- 小 PR 减少合并冲突风险，更容易审查，并与压缩合并完美配对用于清洁还原

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-25-mar-26/2.png" alt="Boris Cherny — PR 大小分布表" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) X — 2026 年 3 月 25 日](https://x.com/bcherny)
