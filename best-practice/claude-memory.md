# Claude 内存

通过 CLAUDE.md 文件实现持久上下文 — 如何编写以及如何在 monorepo 中加载。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1. 编写优质的 CLAUDE.md

结构良好的 CLAUDE.md 是改善 Claude Code 项目输出的最有效的方式。Humanlayer 有一份优秀的指南，涵盖应包含的内容、如何构建以及常见陷阱。

- [Humanlayer - 编写优质的 Claude.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)

---

## 2. 大型 Monorepo 中的 CLAUDE.md

在 monorepo 中使用 Claude Code 时，理解 CLAUDE.md 文件如何加载到上下文中对于有效地组织项目指令至关重要。

<p align="center">
  <a href="https://x.com/bcherny/status/2016339448863355206"><img src="assets/claude-memory/claude-memory-monorepo.jpg" alt="CLAUDE.md 在 monorepo 中的加载" width="600"></a>
</p>

### 两种加载机制

Claude Code 使用两种不同的机制来加载 CLAUDE.md 文件：

#### 祖先加载（向上遍历目录树）

当启动 Claude Code 时，它从当前工作目录向文件系统根目录向上遍历，加载找到的每个 CLAUDE.md。这些文件在启动时**立即加载**。

#### 后代加载（向下遍历目录树）

位于当前工作目录下子目录中的 CLAUDE.md 文件**在启动时不被加载**。它们仅在 Claude 在会话期间读取这些子目录中的文件时才被包括。这称为**惰性加载**。

### 示例 Monorepo 结构

考虑一个典型的 monorepo，其中有不同组件的单独目录：

```
/mymonorepo/
├── CLAUDE.md          # 根级别指令（在所有组件中共享）
├── frontend/
│   └── CLAUDE.md      # 前端特定指令
├── backend/
│   └── CLAUDE.md      # 后端特定指令
└── api/
    └── CLAUDE.md      # API 特定指令
```

### 场景 1：从根目录运行 Claude Code

从 `/mymonorepo/` 运行 Claude Code 时：

```bash
cd /mymonorepo
claude
```

| 文件 | 启动时加载？ | 原因 |
|------|-------------------|--------|
| `/mymonorepo/CLAUDE.md` | 是 | 这是您的当前工作目录 |
| `/mymonorepo/frontend/CLAUDE.md` | 否 | 仅在您读/编辑 `frontend/` 中的文件时加载 |
| `/mymonorepo/backend/CLAUDE.md` | 否 | 仅在您读/编辑 `backend/` 中的文件时加载 |
| `/mymonorepo/api/CLAUDE.md` | 否 | 仅在您读/编辑 `api/` 中的文件时加载 |

### 场景 2：从组件目录运行 Claude Code

从 `/mymonorepo/frontend/` 运行 Claude Code 时：

```bash
cd /mymonorepo/frontend
claude
```

| 文件 | 启动时加载？ | 原因 |
|------|-------------------|--------|
| `/mymonorepo/CLAUDE.md` | 是 | 这是一个祖先目录 |
| `/mymonorepo/frontend/CLAUDE.md` | 是 | 这是您的当前工作目录 |
| `/mymonorepo/backend/CLAUDE.md` | 否 | 目录树的不同分支 |
| `/mymonorepo/api/CLAUDE.md` | 否 | 目录树的不同分支 |

### 关键要点

1. **祖先总是在启动时加载** — Claude 沿着目录树向上遍历，加载找到的所有 CLAUDE.md 文件。这确保您始终可以访问根级别的、整个仓库范围的指令。

2. **后代惰性加载** — 子目录 CLAUDE.md 文件仅在您与那些子目录中的文件交互时才加载。这可以防止不相关的上下文膨胀会话。

3. **同级永不加载** — 如果您在 `frontend/` 中工作，您不会获得 `backend/CLAUDE.md` 或 `api/CLAUDE.md` 的加载。

4. **全局 CLAUDE.md** — 您还可以在主文件夹中的 `~/.claude/CLAUDE.md` 放置 CLAUDE.md，这适用于所有 Claude Code 会话，无论项目如何。

### 为什么这种设计适用于 Monorepo

- **共享指令向下传播** — 根级别 CLAUDE.md 包含在任何地方都适用的整个仓库约定、编码标准和常见模式。

- **组件特定指令保持隔离** — 前端开发者不需要后端特定指令搞乱他们的上下文，反之亦然。

- **上下文优化** — 通过惰性加载后代 CLAUDE.md 文件，Claude Code 避免在启动时加载可能数百 KB 的无关指令。

### 最佳实践

1. **在根 CLAUDE.md 中放置共享约定** — 编码标准、提交消息格式、PR 模板以及其他适用于整个仓库的指南。

2. **在组件 CLAUDE.md 中放置组件特定指令** — 框架特定模式、组件架构、该组件独有的测试约定。

3. **使用 CLAUDE.local.md 记录个人偏好** — 将其添加到 `.gitignore` 以获取不应与团队共享的指令。

---

## 来源

- [Claude Code 文档 - Claude 如何查找内存](https://code.claude.com/docs/en/memory#how-claude-looks-up-memories)
- [Boris Cherny 在 X 上 - CLAUDE.md 加载说明](https://x.com/bcherny/status/2016339448863355206)
- [Humanlayer - 编写优质的 Claude.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
