# claude-code-best-practice

实践使得claude完美

![updated with Claude Code](https://img.shields.io/badge/updated_with_Claude_Code-v2.1.87%20(Mar%2030%2C%202026%2011%3A44%20AM%20PKT)-white?style=flat&labelColor=555) <a href="https://github.com/shanraisshan/claude-code-best-practice/stargazers"><img src="https://img.shields.io/github/stars/shanraisshan/claude-code-best-practice?style=flat&label=%E2%98%85&labelColor=555&color=white" alt="GitHub Stars"></a><br>

[![Best Practice](!/tags/best-practice.svg)](best-practice/) [![Implemented](!/tags/implemented.svg)](implementation/) [![Orchestration Workflow](!/tags/orchestration-workflow.svg)](orchestration-workflow/orchestration-workflow.md) [![Boris](!/tags/boris-cherny.svg)](#-tips-and-tricks) ![Click on these badges below to see the actual sources](!/tags/click-badges.svg)<br>
<img src="!/tags/a.svg" height="14"> = 代理 · <img src="!/tags/c.svg" height="14"> = 命令 · <img src="!/tags/s.svg" height="14"> = 技能

<p align="center">
  <img src="!/claude-jumping.svg" alt="Claude Code mascot jumping" width="120" height="100"><br>
  <a href="https://github.com/trending"><img src="!/root/github-trending-day.svg" alt="GitHub Trending #1 Repository Of The Day"></a>
</p>

<p align="center">
  <img src="!/root/boris-slider.gif" alt="Boris Cherny on Claude Code" width="600"><br>
  Boris Cherny on X (<a href="https://x.com/bcherny/status/2007179832300581177">tweet 1</a> · <a href="https://x.com/bcherny/status/2017742741636321619">tweet 2</a> · <a href="https://x.com/bcherny/status/2021699851499798911">tweet 3</a>)
</p>


## 🧠 概念

| 功能 | 位置 | 描述 |
|---------|----------|-------------|
| <img src="!/tags/a.svg" height="14"> [**代理**](https://code.claude.com/docs/en/sub-agents) | `.claude/agents/<name>.md` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-subagents.md) [![Implemented](!/tags/implemented.svg)](implementation/claude-subagents-implementation.md) 在新的隔离上下文中的自主执行者 — 自定义工具、权限、模型、记忆和持久身份 |
| <img src="!/tags/c.svg" height="14"> [**命令**](https://code.claude.com/docs/en/slash-commands) | `.claude/commands/<name>.md` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-commands.md) [![Implemented](!/tags/implemented.svg)](implementation/claude-commands-implementation.md) 注入到现有上下文中的知识 — 用于工作流编排的简单用户调用的提示模板 |
| <img src="!/tags/s.svg" height="14"> [**技能**](https://code.claude.com/docs/en/skills) | `.claude/skills/<name>/SKILL.md` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-skills.md) [![Implemented](!/tags/implemented.svg)](implementation/claude-skills-implementation.md) 注入到现有上下文中的知识 — 可配置、可预加载、自动发现的，具有上下文分叉和渐进式披露 · [官方技能](https://github.com/anthropics/skills/tree/main/skills) |
| [**工作流**](https://code.claude.com/docs/en/common-workflows) | [`.claude/commands/weather-orchestrator.md`](.claude/commands/weather-orchestrator.md) | [![Orchestration Workflow](!/tags/orchestration-workflow.svg)](orchestration-workflow/orchestration-workflow.md) |
| [**钩子**](https://code.claude.com/docs/en/hooks) | `.claude/hooks/` | [![Best Practice](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-hooks) [![Implemented](!/tags/implemented.svg)](https://github.com/shanraisshan/claude-code-hooks) 在代理循环外运行在特定事件上的用户定义处理程序（脚本、HTTP、提示、代理）· [指南](https://code.claude.com/docs/en/hooks-guide) |
| [**MCP 服务器**](https://code.claude.com/docs/en/mcp) | `.claude/settings.json`, `.mcp.json` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-mcp.md) [![Implemented](!/tags/implemented.svg)](.mcp.json) 模型上下文协议连接到外部工具、数据库和 API |
| [**插件**](https://code.claude.com/docs/en/plugins) | 可分配的包 | 技能、代理、钩子、MCP 服务器和 LSP 服务器的捆绑包 · [市场](https://code.claude.com/docs/en/discover-plugins) · [创建市场](https://code.claude.com/docs/en/plugin-marketplaces) |
| [**设置**](https://code.claude.com/docs/en/settings) | `.claude/settings.json` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-settings.md) [![Implemented](!/tags/implemented.svg)](.claude/settings.json) 分层配置系统 · [权限](https://code.claude.com/docs/en/permissions) · [模型配置](https://code.claude.com/docs/en/model-config) · [输出样式](https://code.claude.com/docs/en/output-styles) · [沙箱](https://code.claude.com/docs/en/sandboxing) · [快捷键](https://code.claude.com/docs/en/keybindings) · [快速模式](https://code.claude.com/docs/en/fast-mode) |
| [**状态行**](https://code.claude.com/docs/en/statusline) | `.claude/settings.json` | [![Best Practice](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-status-line) [![Implemented](!/tags/implemented.svg)](.claude/settings.json) 显示上下文使用、模型、成本和会话信息的可自定义状态栏 |
| [**记忆**](https://code.claude.com/docs/en/memory) | `CLAUDE.md`, `.claude/rules/`, `~/.claude/rules/`, `~/.claude/projects/<project>/memory/` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-memory.md) [![Implemented](!/tags/implemented.svg)](CLAUDE.md) 通过 CLAUDE.md 文件和 `@path` 导入的持久上下文 · [自动记忆](https://code.claude.com/docs/en/memory) · [规则](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) |
| [**检查点**](https://code.claude.com/docs/en/checkpointing) | 自动（基于 git） | 文件编辑的自动跟踪，支持回放（`Esc Esc` 或 `/rewind`）和目标摘要 |
| [**CLI 启动标志**](https://code.claude.com/docs/en/cli-reference) | `claude [flags]` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-cli-startup-flags.md) 启动 Claude Code 的命令行标志、子命令和环境变量 · [交互模式](https://code.claude.com/docs/en/interactive-mode) |
| **AI 术语** | | [![Best Practice](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-codex-cursor-gemini/blob/main/reports/ai-terms.md) 代理工程 · 上下文工程 · 氛围编码 |
| [**最佳实践**](https://code.claude.com/docs/en/best-practices) | | 官方最佳实践 · [提示工程](https://github.com/anthropics/prompt-eng-interactive-tutorial) · [扩展 Claude Code](https://code.claude.com/docs/en/features-overview) |

### 🔥 热门

| 功能 | 位置 | 描述 |
|---------|----------|-------------|
| [**自动模式**](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) ![beta](!/tags/beta.svg) | `--permission-mode auto` | [![Best Practice](!/tags/best-practice.svg)](https://x.com/claudeai/status/2036503582166393240) 后台安全分类器替代手动权限提示 — Claude 决定什么是安全的，同时阻止提示注入和危险升级 · [博客](https://claude.com/blog/auto-mode) |
| [**频道**](https://code.claude.com/docs/en/channels) ![beta](!/tags/beta.svg) | `--channels`, 基于插件 | 从 Telegram、Discord 或 webhooks 推送事件到正在运行的会话 — Claude 在您离开时做出反应 · [参考](https://code.claude.com/docs/en/channels-reference) |
| [**Slack**](https://code.claude.com/docs/en/slack) | Slack 中的 `@Claude` | 在团队聊天中提及 @Claude 并附带编码任务 — 路由到 Claude Code 网络会话以进行错误修复、代码审查和并行任务执行 |
| [**代码审查**](https://code.claude.com/docs/en/code-review) ![beta](!/tags/beta.svg) | GitHub App（托管） | [![Best Practice](!/tags/best-practice.svg)](https://x.com/claudeai/status/2031088171262554195) 多代理 PR 分析，捕获错误、安全漏洞和回归 · [博客](https://claude.com/blog/code-review) |
| [**GitHub Actions**](https://code.claude.com/docs/en/github-actions) | `.github/workflows/` | 自动化 PR 审查、问题分类和 CI/CD 管道中的代码生成 · [GitLab CI/CD](https://code.claude.com/docs/en/gitlab-ci-cd) |
| [**Chrome**](https://code.claude.com/docs/en/chrome) ![beta](!/tags/beta.svg) | `--chrome`, 扩展 | [![Best Practice](!/tags/best-practice.svg)](reports/claude-in-chrome-v-chrome-devtools-mcp.md) 通过 Chrome 中的 Claude 进行浏览器自动化 — 测试网络应用、使用控制台调试、自动化表单、从页面提取数据 |
| [**定时任务**](https://code.claude.com/docs/en/scheduled-tasks) | `/loop`、`/schedule`、cron 工具 | [![Best Practice](!/tags/best-practice.svg)](https://x.com/bcherny/status/2030193932404150413) [![Implemented](!/tags/implemented.svg)](implementation/claude-scheduled-tasks-implementation.md) `/loop` 在本地按照循环计划运行提示（最多 3 天）· [`/schedule`](https://code.claude.com/docs/en/web-scheduled-tasks) 在 Anthropic 基础设施中在云中运行提示 — 即使在您的计算机关闭时也能工作 · [公告](https://x.com/noahzweben/status/2036129220959805859) |
| [**语音听写**](https://code.claude.com/docs/en/voice-dictation) ![beta](!/tags/beta.svg) | `/voice` | [![Best Practice](!/tags/best-practice.svg)](https://x.com/trq212/status/2028628570692890800) 支持 20 种语言和可重新绑定激活键的推动式通话语音输入 |
| [**简化和批处理**](https://code.claude.com/docs/en/skills#bundled-skills) | `/simplify`、`/batch` | [![Best Practice](!/tags/best-practice.svg)](https://x.com/bcherny/status/2027534984534544489) 用于代码质量和批量操作的内置技能 — 简化重构以提高重用性和效率，批处理在文件间运行命令 |
| [**代理团队**](https://code.claude.com/docs/en/agent-teams) ![beta](!/tags/beta.svg) | 内置（环境变量） | [![Best Practice](!/tags/best-practice.svg)](https://x.com/bcherny/status/2019472394696683904) [![Implemented](!/tags/implemented.svg)](implementation/claude-agent-teams-implementation.md) 多个代理在同一代码库上并行工作，具有共享任务协调 |
| [**远程控制**](https://code.claude.com/docs/en/remote-control) | `/remote-control`、`/rc` | [![Best Practice](!/tags/best-practice.svg)](https://x.com/noahzweben/status/2032533699116355819) 从任何设备继续本地会话 — 手机、平板电脑或浏览器 · [无头模式](https://code.claude.com/docs/en/headless) |
| [**Git Worktrees**](https://code.claude.com/docs/en/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees) | 内置 | [![Best Practice](!/tags/best-practice.svg)](https://x.com/bcherny/status/2025007393290272904) 用于并行开发的隔离 git 分支 — 每个代理获得自己的工作副本 |
| [**Ralph Wiggum 循环**](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum) | 插件 | [![Best Practice](!/tags/best-practice.svg)](https://github.com/ghuntley/how-to-ralph-wiggum) [![Implemented](!/tags/implemented.svg)](https://github.com/shanraisshan/novel-llm-26) 用于长期运行任务的自主开发循环 — 迭代直到完成 |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<a id="orchestration-workflow"></a>

## <a href="orchestration-workflow/orchestration-workflow.md"><img src="!/tags/orchestration-workflow-hd.svg" alt="Orchestration Workflow"></a>

见 [orchestration-workflow](orchestration-workflow/orchestration-workflow.md) 了解 <img src="!/tags/c.svg" height="14"> **命令** → <img src="!/tags/a.svg" height="14"> **代理** → <img src="!/tags/s.svg" height="14"> **技能** 模式的实现细节。


<p align="center">
  <img src="orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent Architecture Flow" width="100%">
</p>

<p align="center">
  <img src="orchestration-workflow/orchestration-workflow.gif" alt="Orchestration Workflow Demo" width="600">
</p>

![How to Use](!/tags/how-to-use.svg)

```bash
claude
/weather-orchestrator
```

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## ⚙️ 开发工作流

所有主要工作流汇聚到相同的架构模式：**研究 → 计划 → 执行 → 审查 → 发布**

| 名称 | ★ | 独特性 | 计划 | <img src="!/tags/a.svg" height="14"> | <img src="!/tags/c.svg" height="14"> | <img src="!/tags/s.svg" height="14"> |
|------|---|------------|------|---|---|---|
| [Superpowers](https://github.com/obra/superpowers) | 122k | ![TDD-first](https://img.shields.io/badge/TDD--first-ddf4ff) ![Iron Laws](https://img.shields.io/badge/Iron_Laws-ddf4ff) ![whole-plan review](https://img.shields.io/badge/whole--plan_review-ddf4ff) | <img src="!/tags/s.svg" height="14"> [writing-plans](https://github.com/obra/superpowers/tree/main/skills/writing-plans) | 5 | 3 | 14 |
| [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) | 116k | ![instinct scoring](https://img.shields.io/badge/instinct_scoring-ddf4ff) ![AgentShield](https://img.shields.io/badge/AgentShield-ddf4ff) ![multi-lang rules](https://img.shields.io/badge/multi--lang_rules-ddf4ff) | <img src="!/tags/a.svg" height="14"> [planner](https://github.com/affaan-m/everything-claude-code/blob/main/agents/planner.md) | 30 | 63 | 135 |
| [Spec Kit](https://github.com/github/spec-kit) | 83k | ![spec-driven](https://img.shields.io/badge/spec--driven-ddf4ff) ![constitution](https://img.shields.io/badge/constitution-ddf4ff) ![22+ tools](https://img.shields.io/badge/22%2B_tools-ddf4ff) | <img src="!/tags/c.svg" height="14"> [speckit.plan](https://github.com/github/spec-kit/blob/main/templates/commands/plan.md) | 0 | 9+ | 0 |
| [gstack](https://github.com/garrytan/gstack) | 55k | ![role personas](https://img.shields.io/badge/role_personas-ddf4ff) ![/codex review](https://img.shields.io/badge/%2Fcodex_review-ddf4ff) ![parallel sprints](https://img.shields.io/badge/parallel_sprints-ddf4ff) | <img src="!/tags/s.svg" height="14"> [autoplan](https://github.com/garrytan/gstack/tree/main/autoplan) | 0 | 0 | 28 |
| [Get Shit Done](https://github.com/gsd-build/get-shit-done) | 44k | ![fresh 200K contexts](https://img.shields.io/badge/fresh_200K_contexts-ddf4ff) ![wave execution](https://img.shields.io/badge/wave_execution-ddf4ff) ![XML plans](https://img.shields.io/badge/XML_plans-ddf4ff) | <img src="!/tags/a.svg" height="14"> [gsd-planner](https://github.com/gsd-build/get-shit-done/blob/main/agents/gsd-planner.md) | 18 | 57 | 0 |
| [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) | 43k | ![full SDLC](https://img.shields.io/badge/full_SDLC-ddf4ff) ![agent personas](https://img.shields.io/badge/agent_personas-ddf4ff) ![22+ platforms](https://img.shields.io/badge/22%2B_platforms-ddf4ff) | <img src="!/tags/s.svg" height="14"> [bmad-create-prd](https://github.com/bmad-code-org/BMAD-METHOD/tree/main/src/bmm-skills/2-plan-workflows/bmad-create-prd) | 0 | 0 | 40 |
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | 35k | ![delta specs](https://img.shields.io/badge/delta_specs-ddf4ff) ![brownfield](https://img.shields.io/badge/brownfield-ddf4ff) ![artifact DAG](https://img.shields.io/badge/artifact_DAG-ddf4ff) | <img src="!/tags/c.svg" height="14"> [opsx:propose](https://github.com/Fission-AI/OpenSpec/blob/main/src/commands/workflow/new-change.ts) | 0 | 11 | 0 |
| [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) | 12k | ![Compound Learning](https://img.shields.io/badge/Compound_Learning-ddf4ff) ![Multi-Platform CLI](https://img.shields.io/badge/Multi--Platform_CLI-ddf4ff) ![Plugin Marketplace](https://img.shields.io/badge/Plugin_Marketplace-ddf4ff) | <img src="!/tags/s.svg" height="14"> [ce-plan](https://github.com/EveryInc/compound-engineering-plugin/tree/main/plugins/compound-engineering/skills/ce-plan) | 48 | 4 | 42 |
| [HumanLayer](https://github.com/humanlayer/humanlayer) | 10k | ![RPI](https://img.shields.io/badge/RPI-ddf4ff) ![context engineering](https://img.shields.io/badge/context_engineering-ddf4ff) ![300k+ LOC](https://img.shields.io/badge/300k%2B_LOC-ddf4ff) | <img src="!/tags/c.svg" height="14"> [create_plan](https://github.com/humanlayer/humanlayer/blob/main/.claude/commands/create_plan.md) | 6 | 27 | 0 |

### 其他
- [跨模型（Claude Code + Codex）工作流](development-workflows/cross-model-workflow/cross-model-workflow.md) [![Implemented](!/tags/implemented.svg)](development-workflows/cross-model-workflow/cross-model-workflow.md)
- [RPI](development-workflows/rpi/rpi-workflow.md) [![Implemented](!/tags/implemented.svg)](development-workflows/rpi/rpi-workflow.md)
- [Ralph Wiggum 循环](https://www.youtube.com/watch?v=eAtvoGlpeRU) [![Implemented](!/tags/implemented.svg)](https://github.com/shanraisshan/novel-llm-26)
- [Andrej Karpathy（OpenAI 创始成员）工作流](https://x.com/karpathy/status/2015883857489522876)
- [Peter Steinberger（OpenClaw 创建者）工作流](https://youtu.be/8lF7HmQ_RgY?t=2582)
- Boris Cherny（Claude Code 创建者）工作流 — [13 个技巧](tips/claude-boris-13-tips-03-jan-26.md) · [10 个技巧](tips/claude-boris-10-tips-01-feb-26.md) · [12 个技巧](tips/claude-boris-12-tips-12-feb-26.md) · [2 个技巧](tips/claude-boris-2-tips-25-mar-26.md) · [15 个技巧](tips/claude-boris-15-tips-30-mar-26.md) [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny)

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 💡 技巧和诀窍 (86)

🚫👶 = 不要过度指导

[提示](#tips-prompting) · [计划](#tips-planning) · [CLAUDE.md](#tips-claudemd) · [代理](#tips-agents) · [命令](#tips-commands) · [技能](#tips-skills) · [钩子](#tips-hooks) · [工作流](#tips-workflows) · [高级](#tips-workflows-advanced) · [Git / PR](#tips-git-pr) · [调试](#tips-debugging) · [实用工具](#tips-utilities) · [日常](#tips-daily)

![Community](!/tags/community.svg)

<a id="tips-prompting"></a>■ **提示 (3)**

| 技巧 | 来源 |
|-----|--------|
| 挑战 Claude — "关于这些变更测试我，在我通过你的测试前不要提交 PR。"或"向我证明这有效"并让 Claude 比较 main 和你的分支之间的差异 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| 在一个一般的修复之后 — "知道现在你知道的一切，从头开始并实现优雅的解决方案" 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| Claude 本身会修复大多数错误 — 粘贴错误，说"修复"，不要微管理如何修复 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742750473720121) |

<a id="tips-planning"></a>■ **计划/规范 (6)**

| 技巧 | 来源 |
|-----|--------|
| 始终从[计划模式](https://code.claude.com/docs/en/common-workflows)开始 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179845336527000) |
| 从最小规范或提示开始，并让 Claude 使用 [AskUserQuestion](https://code.claude.com/docs/en/cli-reference) 工具来采访你，然后创建新会话来执行规范 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2005315275026260309) |
| 始终制定阶段性的门控计划，每个阶段都有多个测试（单元、自动化、集成） | |
| 启动第二个 Claude 作为员工工程师审查你的计划，或使用[跨模型](development-workflows/cross-model-workflow/cross-model-workflow.md)进行审查 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742745365057733) |
| 编写详细规范并减少歧义再交接工作 — 你越具体，输出就越好 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| 原型 > PRD — 构建 20-30 个版本而不是编写规范，构建成本低，所以尝试多次 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=3630) [![Video](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=3630) |

<a id="tips-claudemd"></a>■ **CLAUDE.md (7)**

| 技巧 | 来源 |
|-----|--------|
| [CLAUDE.md](https://code.claude.com/docs/en/memory) 应该目标低于[200 行](https://code.claude.com/docs/en/memory#write-effective-instructions)每个文件。[humanlayer 中的 60 行](https://www.humanlayer.dev/blog/writing-a-good-claude-md)（[仍然不能 100% 保证](https://www.reddit.com/r/ClaudeCode/comments/1qn9pb9/claudemd_says_must_use_agent_claude_ignores_it_80/)） | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179840848597422) [![Dex](!/tags/dex.svg)](https://www.humanlayer.dev/blog/writing-a-good-claude-md) |
| 用 [\<important if="..."\> 标签](https://www.hlyr.dev/blog/stop-claude-from-ignoring-your-claude-md)包装特定领域的 CLAUDE.md 规则以停止 Claude 随着文件增长而忽略它们 | [![Dex](!/tags/dex.svg)](https://www.hlyr.dev/blog/stop-claude-from-ignoring-your-claude-md) |
| 对 monorepo 使用[多个 CLAUDE.md](best-practice/claude-memory.md) — 祖先 + 后代加载 | |
| 使用 [.claude/rules/](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) 拆分大型指导 | |
| [memory.md](https://code.claude.com/docs/en/memory)、constitution.md 不保证任何内容 | |
| 任何开发者应该能够启动 Claude，说"运行测试"，第一次就能在它上面工作 — 如果不能，你的 CLAUDE.md 缺少必要的设置/构建/测试命令 | [![Dex](!/tags/dex.svg)](https://x.com/dexhorthy/status/2034713765401551053) |
| 保持代码库清洁并完成迁移 — 部分迁移的框架会混淆可能选择错误模式的模型 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=1112) [![Video](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=1112) |
| 使用 [settings.json](best-practice/claude-settings.md) 来实现工具强制行为（归属、权限、模型）— 不要在 CLAUDE.md 中放"切勿添加 Co-Authored-By"，当 `attribution.commit: ""` 是决定性的时 | [![davila7](!/tags/davila7.svg)](https://x.com/dani_avila7/status/2036182734310195550) |

<a id="tips-agents"></a><img src="!/tags/a.svg" height="14"> **代理 (4)**

| 技巧 | 来源 |
|-----|--------|
| 拥有功能特定的[代理](https://code.claude.com/docs/en/sub-agents)（额外上下文）及其[技能](https://code.claude.com/docs/en/skills)（渐进式披露）而不是通用的 qa、后端工程师 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179850139000872) |
| 说"使用代理"来对问题投入更多计算 — 将任务卸载到保持主上下文清洁和专注 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742755737555434) |
| [具有 tmux 的代理团队](https://code.claude.com/docs/en/agent-teams)和 [git worktrees](https://x.com/bcherny/status/2025007393290272904) 进行并行开发 | |
| 使用[测试时计算](https://code.claude.com/docs/en/sub-agents) — 单独的上下文窗口使结果更好；一个代理可能导致错误，另一个（相同模型）可以找到它们 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2031151689219321886) |

<a id="tips-commands"></a><img src="!/tags/c.svg" height="14"> **命令 (3)**

| 技巧 | 来源 |
|-----|--------|
| 使用[命令](https://code.claude.com/docs/en/slash-commands)进行工作流而不是[代理](https://code.claude.com/docs/en/sub-agents) | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179847949500714) |
| 为每个你一天做许多次的"内部循环"工作流使用[斜杠命令](https://code.claude.com/docs/en/slash-commands) — 节省重复提示，命令生活在 `.claude/commands/` 中并签入 git | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179847949500714) |
| 如果你一天做某事超过一次，把它变成[技能](https://code.claude.com/docs/en/skills)或[命令](https://code.claude.com/docs/en/slash-commands) — 构建 `/techdebt`、context-dump 或分析命令 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742748984742078) |

<a id="tips-skills"></a><img src="!/tags/s.svg" height="14"> **技能 (9)**

| 技巧 | 来源 |
|-----|--------|
| 使用 [context: fork](https://code.claude.com/docs/en/skills) 在隔离的代理中运行技能 — 主上下文仅看到最终结果，不是中间工具调用。agent 字段让你设置代理类型 | [![Lydia](!/tags/lydia.svg)](https://x.com/lydiahallie/status/2033603164398883042) |
| 为 monorepo 使用[子文件夹中的技能](reports/claude-skills-for-larger-mono-repos.md) | |
| 技能是文件夹，不是文件 — 对[渐进式披露](https://code.claude.com/docs/en/skills)使用 references/、scripts/、examples/ 子目录 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在每个技能中构建一个 Gotchas 部分 — 最高信号内容，添加 Claude 随时间推移的失败点 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 技能描述字段是触发器，不是摘要 — 为模型编写它（"我何时应该触发？"） | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 不要在技能中说显而易见的事情 — 专注于将 Claude 推出其默认行为的内容 🚫👶 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 不要在技能中限制 Claude — 给出目标和约束，不是规定性逐步说明 🚫👶 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在技能中包含脚本和库以便 Claude 组合而不是重建样板 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在 SKILL.md 中嵌入 `` !`command` `` 将动态 shell 输出注入到提示中 — Claude 在调用时运行它，模型仅看到结果 | [![Lydia](!/tags/lydia.svg)](https://x.com/lydiahallie/status/2034337963820327017) |

<a id="tips-hooks"></a>■ **钩子 (5)**

| 技巧 | 来源 |
|-----|--------|
| 在技能中使用[按需钩子](https://code.claude.com/docs/en/skills) — /careful 阻止破坏性命令，/freeze 阻止目录外编辑 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 PreToolUse 钩子[衡量技能使用](https://code.claude.com/docs/en/skills)以找到受欢迎或触发不足的技能 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 [PostToolUse 钩子](https://code.claude.com/docs/en/hooks)自动格式化代码 — Claude 生成格式良好的代码，钩子处理最后 10% 以避免 CI 失败 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179852047335529) |
| 通过钩子将[权限请求](https://code.claude.com/docs/en/hooks)路由到 Opus — 让它扫描攻击并自动批准安全的 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742755737555434) |
| 使用 [Stop 钩子](https://code.claude.com/docs/en/hooks)在轮次结束时推动 Claude 继续或验证其工作 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021701059253874861) |

<a id="tips-workflows"></a>■ **工作流 (7)**

| 技巧 | 来源 |
|-----|--------|
| 避免代理愚蠢区域，在 50% 时手动进行 [/compact](https://code.claude.com/docs/en/interactive-mode)。如果切换到新任务，使用 [/clear](https://code.claude.com/docs/en/cli-reference) 在会话中重置上下文 | |
| 普通 cc 优于任何具有较小任务的工作流 | |
| 使用 [/model](https://code.claude.com/docs/en/model-config) 选择模型和推理，[/context](https://code.claude.com/docs/en/interactive-mode) 查看上下文使用，[/usage](https://code.claude.com/docs/en/costs) 检查计划限制，[/extra-usage](https://code.claude.com/docs/en/interactive-mode) 配置溢出计费，[/config](https://code.claude.com/docs/en/settings) 配置设置 — 使用 Opus 进行计划模式和 Sonnet 进行代码以获得两者最佳 | [![Cat](!/tags/cat-wu.svg)](https://x.com/_catwu/status/1955694117264261609) |
| 始终使用[思考模式](https://code.claude.com/docs/en/model-config)true（查看推理）和 [Output Style](https://code.claude.com/docs/en/output-styles) Explanatory（查看带有 ★ Insight 框的详细输出）在 [/config](https://code.claude.com/docs/en/settings) 中以更好地理解 Claude 的决策 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179838864666847) |
| 在提示中使用 ultrathink 关键字进行[高努力推理](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking#tips-and-best-practices) | |
| [/rename](https://code.claude.com/docs/en/cli-reference) 重要会话（例如 [TODO - refactor task]）并稍后 [/resume](https://code.claude.com/docs/en/cli-reference) 它们 — 标记每个实例同时运行多个 Claude | [![Cat](!/tags/cat-wu.svg)](https://every.to/podcast/how-to-use-claude-code-like-the-people-who-built-it) |
| 使用 [Esc Esc 或 /rewind](https://code.claude.com/docs/en/checkpointing) 撤销当 Claude 偏离轨道时而不是在相同上下文中尝试修复 | |

<a id="tips-workflows-advanced"></a>■ **工作流高级 (6)**

| 技巧 | 来源 |
|-----|--------|
| 经常使用 ASCII 图来理解你的架构 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742759218794768) |
| 使用 [/loop](https://code.claude.com/docs/en/scheduled-tasks) 用于本地循环监控（最多 3 天）· 使用 [/schedule](https://code.claude.com/docs/en/web-scheduled-tasks) 用于云基络环任务，即使你的机器关闭也能运行 | |
| 对长期运行的自主任务使用 [Ralph Wiggum 插件](https://github.com/shanraisshan/novel-llm-26) | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179858435281082) |
| [/permissions](https://code.claude.com/docs/en/permissions) 带有通配符语法（Bash(npm run *)、Edit(/docs/**)）而不是 dangerously-skip-permissions | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179854077407667) |
| [/sandbox](https://code.claude.com/docs/en/sandboxing) 通过文件和网络隔离减少权限提示 — 内部减少 84% | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021700506465579443) [![Cat](!/tags/cat-wu.svg)](https://creatoreconomy.so/p/inside-claude-code-how-an-ai-native-actually-works-cat-wu) |
| 投资[产品验证](https://code.claude.com/docs/en/skills)技能（signup-flow-driver、checkout-verifier）— 值得花一周来完善 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |

<a id="tips-git-pr"></a>■ **Git / PR (5)**

| 技巧 | 来源 |
|-----|--------|
| 保持 PR 小而专注 — [p50 of 118 lines](tips/claude-boris-2-tips-25-mar-26.md)（141 PRs，一天内 45K 行变更），每个 PR 一个功能，更容易审查和回退 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038552880018538749) |
| 始终[压缩合并](tips/claude-boris-2-tips-25-mar-26.md) PR — 干净的线性历史，每个功能一个提交，轻松 git 回退和 git 二分 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038552880018538749) |
| 频繁提交 — 尽量至少每小时提交一次，任务完成后立即提交 | |
| 在同事的 PR 上标记 [@claude](https://github.com/apps/claude) 以自动生成重复审查反馈的 lint 规则 — 将自己从代码审查中自动化出去 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=2715) [![Video](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=2715) |
| 使用 [/code-review](https://code.claude.com/docs/en/code-review) 进行多代理 PR 分析 — 在合并前捕获错误、安全漏洞和回归 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2031089411820228645) |

<a id="tips-debugging"></a>■ **调试 (7)**

| 技巧 | 来源 |
|-----|--------|
| 养成习惯，在卡住任何问题时截图并与 Claude 分享 | |
| 使用 mcp（[Chrome 中的 Claude](https://code.claude.com/docs/en/chrome)、[Playwright](https://github.com/microsoft/playwright-mcp)、[Chrome DevTools](https://developer.chrome.com/blog/chrome-devtools-mcp)）让 claude 自己看到 chrome 控制台日志 | |
| 始终要求 claude 将终端（你想看日志的）作为后台任务运行以更好地调试 | |
| [/doctor](https://code.claude.com/docs/en/cli-reference) 诊断安装、身份验证和配置问题 | |
| 压缩期间的错误可以通过使用 [/model](https://code.claude.com/docs/en/model-config) 选择 1M 代币模型，然后运行 [/compact](https://code.claude.com/docs/en/interactive-mode) 来解决 | |
| 将[跨模型](development-workflows/cross-model-workflow/cross-model-workflow.md)用于 QA — 例如 [Codex](https://github.com/shanraisshan/codex-cli-best-practice) 进行计划和实现审查 | |
| 代理搜索（glob + grep）优于 RAG — Claude Code 尝试并放弃了向量数据库，因为代码漂移出同步且权限很复杂 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=3095) [![Video](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=3095) |

<a id="tips-utilities"></a>■ **实用工具 (5)**

| 技巧 | 来源 |
|-----|--------|
| [iTerm](https://iterm2.com/)/[Ghostty](https://ghostty.org/)/[tmux](https://github.com/tmux/tmux) 终端而不是 IDE（[VS Code](https://code.visualstudio.com/)/[Cursor](https://www.cursor.com/)） | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742753971769626) |
| [Wispr Flow](https://wisprflow.ai) 用于语音提示（10 倍生产力） | |
| [claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks) 用于 claude 反馈 | |
| [状态行](https://github.com/shanraisshan/claude-code-status-line) 用于上下文意识和快速压缩 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021700784019452195) |
| 探索 [settings.json](best-practice/claude-settings.md) 功能如 [Plans Directory](best-practice/claude-settings.md#plans-directory)、[Spinner Verbs](best-practice/claude-settings.md#display--ux) 以获得个性化体验 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021701145023197516) |

<a id="tips-daily"></a>■ **日常 (3)**

| 技巧 | 来源 |
|-----|--------|
| [更新](https://code.claude.com/docs/en/setup) Claude Code 每天并通过阅读 [changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md) 开始你的一天 | |
| 关注 [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/)、[r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/) | ![Reddit](https://img.shields.io/badge/-FF4500?style=flat&logo=reddit&logoColor=white) |
| 关注 [Boris](https://x.com/bcherny)、[Thariq](https://x.com/trq212)、[Cat](https://x.com/_catwu)、[Lydia](https://x.com/lydiahallie)、[Noah](https://x.com/noahzweben)、[Anthony](https://x.com/amorriscode)、[Alex](https://x.com/alexalbert__)、[Kenneth](https://x.com/neilhtennek)、[Claude](https://x.com/claudeai) | ![X](https://img.shields.io/badge/-000?style=flat&logo=x&logoColor=white) |

![Boris Cherny + Team](!/tags/boris-team.svg)

| 文章 / 推文 | 来源 |
|-----------------|--------|
| [Claude Code 中的 15 个隐藏和未充分利用的功能（Boris）\| 30/Mar/26](tips/claude-boris-15-tips-30-mar-26.md) | [推文](https://x.com/bcherny/status/2038454336355999749) |
| [压缩合并和 PR 大小分布（Boris）\| 25/Mar/26](tips/claude-boris-2-tips-25-mar-26.md) | [推文](https://x.com/bcherny/status/2038552880018538749) |
| [从构建 Claude Code 中学到的经验：我们如何使用技能（Thariq）\| 17/Mar/26](tips/claude-thariq-tips-17-mar-26.md) | [文章](https://x.com/trq212/status/2033949937936085378) |
| [代码审查和测试时计算（Boris）\| 10/Mar/26](tips/claude-boris-2-tips-10-mar-26.md) | [推文](https://x.com/bcherny/status/2031089411820228645) |
| /loop — 为期最多 3 天的定时循环任务（Boris）\| 07 Mar 2026 | [推文](https://x.com/bcherny/status/2030193932404150413) |
| AskUserQuestion + ASCII Markdowns（Thariq）\| 28 Feb 2026 | [推文](https://x.com/trq212/status/2027543858289250472) |
| 像代理一样看 - 从构建 Claude Code 中学到的经验（Thariq）\| 28 Feb 2026 | [文章](https://x.com/trq212/status/2027463795355095314) |
| Git Worktrees - boris 使用的 5 种方式\| 21 Feb 2026 | [推文](https://x.com/bcherny/status/2025007393290272904) |
| 从构建 Claude Code 中学到的经验：提示缓存无所不在（Thariq）\| 20 Feb 2026 | [文章](https://x.com/trq212/status/2024574133011673516) |
| [人们定制他们的 claude 的 12 种方式（Boris）\| 12/Feb/26](tips/claude-boris-12-tips-12-feb-26.md) | [推文](https://x.com/bcherny/status/2021699851499798911) |
| [使用 Claude Code 的 10 个技巧来自团队（Boris）\| 01/Feb/26](tips/claude-boris-10-tips-01-feb-26.md) | [推文](https://x.com/bcherny/status/2017742741636321619) |
| [我如何使用 Claude Code — 来自我令人惊讶普通设置的 13 个技巧（Boris）\| 03/Jan/26](tips/claude-boris-13-tips-03-jan-26.md) | [推文](https://x.com/bcherny/status/2007179832300581177) |
| 要求 Claude 使用 AskUserQuestion 工具采访你（Thariq）\| 28/Dec/25 | [推文](https://x.com/trq212/status/2005315275026260309) |
| 始终使用计划模式，给 Claude 验证的方式，使用 /code-review（Boris）\| 27/Dec/25 | [推文](https://x.com/bcherny/status/2004711722926616680) |

![视频 / 播客](!/tags/videos-podcasts.svg)

| 视频 / 播客 | YouTube |
|-----------------|---------|
| 与 Boris Cherny 构建 Claude Code（Boris）\| 04 Mar 2026 \| The Pragmatic Engineer | [YouTube](https://youtu.be/julbw1JuAz0) |
| Claude Code 负责人：编码解决后会发生什么（Boris）\| 19 Feb 2026 \| Lenny's Podcast | [YouTube](https://youtu.be/We7BZVKbCVw) |
| 与创建者 Boris Cherny 的 Claude Code 内部（Boris）\| 17 Feb 2026 \| Y Combinator | [YouTube](https://youtu.be/PQU9o_5rHC4) |
| Boris Cherny（Claude Code 创建者）关于他的职业如何增长（Boris）\| 15 Dec 2025 \| Ryan Peterman | [YouTube](https://youtu.be/AmdLVWMdjOk) |
| 从构建它的工程师那里了解 Claude Code 的秘密（Cat）\| 29 Oct 2025 \| Every | [YouTube](https://youtu.be/IDSAMqip6ms) |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## ☠️ 创业/企业

| Claude | 替代 |
|-|-|
|[**代码审查**](https://code.claude.com/docs/en/code-review)|[Greptile](https://greptile.com), [CodeRabbit](https://coderabbit.ai), [Devin Review](https://devin.ai), [OpenDiff](https://opendiff.com), [Cursor BugBot](https://bugbot.dev)|
|[**语音听写**](https://code.claude.com/docs/en/voice-dictation)|[Wispr Flow](https://wisprflow.ai), [SuperWhisper](https://superwhisper.com/)|
|[**远程控制**](https://code.claude.com/docs/en/remote-control)|[OpenClaw](https://openclaw.ai/)
|[**Cowork**](https://claude.com/blog/cowork-research-preview)|[OpenAI Operator](https://openai.com/operator), [AgentShadow](https://agentshadow.ai)
|[**任务**](https://x.com/trq212/status/2014480496013803643)|[Beads](https://github.com/steveyegge/beads)
|[**计划模式**](https://code.claude.com/docs/en/common-workflows)|[Agent OS](https://github.com/buildermethods/agent-os)|
|[**技能 / 插件**](https://code.claude.com/docs/en/plugins)|YC AI 包装初创企业（[reddit](https://reddit.com/r/ClaudeAI/comments/1r6bh4d/claude_code_skills_are_basically_yc_ai_startup/))|

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<a id="billion-dollar-questions"></a>
![十亿美元问题](!/tags/billion-dollar-questions.svg)

*如果你有答案，请让我在 shanraisshan@gmail.com 知道*

**记忆和指导 (4)**

1. 你究竟应该把什么放在你的 CLAUDE.md 中 — 你应该留出什么？
2. 如果你已经有一个 CLAUDE.md，是否真的需要单独的 constitution.md 或 rules.md？
3. 你应该多久更新一次你的 CLAUDE.md，你如何知道它何时变得陈旧？
4. 为什么 Claude 仍然忽略 CLAUDE.md 指导 — 即使它们以全大写说 MUST？（[reddit](https://reddit.com/r/ClaudeCode/comments/1qn9pb9/claudemd_says_must_use_agent_claude_ignores_it_80/)）

**代理、技能和工作流 (6)**

1. 何时应该使用命令与代理与技能 — 以及何时普通 Claude Code 更好？
2. 应该多久更新一次你的代理、命令和工作流，因为模型改进？
3. 给你的代理一个详细的个性可以改善质量吗？对于研究/QA 代理，什么是"完美的个性/提示"的样子？
4. 你应该依赖 Claude Code 的内置计划模式 — 还是构建你自己的强制执行团队工作流的计划命令/代理？
5. 如果你有一个个人技能（例如，/implement 带着你的编码风格），你如何在不冲突的情况下整合社区技能（例如，/simplify）— 当他们不同意时谁赢？
6. 我们到了吗？我们能将现有的代码库转换为规范，删除代码，并让 AI 单独从这些规范重新生成完全相同的代码吗？

**规范和文档 (3)**

1. 你的 repo 中的每个功能都应该有一个规范作为 markdown 文件吗？
2. 你需要多久更新一次规范，所以当实现新功能时它们不会变得过时？
3. 实现新功能时，你如何处理其他功能规范的涟漪效应？

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 报告

<p align="center">
  <a href="reports/claude-agent-sdk-vs-cli-system-prompts.md"><img src="https://img.shields.io/badge/Agent_SDK_vs_CLI-555?style=for-the-badge" alt="Agent SDK vs CLI"></a>
  <a href="reports/claude-in-chrome-v-chrome-devtools-mcp.md"><img src="https://img.shields.io/badge/Browser_Automation_MCP-555?style=for-the-badge" alt="Browser Automation MCP"></a>
  <a href="reports/claude-global-vs-project-settings.md"><img src="https://img.shields.io/badge/Global_vs_Project_Settings-555?style=for-the-badge" alt="Global vs Project Settings"></a>
  <a href="reports/claude-skills-for-larger-mono-repos.md"><img src="https://img.shields.io/badge/Skills_in_Monorepos-555?style=for-the-badge" alt="Skills in Monorepos"></a>
  <br>
  <a href="reports/claude-agent-memory.md"><img src="https://img.shields.io/badge/Agent_Memory-555?style=for-the-badge" alt="Agent Memory"></a>
  <a href="reports/claude-advanced-tool-use.md"><img src="https://img.shields.io/badge/Advanced_Tool_Use-555?style=for-the-badge" alt="Advanced Tool Use"></a>
  <a href="reports/claude-usage-and-rate-limits.md"><img src="https://img.shields.io/badge/Usage_&_Rate_Limits-555?style=for-the-badge" alt="Usage & Rate Limits"></a>
  <a href="reports/claude-agent-command-skill.md"><img src="https://img.shields.io/badge/Agents_vs_Commands_vs_Skills-555?style=for-the-badge" alt="Agents vs Commands vs Skills"></a>
  <br>
  <a href="reports/llm-day-to-day-degradation.md"><img src="https://img.shields.io/badge/LLM_Degradation-555?style=for-the-badge" alt="LLM Degradation"></a>
</p>

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

![使用方式](!/tags/how-to-use.svg)

```
1. 像学习课程一样阅读 repo，在尝试使用它们之前学习命令、代理、技能和钩子是什么。
2. 克隆此 repo 并使用示例进行实验，尝试 /weather-orchestrator，听钩子声音，运行代理团队，这样你可以看到东西实际上是如何工作的。
3. 进入你自己的项目并要求 Claude 建议你应该从此 repo 添加哪些最佳实践，给它这个 repo 作为参考，所以它知道什么是可能的。
```

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<p align="center">
  <a href="https://github.com/trending?since=monthly"><img src="!/root/github-trending.png" alt="GitHub Trending" width="1200"></a><br>
  ✨2026 年 3 月在 Github 趋势✨
</p>

## 其他 Repos

<a href="https://github.com/shanraisshan/claude-code-hooks"><img src="!/claude-speaking.svg" alt="Claude Code Hooks" width="40" height="40" align="center"></a> <a href="https://github.com/shanraisshan/claude-code-hooks"><strong>claude-code-hooks</strong></a> · <a href="https://github.com/shanraisshan/codex-cli-best-practice"><img src="!/codex-jumping.svg" alt="Codex CLI" width="40" height="40" align="center"></a> <a href="https://github.com/shanraisshan/codex-cli-best-practice"><strong>codex-cli-best-practice</strong></a> · <a href="https://github.com/shanraisshan/codex-cli-hooks"><img src="!/codex-speaking.svg" alt="Codex CLI Hooks" width="40" height="40" align="center"></a> <a href="https://github.com/shanraisshan/codex-cli-hooks"><strong>codex-cli-hooks</strong></a>

## 开发者

![开发者](!/tags/developed-by.svg)

> | 工作流 | 描述 |
> |----------|-------------|
> | /workflows:development-workflows | 通过并行研究所有 8 个工作流 repos 来更新开发工作流表和跨工作流分析报告 |
> | /workflows:best-practice:workflow-concepts | 使用最新的 Claude Code 功能和概念更新 README 概念部分 |
> | /workflows:best-practice:workflow-claude-settings | 跟踪 Claude Code 设置报告更改并找到需要更新的内容 |
> | /workflows:best-practice:workflow-claude-subagents | 跟踪 Claude Code 代理报告更改并找到需要更新的内容 |
> | /workflows:best-practice:workflow-claude-commands | 跟踪 Claude Code 命令报告更改并找到需要更新的内容 |
> | /workflows:best-practice:workflow-claude-skills | 跟踪 Claude Code 技能报告更改并找到需要更新的内容 |

[![Claude for OSS](!/tags/claude-for-oss.svg)](https://claude.com/contact-sales/claude-for-oss)
[![Claude 社区大使](!/tags/claude-community-ambassador.svg)](https://claude.com/community/ambassadors)
[![Claude 认证架构师](!/tags/claude-certified-architect.svg)](https://anthropic.skilljar.com/claude-certified-architect-foundations-access-request)
[![Anthropic Academy](!/tags/anthropic-academy.svg)](https://anthropic.skilljar.com/)

## 星标历史

[![Star History Chart](https://api.star-history.com/svg?repos=shanraisshan/claude-code-best-practice&type=Date)](https://star-history.com/#shanraisshan/claude-code-best-practice&Date)
