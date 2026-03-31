# Claude Code: 代理内存前置

用于子代理的持久内存 — 使代理能够跨会话学习、记住和积累知识。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

在 **Claude Code v2.1.33**（2026 年 2 月）中引入，`memory` 前置字段为每个子代理提供了自己的持久 markdown 知识库。在此之前，每次代理调用都从头开始。

```yaml
---
name: code-reviewer
description: 审查代码质量和最佳实践
tools: Read, Write, Edit, Bash
model: sonnet
memory: user
---

你是代码审查员。当你审查代码时，用
模式、惯例和你发现的递归问题更新你的代理内存。
```

---

## 内存范围

| 范围 | 存储位置 | 版本控制 | 共享 | 最适合 |
|-------|-----------------|-------------------|--------|----------|
| `user` | `~/.claude/agent-memory/<agent-name>/` | 否 | 否 | 跨项目知识（推荐默认） |
| `project` | `.claude/agent-memory/<agent-name>/` | 是 | 是 | 团队应共享的项目特定知识 |
| `local` | `.claude/agent-memory-local/<agent-name>/` | 否（git 忽略） | 否 | 个人的项目特定知识 |

这些范围镜像设置层级（`~/.claude/settings.json` → `.claude/settings.json` → `.claude/settings.local.json`）。

---

## 工作原理

1. **启动时**：前 200 行 `MEMORY.md` 被注入代理的系统提示
2. **工具访问**：`Read`、`Write`、`Edit` 自动启用，所以代理可以管理其内存
3. **执行期间**：代理自由读取/写入其内存目录
4. **策划**：如果 `MEMORY.md` 超过 200 行，代理将细节移到主题特定文件

```
~/.claude/agent-memory/code-reviewer/     # user 范围示例
├── MEMORY.md                              # 主文件（前 200 行加载）
├── react-patterns.md                      # 主题特定文件
└── security-checklist.md                  # 主题特定文件
```

---

## 代理内存 vs 其他内存系统

| 系统 | 谁写 | 谁读 | 范围 |
|--------|-----------|-----------|-------|
| **CLAUDE.md** | 你（手动） | 主 Claude + 所有代理 | 项目 |
| **自动内存** | 主 Claude（自动） | 仅主 Claude | 每项目每用户 |
| **`/memory` 命令** | 你（通过编辑器） | 仅主 Claude | 每项目每用户 |
| **代理内存** | 代理本身 | 仅该特定代理 | 可配置（user/project/local） |

这些系统是**互补的** — 代理同时读取 CLAUDE.md（项目上下文）和自己的内存（代理特定知识）。

---

## 实际示例

```yaml
---
name: api-developer
description: 实现遵循团队惯例的 API 端点
tools: Read, Write, Edit, Bash
model: sonnet
memory: project
skills:
  - api-conventions
  - error-handling-patterns
---

实现 API 端点。遵循预加载技能中的惯例。
当你工作时，将架构决策和模式保存到你的内存。
```

这结合了**技能**（启动时的静态知识）和**内存**（随时间构建的动态知识）。

---

## 提示

- **提示内存使用** — 包含明确说明：`"在启动前，查看你的内存。完成后，用你学到的东西更新你的内存。"`
- **调用代理时请求内存检查**：`"审查此 PR，并检查你的内存中你以前看到的模式。"`
- **选择正确的范围** — `user` 用于跨项目，`project` 用于团队共享，`local` 用于个人

---

## 来源

- [创建自定义子代理 — Claude Code 文档](https://code.claude.com/docs/en/sub-agents)
- [管理 Claude 的内存 — Claude Code 文档](https://code.claude.com/docs/en/memory)
- [Claude Code v2.1.33 发布说明](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
