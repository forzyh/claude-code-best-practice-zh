# 命令最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2028%2C%202026%206%3A05%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-commands-implementation.md)

Claude Code 命令 — 前言字段和官方内置斜杠命令。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 前言字段（13个）

| 字段 | 类型 | 必需 | 说明 |
|-------|------|----------|-------------|
| `name` | string | 否 | 显示名称和 `/斜杠命令` 标识符。如果省略，默认为目录名 |
| `description` | string | 推荐 | 命令的功能。显示在自动完成中，由 Claude 用于自动发现 |
| `argument-hint` | string | 否 | 自动完成时显示的提示（例如 `[issue-number]`、`[filename]`） |
| `disable-model-invocation` | boolean | 否 | 设置 `true` 以防止 Claude 自动调用此命令 |
| `user-invocable` | boolean | 否 | 设置 `false` 以从 `/` 菜单中隐藏 — 命令仅成为背景知识 |
| `paths` | string/list | 否 | Glob 模式，限制何时激活此技能。接受逗号分隔的字符串或 YAML 列表。设置后，Claude 仅在处理匹配模式的文件时自动加载技能 |
| `allowed-tools` | string | 否 | 此命令活跃时无需权限提示即可使用的工具 |
| `model` | string | 否 | 此命令运行时使用的模型（例如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | 否 | 调用时覆盖模型效能级别（`low`、`medium`、`high`、`max`） |
| `context` | string | 否 | 设置为 `fork` 以在隔离的子代理上下文中运行命令 |
| `agent` | string | 否 | 设置 `context: fork` 时的子代理类型（默认：`general-purpose`） |
| `shell` | string | 否 | `` !`command` `` 块的 Shell — 接受 `bash`（默认）或 `powershell`。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |
| `hooks` | object | 否 | 此命令范围内的生命周期钩子 |

---

## ![Official](../!/tags/official.svg) **(64)**

