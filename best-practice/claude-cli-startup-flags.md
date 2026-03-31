# CLI 启动标志最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2002%2C%202026-white?style=flat&labelColor=555)

Claude Code 启动标志、顶级子命令和启动环境变量的参考指南。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 目录

1. [会话管理](#会话管理)
2. [模型和配置](#模型和配置)
3. [权限和安全](#权限和安全)
4. [输出和格式](#输出和格式)
5. [系统提示](#系统提示)
6. [代理和子代理](#代理和子代理)
7. [MCP 和插件](#mcp-和插件)
8. [目录和工作区](#目录和工作区)
9. [预算和限制](#预算和限制)
10. [集成](#集成)
11. [初始化和维护](#初始化和维护)
12. [调试和诊断](#调试和诊断)
13. [设置覆盖](#设置覆盖)
14. [版本和帮助](#版本和帮助)
15. [子命令](#子命令)
16. [环境变量](#环境变量)

---

## 会话管理

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--continue` | `-c` | 继续当前目录中最近的会话 |
| `--resume` | `-r` | 按 ID 或名称恢复特定会话，或显示交互式选择器 |
| `--from-pr <NUMBER\|URL>` | | 恢复与特定 GitHub PR 链接的会话 |
| `--fork-session` | | 恢复时创建新会话 ID（与 `--resume` 或 `--continue` 一起使用） |
| `--session-id <UUID>` | | 使用特定会话 ID（必须是有效的 UUID） |
| `--no-session-persistence` | | 禁用会话持久化（仅限打印模式） |
| `--remote` | | 在 claude.ai 上创建新的网页会话 |
| `--teleport` | | 在本地终端中恢复网页会话 |

---

## 模型和配置

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--model <NAME>` | | 使用别名（`sonnet`、`opus`、`haiku`）或完整模型 ID 设置模型 |
| `--fallback-model <NAME>` | | 默认模型过载时的自动备用模型（仅限打印模式） |
| `--betas <LIST>` | | 包含在 API 请求中的 Beta 头（仅限 API 密钥用户） |

---

## 权限和安全

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--dangerously-skip-permissions` | | 跳过所有权限提示。请谨慎使用 |
| `--allow-dangerously-skip-permissions` | | 启用权限绕过选项但不激活它 |
| `--permission-mode <MODE>` | | 开始时使用指定的权限模式：`default`、`plan`、`acceptEdits`、`bypassPermissions` |
| `--allowedTools <TOOLS>` | | 无需提示即可执行的工具（权限规则语法） |
| `--disallowedTools <TOOLS>` | | 从模型上下文中完全删除的工具 |
| `--tools <TOOLS>` | | 限制 Claude 可以使用的内置工具（使用 `""` 禁用所有工具） |
| `--permission-prompt-tool <TOOL>` | | 指定 MCP 工具在非交互模式下处理权限提示 |

---

## 输出和格式

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--print` | `-p` | 打印响应而不进入交互模式（无头模式/SDK 模式） |
| `--output-format <FORMAT>` | | 输出格式：`text`、`json`、`stream-json` |
| `--input-format <FORMAT>` | | 输入格式：`text`、`stream-json` |
| `--json-schema <SCHEMA>` | | 获取与架构匹配的验证 JSON（仅限打印模式） |
| `--include-partial-messages` | | 包含部分流式事件（需要 `--print` 和 `--output-format=stream-json`） |
| `--verbose` | | 启用详细日志记录，包含完整的逐转输出 |

---

## 系统提示

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--system-prompt <TEXT>` | | 用自定义文本替换整个系统提示 |
| `--system-prompt-file <PATH>` | | 从文件加载系统提示，替换默认值（仅限打印模式） |
| `--append-system-prompt <TEXT>` | | 将自定义文本追加到默认系统提示 |
| `--append-system-prompt-file <PATH>` | | 将文件内容追加到默认提示（仅限打印模式） |

---

## 代理和子代理

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--agent <NAME>` | | 为当前会话指定代理 |
| `--agents <JSON>` | | 通过 JSON 动态定义自定义子代理 |
| `--teammate-mode <MODE>` | | 设置代理团队显示：`auto`、`in-process`、`tmux` |

---

## MCP 和插件

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--mcp-config <PATH\|JSON>` | | 从 JSON 文件或字符串加载 MCP 服务器 |
| `--strict-mcp-config` | | 仅使用 `--mcp-config` 中的 MCP 服务器，忽略所有其他 |
| `--plugin-dir <PATH>` | | 从目录加载该会话的插件（可重复） |

---

## 目录和工作区

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--add-dir <PATH>` | | 为 Claude 添加额外的工作目录 |
| `--worktree` | `-w` | 在隔离的 git worktree 中启动 Claude（从 HEAD 分支） |

---

## 预算和限制

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--max-budget-usd <AMOUNT>` | | API 调用前的最大美元金额限制（仅限打印模式） |
| `--max-turns <NUMBER>` | | 限制代理转数（仅限打印模式） |

---

## 集成

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--chrome` | | 启用 Chrome 浏览器集成以进行网页自动化 |
| `--no-chrome` | | 禁用此会话的 Chrome 浏览器集成 |
| `--ide` | | 启动时如果恰好有一个有效 IDE，自动连接 |

---

## 初始化和维护

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--init` | | 运行初始化钩子并启动交互模式 |
| `--init-only` | | 运行初始化钩子并退出（无交互会话） |
| `--maintenance` | | 运行维护钩子并退出 |

---

## 调试和诊断

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--debug <CATEGORIES>` | | 启用调试模式，可选类别筛选（例如 `"api,hooks"`） |

---

## 设置覆盖

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--settings <PATH\|JSON>` | | 要加载的设置 JSON 文件或 JSON 字符串的路径 |
| `--setting-sources <LIST>` | | 要加载的来源的逗号分隔列表：`user`、`project`、`local` |
| `--disable-slash-commands` | | 禁用此会话的所有技能和斜杠命令 |

---

## 版本和帮助

| 标志 | 简写 | 说明 |
|------|-------|-------------|
| `--version` | `-v` | 输出版本号 |
| `--help` | `-h` | 显示帮助信息 |

---

## 子命令

这些是作为 `claude <子命令>` 运行的顶级命令：

| 子命令 | 说明 |
|------------|-------------|
| `claude` | 启动交互式 REPL |
| `claude "query"` | 用初始提示启动 REPL |
| `claude agents` | 列出配置的代理 |
| `claude auth` | 管理 Claude Code 身份验证 |
| `claude doctor` | 从命令行运行诊断 |
| `claude install` | 安装或切换 Claude Code 原生构建 |
| `claude mcp` | 配置 MCP 服务器（`add`、`remove`、`list`、`get`、`enable`） |
| `claude plugin` | 管理 Claude Code 插件 |
| `claude remote-control` | 管理远程控制会话 |
| `claude setup-token` | 创建用于订阅使用的长期令牌 |
| `claude update` / `claude upgrade` | 更新到最新版本 |

---

## 环境变量

这些启动专用环境变量在启动 Claude Code 之前在 shell 中设置（无法通过 `settings.json` 配置）：

| 变量 | 说明 |
|----------|-------------|
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` | 启用实验性代理团队 |
| `CLAUDE_CODE_TMPDIR` | 覆盖内部文件的临时目录 |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` | 启用其他目录 CLAUDE.md 加载 |
| `DISABLE_AUTOUPDATER=1` | 禁用自动更新 |
| `CLAUDE_CODE_EFFORT_LEVEL` | 控制思考深度 — 参见[设置参考](./claude-settings.md#环境变量) |
| `USE_BUILTIN_RIPGREP=0` | 使用系统 ripgrep 而不是内置（Alpine Linux） |
| `CLAUDE_CODE_SIMPLE` | 启用简单模式（仅限 Bash + Edit 工具） |
| `CLAUDE_BASH_NO_LOGIN=1` | 跳过 BashTool 的登录 shell |

关于可通过 `settings.json` 中的 `"env"` 键配置的环境变量（包括 `MAX_THINKING_TOKENS`、`CLAUDE_CODE_SHELL`、`CLAUDE_CODE_ENABLE_TASKS`、`CLAUDE_CODE_DISABLE_BACKGROUND_TASKS`、`CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` 等），见[Claude 设置参考](./claude-settings.md#环境变量)。

---

## 来源

- [Claude Code CLI 参考](https://code.claude.com/docs/en/cli-reference)
- [Claude Code 无头模式](https://code.claude.com/docs/en/headless)
- [Claude Code 设置](https://code.claude.com/docs/en/setup)
- [Claude Code 更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code 常见工作流](https://code.claude.com/docs/en/common-workflows)
