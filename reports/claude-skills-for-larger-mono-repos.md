# 理解大型单体库中的 Claude 技能发现

在单体库中使用 Claude Code 时，理解技能如何被发现和加载到上下文中对于有效组织项目特定功能至关重要。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 与 CLAUDE.md 的重要区别

**技能与 CLAUDE.md 文件没有相同的加载行为。** 虽然 CLAUDE.md 文件向上遍历目录树（祖先加载），但技能使用不同的发现机制，专注于项目内的嵌套目录。

## 技能如何被发现

### 1. 标准技能位置

技能从这些基于范围的固定位置加载：

| 位置 | 路径 | 应用于 |
|----------|------|------------|
| 企业 | 托管设置 | 组织中的所有用户 |
| 个人 | `~/.claude/skills/<skill-name>/SKILL.md` | 你的所有项目 |
| 项目 | `.claude/skills/<skill-name>/SKILL.md` | 仅此项目 |
| 插件 | `<plugin>/skills/<skill-name>/SKILL.md` | 启用插件的位置 |

### 2. 从嵌套目录自动发现

当你在子目录中使用文件时，Claude Code 自动从嵌套的 `.claude/skills/` 目录发现技能。例如，如果你在 `packages/frontend/` 中编辑文件，Claude Code 也会在 `packages/frontend/.claude/skills/` 中查看。

这支持单体库设置，其中包有自己的技能。

## 示例单体库结构

考虑一个具有单独包的典型单体库：

```
/mymonorepo/
├── .claude/
│   └── skills/
│       └── shared-conventions/SKILL.md    # 项目级技能
├── packages/
│   ├── frontend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── react-patterns/SKILL.md  # 前端特定技能
│   │   └── src/
│   │       └── App.tsx
│   ├── backend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── api-design/SKILL.md      # 后端特定技能
│   │   └── src/
│   └── shared/
│       ├── .claude/
│       │   └── skills/
│       │       └── utils-patterns/SKILL.md  # 共享实用程序技能
│       └── src/
```

## 场景 1：刚在根目录启动 Claude（还未编辑任何文件）

当你从 `/mymonorepo/` 运行 Claude Code 且还未编辑任何文件时：

```bash
cd /mymonorepo
claude
# 刚启动 - 还未编辑任何文件
```

| 技能 | 在上下文中？ | 原因 |
|-------|-------------|--------|
| `shared-conventions` | **是** | 根 `.claude/skills/` 中的项目级技能 |
| `react-patterns` | **否** | 未发现 - 还未在 `packages/frontend/` 中处理文件 |
| `api-design` | **否** | 未发现 - 还未在 `packages/backend/` 中处理文件 |
| `utils-patterns` | **否** | 未发现 - 还未在 `packages/shared/` 中处理文件 |

## 场景 2：编辑包中的文件后

在要求 Claude 编辑 `packages/frontend/src/App.tsx` 后：

| 技能 | 在上下文中？ | 原因 |
|-------|-------------|--------|
| `shared-conventions` | **是** | 根 `.claude/skills/` 中的项目级技能 |
| `react-patterns` | **是** | 在编辑 `packages/frontend/` 中的文件时发现 |
| `api-design` | **否** | 仍未发现 - 还未在 `packages/backend/` 中处理文件 |
| `utils-patterns` | **否** | 仍未发现 - 还未在 `packages/shared/` 中处理文件 |

**关键洞察**：嵌套技能**按需发现**当你在这些目录中处理文件时。它们在会话启动时不会预加载。

## 关键行为：描述 vs 完整内容

技能描述被加载到上下文，但**完整技能内容仅在调用时加载**。这是重要的优化：

- **描述**：始终在上下文中（在字符预算内）
- **完整内容**：在调用技能时按需加载

> 注意：具有预加载技能的子代理工作方式不同 - 完整技能内容在启动时被注入。

## 优先级顺序（当技能共享名称时）

当技能在级别间共享相同名称时，较高优先级位置获胜：

| 优先级 | 位置 | 范围 |
|----------|----------|-------|
| 1（最高） | 企业 | 组织范围 |
| 2 | 个人（`~/.claude/skills/`） | 你的所有项目 |
| 3（最低） | 项目（`.claude/skills/`） | 仅此项目 |

插件技能使用 `plugin-name:skill-name` 命名空间，因此不会与其他级别冲突。

## 为什么这个设计适用于单体库

- **包特定技能保持隔离** - 在 `packages/frontend/` 工作的前端开发者获得前端特定技能，无需后端技能杂乱上下文。

- **自动发现减少配置** - 无需显式注册包级技能；在这些目录中处理文件时它们被发现。

- **上下文被优化** - 仅技能描述初始加载，嵌套技能按需发现。

- **团队可维护自己的技能** - 每个包团队可定义其域特定的技能，无需与其他团队协调。

## 字符预算考虑

技能描述被加载到上下文，上限为字符预算（默认 15,000 字符）。在具有许多包和技能的大型单体库中，你可能达到此限制。

- 运行 `/context` 检查排除技能的警告
- 设置 `SLASH_COMMAND_TOOL_CHAR_BUDGET` 环境变量增加限制

## 最佳实践

1. **将共享工作流放在根 `.claude/skills/`** - 存储库范围的惯例、提交工作流和共享模式。

2. **将包特定技能放在包 `.claude/skills/`** - 框架特定模式、组件惯例、该包特有的测试实用程序。

3. **对危险技能使用 `disable-model-invocation: true`** - 部署或破坏性技能应需要显式用户调用。

4. **保持技能描述简洁** - 描述始终在上下文中（上限为字符预算），因此冗长的描述浪费上下文空间。

5. **在技能名称中使用命名空间** - 考虑以包名称为前缀（例如 `frontend-review`、`backend-deploy`），以避免混淆。

## 比较：技能 vs CLAUDE.md 加载

| 行为 | CLAUDE.md | 技能 |
|----------|-----------|--------|
| 祖先加载（向上目录树） | 是 | 否 |
| 嵌套/后代发现（向下目录树） | 是（惰性） | 是（自动发现） |
| 全局位置 | `~/.claude/CLAUDE.md` | `~/.claude/skills/` |
| 项目位置 | `.claude/` 或存储库根 | `.claude/skills/` |
| 内容加载 | 完整内容 | 仅描述（调用时完整） |

---

## 来源

- [Claude Code 文档 - 用技能扩展 Claude](https://code.claude.com/docs/en/skills)
- [Claude Code 文档 - 从嵌套目录自动发现](https://code.claude.com/docs/en/skills#automatic-discovery-from-nested-directories)
