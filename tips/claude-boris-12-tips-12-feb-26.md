# 12 种自定义 Claude Code 的方法 — Boris Cherny 的提示

Boris Cherny（[@bcherny](https://x.com/bcherny)）Claude Code 创建者于 2026 年 2 月 12 日分享的自定义提示总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris Cherny 强调可自定义性是工程师最喜欢 Claude Code 的事物之一 — 钩子、插件、LSP、MCP、技能、努力、自定义代理、状态线、输出样式等等。他分享了开发者和团队自定义他们设置的 12 个实用方式。

<a href="https://x.com/bcherny/status/2021699851499798911"><img src="assets/boris-12-feb-26/0.webp" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ 配置你的终端

设置你的终端以获得最佳 Claude Code 体验：

- **主题**：运行 `/config` 设置浅/深模式
- **通知**：为 iTerm2 启用通知，或使用自定义通知钩子
- **新行**：如果在 IDE 终端、Apple Terminal、Warp 或 Alacritty 中使用 Claude Code，运行 `/terminal-setup` 启用 shift+enter 新行（所以你不需要键入 `\`）
- **Vim 模式**：运行 `/vim`

<a href="https://x.com/bcherny/status/2021699859359883608"><img src="assets/boris-12-feb-26/1.webp" alt="配置你的终端" width="50%" /></a>

---

## 2/ 调整努力级别

运行 `/model` 选择你的首选努力级别：

- **低** — 更少令牌，更快响应
- **中** — 平衡行为
- **高** — 更多令牌，更多智能

Boris 的偏好：一切都高。

<a href="https://x.com/bcherny/status/2021699860869902424"><img src="assets/boris-12-feb-26/2.webp" alt="调整努力级别" width="50%" /></a>

---

## 3/ 安装插件、MCP 和技能

插件让你安装 LSP（对每个主要语言都可用）、MCP、技能、代理和自定义钩子。

从官方 Anthropic 插件市场安装，或为你的公司创建你自己的市场。将 `settings.json` 检入代码库以为你的团队自动添加市场。

运行 `/plugin` 开始。

<a href="https://x.com/bcherny/status/2021699862522364149"><img src="assets/boris-12-feb-26/3.webp" alt="安装插件、MCP 和技能" width="50%" /></a>

---

## 4/ 创建自定义代理

在 `.claude/agents` 中放置 `.md` 文件来创建自定义代理。每个代理可具有自定义名称、颜色、工具集、预允许和预禁止工具、权限模式和模型。

你也可以使用 `settings.json` 中的 `"agent"` 字段或 `--agent` 标志设置主对话的默认代理。

运行 `/agents` 开始。

<a href="https://x.com/bcherny/status/2021700144039903699"><img src="assets/boris-12-feb-26/4.webp" alt="创建自定义代理" width="50%" /></a>

---

## 5/ 预批准常见权限

Claude Code 使用结合提示注入检测、静态分析、沙箱和人类监督的权限系统。

开箱即用，少量安全命令被预批准。为了预批准更多，运行 `/permissions` 并添加到允许和阻止列表。将这些检入你的团队的 `settings.json`。

支持完整通配符语法 — 例如 `Bash(bun run *)` 或 `Edit(/docs/**)`。

<a href="https://x.com/bcherny/status/2021700332292911228"><img src="assets/boris-12-feb-26/5.webp" alt="预批准常见权限" width="50%" /></a>

---

## 6/ 启用沙箱

选择使用 Claude Code 的开源沙箱运行时来改进安全同时减少权限提示。

运行 `/sandbox` 启用它。沙箱在你的机器上运行，支持文件和网络隔离。

<a href="https://x.com/bcherny/status/2021700506465579443"><img src="assets/boris-12-feb-26/6.webp" alt="启用沙箱" width="50%" /></a>

---

## 7/ 添加状态线

自定义状态线显示在编辑器下方，显示模型、目录、剩余上下文、成本和你想看到的任何其他东西。

每个团队成员可拥有不同的状态线。使用 `/statusline` 根据你的 `.bashrc`/`.zshrc` 生成一个。

<a href="https://x.com/bcherny/status/2021700784019452195"><img src="assets/boris-12-feb-26/7.webp" alt="添加状态线" width="50%" /></a>

---

## 8/ 自定义你的键绑定

Claude Code 中的每个键绑定都是可自定义的。运行 `/keybindings` 重新映射任何键。设置实时重新加载，所以你可立即看到它的感觉。

<a href="https://x.com/bcherny/status/2021700883873164435"><img src="assets/boris-12-feb-26/8.webp" alt="自定义你的键绑定" width="50%" /></a>

---

## 9/ 设置钩子

钩子让你确定性地进入 Claude 的生命周期：

- 自动将权限请求路由到 Slack 或 Opus
- 当它达到回合末尾时轻推 Claude 继续（你甚至可以踢开一个代理或使用提示决定 Claude 是否应继续）
- 预处理或后处理工具调用，例如添加你自己的日志

要求 Claude 添加钩子以开始。

<a href="https://x.com/bcherny/status/2021701059253874861"><img src="assets/boris-12-feb-26/9.webp" alt="设置钩子" width="50%" /></a>

---

## 10/ 自定义你的旋转动词

自定义你的旋转动词以从默认列表中添加或替换你自己的动词。将 `settings.json` 检入源控制以与团队共享动词。

<a href="https://x.com/bcherny/status/2021701145023197516"><img src="assets/boris-12-feb-26/10.webp" alt="自定义你的旋转动词" width="50%" /></a>

---

## 11/ 使用输出样式

运行 `/config` 并设置输出样式以让 Claude 使用不同的语气或格式进行回应。

- **解释** — 当熟悉新代码库时推荐，让 Claude 在工作时解释框架和代码模式
- **学习** — 让 Claude 指导你通过进行代码变化
- **自定义** — 创建自定义输出样式以调整 Claude 的声音

<a href="https://x.com/bcherny/status/2021701379409273093"><img src="assets/boris-12-feb-26/11.webp" alt="使用输出样式" width="50%" /></a>

---

## 12/ 自定义所有的东西！

Claude Code 开箱即用工作很好，但当你自定义时，将你的 `settings.json` 检入 git，以便你的团队也能受益。配置在多个级别支持：

- 对于你的代码库
- 对于子文件夹
- 仅为你
- 通过企业范围的政策

有 37 个设置和 84 个环境变量（使用你的 `settings.json` 中的 `"env"` 字段避免包装脚本），有很好的机会任何你想要的行为都是可配置的。

<a href="https://x.com/bcherny/status/2021701636075458648"><img src="assets/boris-12-feb-26/12.webp" alt="自定义所有的东西" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) X — 2026 年 2 月 12 日](https://x.com/bcherny)
- [Claude Code 终端设置文档](https://code.claude.com/docs/en/terminal)
- [Claude Code 插件和发现文档](https://code.claude.com/docs/en/discover-plugins)
- [Claude Code 子代理文档](https://code.claude.com/docs/en/sub-agents)
- [Claude Code 权限文档](https://code.claude.com/docs/en/permissions)
- [Claude Code 沙箱文档](https://code.claude.com/docs/en/sandbox)
- [Claude Code 状态线文档](https://code.claude.com/docs/en/statusline)
- [Claude Code 键盘快捷键文档](https://code.claude.com/docs/en/keybindings)
- [Claude Code 钩子参考](https://code.claude.com/docs/en/hooks)
- [Claude Code 输出样式文档](https://code.claude.com/docs/en/output-styles)
- [Claude Code 设置文档](https://code.claude.com/docs/en/settings)