| # | 命令 | 标签 | 说明 |
|---|---------|-----|-------------|
| 1 | `/login` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 通过 OAuth 验证 Claude Code |
| 2 | `/logout` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 从 Claude Code 登出 |
| 3 | `/upgrade` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 打开升级页面以切换到更高计划层级 |
| 4 | `/color [color\|default]` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 为当前会话设置提示栏颜色 |
| 5 | `/config` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 打开设置界面以调整主题、模型、输出风格和其他偏好。别名：`/settings` |
| 6 | `/keybindings` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 按上下文自定义键盘快捷方式并创建弦序列 |
| 7 | `/permissions` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 查看或更新工具权限。别名：`/allowed-tools` |
| 8 | `/privacy-settings` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 管理隐私和遥测偏好 |
| 9 | `/sandbox` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 配置沙箱及依赖项状态 |
| 10 | `/statusline` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 设置 Claude Code 的状态行 UI |
| 11 | `/stickers` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 订购 Claude Code 贴纸 |
| 12 | `/terminal-setup` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 在 IDE 终端中启用 shift+enter 换行 |
| 13 | `/theme` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 更改配色主题 |
| 14 | `/vim` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 启用 vim 风格编辑模式 |
| 15 | `/voice` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 切换按住即说语音输入。需要 Claude.ai 账户 |
| 16 | `/context` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 将当前上下文使用可视化为带有令牌计数的彩色网格 |
| 17 | `/cost` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 显示当前会话的令牌使用统计 |
| 18 | `/extra-usage` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 为订阅计划配置按使用量付费溢出计费 |
| 19 | `/insights` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 生成报告分析您的 Claude Code 会话，包括项目区域、交互模式和摩擦点 |
| 20 | `/stats` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 可视化每日使用情况、会话历史、连续使用和模型偏好 |
| 21 | `/status` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 打开显示版本、模型、账户和连接性的设置界面（状态标签） |
| 22 | `/usage` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 显示计划使用限制和速率限制状态（仅限订阅计划） |
| 23 | `/doctor` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 检查 Claude Code 安装的健康状况 |
| 24 | `/feedback [report]` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 提交关于 Claude Code 的反馈。别名：`/bug` |
| 25 | `/help` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 显示斜杠命令帮助 |
| 26 | `/release-notes` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 显示最近的 Claude Code 发行说明 |
| 27 | `/tasks` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 列出和管理后台任务 |
| 28 | `/copy [N]` | ![Export](https://img.shields.io/badge/Export-7F8C8D?style=flat) | 将最后（或第 N 个最近的）助手响应复制到剪贴板。显示代码块的交互式选择器 |
| 29 | `/export [filename]` | ![Export](https://img.shields.io/badge/Export-7F8C8D?style=flat) | 将当前对话导出为文件或剪贴板 |
| 30 | `/agents` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理自定义子代理 — 查看、创建、编辑、删除 |
| 31 | `/chrome` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 Chrome 中的 Claude 浏览器集成 |
| 32 | `/hooks` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理工具事件的钩子配置 |
| 33 | `/ide` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 连接到 IDE 集成 |
| 34 | `/mcp` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 MCP 服务器连接 |
| 35 | `/plugin` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 Claude Code 插件 |
| 36 | `/reload-plugins` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 重新加载已安装的插件而不需要重启 |
| 37 | `/skills` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 列出可用技能 |
| 38 | `/memory` | ![Memory](https://img.shields.io/badge/Memory-3498DB?style=flat) | 编辑 CLAUDE.md 内存文件、启用或禁用自动内存、查看自动内存条目 |
| 39 | `/effort [low\|medium\|high\|max\|auto]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 设置模型效能级别 |
| 40 | `/fast [on\|off]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 切换快速模式 — 相同的 Opus 4.6 模型但输出更快 |
| 41 | `/model [model]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 选择或更改 AI 模型 |
| 42 | `/passes` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 与朋友分享免费一周的 Claude Code。仅在您的账户符合条件时可见 |
| 43 | `/plan [description]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 直接从提示进入计划模式。可传递可选描述以立即启动该任务 |
| 44 | `/add-dir <path>` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 向当前会话添加新的工作目录 |
| 45 | `/diff` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 打开交互式 diff 查看器，显示未提交的变更和每次转的 diff |
| 46 | `/init` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 用 CLAUDE.md 指南初始化新项目 |
| 47 | `/pr-comments [PR]` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 获取并显示来自 GitHub 拉取请求的评论。自动检测当前分支的 PR，或传递 PR URL 或编号 |
| 48 | `/review` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 已弃用 — 改为安装 `code-review` 插件 |
| 49 | `/security-review` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 对当前变更运行集中的安全审查 |
| 50 | `/desktop` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 在 Claude Code 桌面应用中继续当前会话。仅限 macOS 和 Windows。别名：`/app` |
| 51 | `/install-github-app` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 为 PR 链接工作流安装 GitHub 应用 |
| 52 | `/install-slack-app` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 为通知和分享安装 Slack 应用 |
| 53 | `/mobile` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 显示下载 Claude 移动应用的二维码。别名：`/ios`、`/android` |
| 54 | `/remote-control` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 使此会话可从 claude.ai 进行远程控制。别名：`/rc` |
| 55 | `/remote-env` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 检查或复制远程控制环境设置 |
| 56 | `/schedule [description]` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 创建、更新、列出或运行云计划任务。Claude 以对话方式引导您完成设置 |
| 57 | `/branch [name]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 在此点创建当前对话的分支。别名：`/fork` |
| 58 | `/btw <question>` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 提出快速侧面问题而不添加到对话中 |
| 59 | `/clear` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 清空对话历史并释放上下文。别名：`/reset`、`/new` |
| 60 | `/compact [instructions]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 使用可选焦点指令压缩对话 |
| 61 | `/exit` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 退出 CLI。别名：`/quit` |
| 62 | `/rename [name]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 重命名当前会话。不带名称时，从对话历史自动生成 |
| 63 | `/resume [session]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 按 ID 或名称恢复之前的对话，或打开会话选择器。别名：`/continue` |
| 64 | `/rewind` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 倒回对话和/或代码到之前的点，或从选定的消息总结。别名：`/checkpoint` |

捆绑的技能（如 `/debug`）也可以显示在斜杠命令菜单中，但它们不是内置命令。

---

## 来源

- [Claude Code 斜杠命令](https://code.claude.com/docs/en/slash-commands)
- [Claude Code 交互模式](https://code.claude.com/docs/en/interactive-mode)
- [Claude Code 更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
