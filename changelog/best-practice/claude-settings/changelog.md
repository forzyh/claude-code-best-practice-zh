# 设置报告 — 变更日志

## 状态说明

| 状态 | 含义 |
|--------|---------|
| ✅ `完成 (原因)` | 操作已成功执行 |
| ❌ `无效 (原因)` | 发现不正确、不适用或有意为之 |
| ✋ `待处理 (原因)` | 操作延迟 — 等待外部依赖或用户决定 |

---

## [2026-03-05 06:18 AM PKT] Claude Code v2.1.69

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 缺失设置 | 添加 13 个非钩子缺失设置键 (`$schema`, `availableModels`, `fastModePerSessionOptIn`, `teammateMode`, `prefersReducedMotion`, `sandbox.filesystem.*`, `sandbox.network.allowManagedDomainsOnly`, `sandbox.enableWeakerNetworkIsolation`, `allowManagedMcpServersOnly`, `blockedMarketplaces`, `includeGitInstructions`, `pluginTrustMessage`, `fileSuggestion` 表条目) | ✅ 完成 (添加到报告) |
| 2 | 高 | 缺失环境变量 | 添加缺失的环境变量，包括 `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING`, `CLAUDE_CODE_DISABLE_1M_CONTEXT`, `CLAUDE_CODE_ACCOUNT_UUID`, `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS`, `ENABLE_CLAUDEAI_MCP_SERVERS` 及其他 | ✅ 完成 (添加 13 个缺失的环保变量到报告) |
| 3 | 高 | 工作强度默认值 | 更新工作强度默认值从"高"改为"中"对于 Max/Team 订阅用户；添加 Sonnet 4.6 支持 (v2.1.68 更改) | ✅ 完成 (更新默认值并添加 Sonnet 注释) |
| 4 | 中 | 设置层次结构 | 通过 macOS plist/Windows 注册表添加托管设置 (v2.1.61/v2.1.69)；记录跨范围的数组合并行为 | ✅ 完成 (添加 plist/registry 和合并注释) |
| 5 | 中 | 沙箱文件系统 | 添加 `sandbox.filesystem.allowWrite`, `denyWrite`, `denyRead` 带路径前缀语义 (`//`, `~/`, `/`, `./`) | ✅ 完成 (添加到沙箱表) |
| 6 | 中 | 权限语法 | 添加 `Agent(name)` 权限模式；记录 `MCP(server:tool)` 语法形式 | ✅ 完成 (添加到工具语法表) |
| 7 | 中 | 插件差距 | 添加 `blockedMarketplaces`, `pluginTrustMessage` | ✅ 完成 (添加到插件表) |
| 8 | 中 | 模型配置 | 添加 `availableModels` 设置 | ✅ 完成 (添加到常规设置表) |
| 9 | 中 | 可疑键 | 验证 `sandbox.network.deniedDomains`, `sandbox.ignoreViolations`, `pluginConfigs` — 在报告中存在但不在官方文档中 | ✋ 待处理 (保留在报告中等待验证) |
| 10 | 低 | 标题计数 | 从"38 个设置和 84 个环保变量"更新标题以反映实际计数 (~55+ 个设置，~110+ 个环保变量) | ✅ 完成 (更新标题) |
| 11 | 低 | CLAUDE.md 同步 | 更新 CLAUDE.md 配置层次结构 (添加托管/CLI/用户级别) | ✋ 待处理 (等待用户批准) |
| 12 | 低 | 示例更新 | 使用 `$schema`, 沙箱文件系统, `Agent(*)` 更新快速参考示例, 移除钩子示例 | ✅ 完成 (示例已更新) |
| 13 | 中 | 钩子重定向 | 用指向 claude-code-hooks 仓库的重定向替换钩子部分 | ✅ 完成 (钩子外包到专用仓库) |

---

## [2026-03-07 02:17 PM PKT] Claude Code v2.1.71

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 更改行为 | 修复 `teammateMode`: 类型 `boolean` → `string`, 默认 `false` → `"auto"`, 描述 → "代理团队显示: auto, in-process, tmux" | ✅ 完成 (类型、默认值和描述已更新) |
| 2 | 高 | 新设置 | 添加 `allowManagedPermissionRulesOnly` 到权限表 (布尔值，仅托管) | ✅ 完成 (添加到权限键表) |
| 3 | 高 | 缺失环境变量 | 添加 ~31 个缺失的环保变量，包括确认的和代理报告的 | ✅ 完成 (添加 31 个环保变量到表) |
| 4 | 中 | 更改默认值 | 修复 `plansDirectory` 默认值从 `.claude/plans/` 改为 `~/.claude/plans` | ✅ 完成 (默认值已更新) |
| 5 | 中 | 更改描述 | 修复 `sandbox.enableWeakerNetworkIsolation` 描述为"(仅 macOS) 允许系统 TLS 信任访问；降低安全性" | ✅ 完成 (描述已更新) |
| 6 | 中 | 范围修复 | 修复 `extraKnownMarketplaces` 范围从"任何"改为"项目" | ✅ 完成 (范围和描述已更新) |
| 7 | 中 | 边界违规 | 在 `claude-cli-startup-flags.md` 中用对设置报告的交叉引用替换 `CLAUDE_CODE_EFFORT_LEVEL` | ✅ 完成 (用链接替换) |
| 8 | 中 | 版本徽章 | 将报告版本从 v2.1.69 更新到 v2.1.71 | ✅ 完成 (徽章和标题已更新) |
| 9 | 低 | 可疑键 | 验证 `skipWebFetchPreflight`, `sandbox.ignoreViolations`, `sandbox.network.deniedDomains`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` | ✋ 待处理 (保留在报告中等待验证 — 从 2026-03-05 开始重复) |
| 10 | 低 | CLAUDE.md 同步 | 更新 CLAUDE.md 配置层次结构 (3 级 → 5 级以上) | ✅ 完成 (更新为 5 级层次结构，包含托管层) |

---

## [2026-03-12 12:23 PM PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 更改行为 | 修复 `dontAsk` 权限模式描述："自动接受所有工具" → "自动拒绝工具，除非通过 `/permissions` 或 `permissions.allow` 规则预批准" | ✅ 完成 (根据官方权限文档修正描述) |
| 2 | 高 | 新设置 | 添加 `modelOverrides` 到模型配置部分 (对象，映射 Anthropic 模型 ID 到提供商特定 ID 如 Bedrock ARN) | ✅ 完成 (添加示例和描述) |
| 3 | 高 | 新设置 | 添加 `allow_remote_sessions` 到仅托管设置列表 (布尔值，默认 `true`，控制远程控制/网络会话访问) | ✅ 完成 (添加到权限键表) |
| 4 | 高 | 更改默认值 | 修复 `$schema` URL 从 `https://www.schemastore.org/...` 改为 `https://json.schemastore.org/...` 根据官方文档 | ✅ 完成 (在描述、示例和来源中更新) |
| 5 | 中 | 更改描述 | 修复 `ANTHROPIC_CUSTOM_HEADERS` 格式描述从"JSON 字符串"改为"名称: 值格式，用换行符分隔" | ✅ 完成 (根据官方文档更新描述) |
| 6 | 中 | 未验证模式 | `askEdits` 和 `viewOnly` 权限模式不在官方文档中 — 仅记录 5 种模式 (default, acceptEdits, plan, dontAsk, bypassPermissions) | ✅ 完成 (标记为表中的"不在官方文档中 — 未验证") |
| 7 | 中 | 缺失环境变量 | 添加 `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS`, `CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY`, `CLAUDE_CODE_DISABLE_TERMINAL_TITLE`, `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL`, `CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS` | ✅ 完成 (添加 5 个环保变量加上 `CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS`) |
| 8 | 中 | 新设置 | 添加 `autoMemoryDirectory` 到核心配置 (字符串，自定义自动内存目录) — 版本不确定 (代理不同意: v2.1.68 vs v2.1.74)，不在设置页面 | ✅ 完成 (在 plansDirectory 附近添加 — 版本未解决) |
| 9 | 低 | 可疑键 | 验证 `skipWebFetchPreflight`, `sandbox.ignoreViolations`, `sandbox.network.deniedDomains`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 仍不在官方文档中 | ✋ 待处理 (保留在报告中等待验证 — 从 2026-03-05 开始重复) |
| 10 | 低 | 缺失环保变量 | 添加 `CLAUDE_CODE_SUBAGENT_MODEL` 到环保变量表 (已在模型环保变量示例块中，但在表中缺失) | ✅ 完成 (添加到环保变量表) |
| 11 | 低 | 示例更新 | 使用 `modelOverrides` 和更正的 `$schema` URL 更新快速参考示例 | ✅ 完成 (示例使用两者更新) |

---

## [2026-03-14 01:35 AM PKT] Claude Code v2.1.75

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 设置层次结构 | 重组为官方 5 级层次结构: 托管 (#1) > CLI 参数 > 本地 > 项目 > 用户。移除 `~/.claude/settings.local.json` 行。添加托管层内部优先级 (服务器托管 > MDM > 文件 > HKCU)。注意托管"不能被任何其他级别覆盖，包括 CLI 参数" | ✅ 完成 (重组表为 5 级，托管作为 #1，添加交付方法、内部优先级和文件路径) |
| 2 | 高 | 更改行为 | 修复 `availableModels` 描述：从复杂对象数组 (`title`/`modelId`/`effortOptions`) 改为简单字符串数组 `["sonnet", "haiku"]` 根据官方文档 | ✅ 完成 (更新描述以匹配官方文档格式) |
| 3 | 高 | 更改行为 | 添加 `cleanupPeriodDays` `0` 值行为："设置为 `0` 删除所有现有文本记录在启动时并完全禁用会话持久化" | ✅ 完成 (添加 0 值行为到描述) |
| 4 | 高 | 权限语法 | 添加评估顺序注释到权限部分："规则按顺序评估: 首先是拒绝规则，然后是询问，然后是允许。第一个匹配规则获胜。" | ✅ 完成 (在 Bash 通配符注释前添加评估顺序) |
| 5 | 中 | 更改描述 | 添加 `autoMemoryDirectory` 范围限制："不从项目设置 (`.claude/settings.json`) 接受。从政策、本地和用户设置接受" | ✅ 完成 (添加范围限制到描述) |
| 6 | 中 | 更改描述 | 添加 `permissions.defaultMode` 远程环境注释：仅 `acceptEdits` 和 `plan` 在远程环境中受尊重 (v2.1.70) | ✅ 完成 (添加远程限制到描述) |
| 7 | 中 | 模型配置 | 添加 Opus 4.6 1M 上下文默认注释：自 v2.1.75 起，1M 上下文是 Max/Team/Enterprise 计划的默认值 | ✅ 完成 (添加到工作强度级别注释) |
| 8 | 中 | 设置层次结构 | 添加 Windows 托管路径注释：v2.1.75 移除已弃用的 `C:\ProgramData\ClaudeCode\` 回退 — 使用 `C:\Program Files\ClaudeCode\managed-settings.json` | ✅ 完成 (在层次结构部分添加弃用注释) |
| 9 | 中 | 显示和 UX | 添加 `fileSuggestion` stdin JSON 格式 (`{"query": "..."}`) 和 15 路径输出限制详情 | ✅ 完成 (添加 stdin 格式和输出限制到文件建议部分) |
| 10 | 中 | 设置层次结构 | 将数组合并注释从"已合并"改为"连接和去重"根据官方文档 | ✅ 完成 (在层次结构重要部分更新措辞) |
| 11 | 低 | 可疑键 | `sandbox.ignoreViolations`, `sandbox.network.deniedDomains` 仍不在官方文档或 JSON schema 顶级中 | ✋ 待处理 (保留在报告中等待验证 — 从 2026-03-05 开始重复) |
| 12 | 低 | 可疑键 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 在 JSON schema 中确认但不在官方设置页面上 | ✋ 待处理 (保留在报告中 — 根据 schema 有效，从 2026-03-05 开始重复) |

---

## [2026-03-15 12:52 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新设置 | 添加 `effortLevel` 到常规设置或模型配置 — 跨会话保持工作强度级别 (`"low"`, `"medium"`, `"high"`)。在官方设置页面上确认 | ✋ 待处理 (等待用户批准) |
| 2 | 高 | 新设置 | 添加工作树设置部分，包含 `worktree.sparsePaths` (数组，稀疏检出圆锥模式) 和 `worktree.symlinkDirectories` (数组，符号链接目录以避免重复)。在官方设置页面上确认 | ✋ 待处理 (等待用户批准) |
| 3 | 高 | 新设置 | 添加 `feedbackSurveyRate` 到常规设置 — 概率 (0-1) 用于会话质量调查。在官方设置页面上确认 | ✋ 待处理 (等待用户批准) |
| 4 | 高 | 缺失环保变量 | 添加 20 个缺失的环保变量到表 (在官方 /en/env-vars 页面上确认) | ✋ 待处理 (等待用户批准) |
| 5 | 高 | 缺失环保变量 | 将 `ANTHROPIC_DEFAULT_OPUS_MODEL`, `ANTHROPIC_DEFAULT_SONNET_MODEL`, `MAX_THINKING_TOKENS` 从仅代码块移到常见环保变量表 | ✋ 待处理 (等待用户批准) |
| 6 | 高 | 损坏的链接 | 修复 `https://claudelog.com/configuration/` — 返回 ECONNREFUSED。移除或用工作来源替换 | ✋ 待处理 (等待用户批准) |
| 7 | 中 | 更改描述 | 更新 `cleanupPeriodDays` 描述以添加："钩子接收空的 `transcript_path`" 当设置为 0 时。根据官方文档 | ✋ 待处理 (等待用户批准) |
| 8 | 中 | 未验证环保变量 | 标记 7 个在报告中但不在官方文档中的环保变量为未验证 | ✋ 待处理 (等待用户批准) |
| 9 | 中 | 新来源 | 添加 `https://code.claude.com/docs/en/env-vars` 到来源部分 — 官方环保变量参考页面 | ✋ 待处理 (等待用户批准) |
| 10 | 中 | 示例更新 | 使用 `effortLevel` 和 `worktree` 设置更新快速参考示例 | ✋ 待处理 (等待用户批准) |
| 11 | 低 | 可疑键 | `sandbox.ignoreViolations`, `sandbox.network.deniedDomains` 仍不在官方文档沙箱表中 | ✋ 待处理 (保留在报告中等待验证 — 从 2026-03-05 开始重复) |
| 12 | 低 | 可疑键 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 仍在 JSON schema 中但不在官方设置页面上 | ✋ 待处理 (保留在报告中 — 根据 schema 有效，从 2026-03-05 开始重复) |

---

## [2026-03-15 01:10 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新设置 | 添加 `effortLevel` 到模型配置 — 跨会话保持工作强度级别 (`"low"`, `"medium"`, `"high"`)。也添加 `/effort` 命令到有用命令，并更新工作强度级别操作指南部分 | ✅ 完成 (添加到模型覆盖表，更新操作指南，添加 /effort 命令) |
| 2 | 高 | 新设置 | 添加工作树设置部分，包含 `worktree.sparsePaths` (数组，稀疏检出圆锥模式) 和 `worktree.symlinkDirectories` (数组，符号链接目录以避免重复) | ✅ 完成 (核心配置中的新工作树设置子部分，包含表和示例) |
| 3 | 高 | 新设置 | 添加 `feedbackSurveyRate` 到常规设置 — 概率 (0-1) 用于会话质量调查 | ✅ 完成 (添加到常规设置表) |
| 4 | 高 | 缺失环保变量 | 添加 23 个缺失的环保变量到表 (20 个真正新的 + 3 个从仅代码块) | ✅ 完成 (添加所有 23 个环保变量到常见环保变量表) |
| 5 | 高 | 损坏的链接 | 前一个 run 标记 `https://claudelog.com/configuration/` 为 ECONNREFUSED — 现在成功加载 | ✅ 完成 (链接已恢复，无需操作) |
| 6 | 中 | 权限语法 | 添加读取/编辑 gitignore 风格路径模式 (`//path`, `~/path`, `/path`, `./path`)、词界通配符详情和遗留 `:*` 弃用注释 | ✅ 完成 (添加路径模式表、词界注释和 `:*` 弃用) |
| 7 | 中 | 更改描述 | 更新 `cleanupPeriodDays` 以添加"钩子接收空的 `transcript_path`" 当设置为 0 时 | ✅ 完成 (添加到描述) |
| 8 | 中 | 未验证环保变量 | 标记不在官方文档中的 7 个环保变量为未验证 | ✅ 完成 (添加"不在官方文档中 — 未验证"标记) |
| 9 | 中 | 新来源 | 添加 `https://code.claude.com/docs/en/env-vars` 和 `https://code.claude.com/docs/en/permissions` 到来源部分 | ✅ 完成 (添加两个 URL) |
| 10 | 中 | 示例更新 | 使用 `effortLevel` 和 `worktree` 设置更新快速参考示例 | ✅ 完成 (添加 effortLevel 和 worktree 块到示例) |
| 11 | 低 | 可疑键 | `sandbox.ignoreViolations`, `sandbox.network.deniedDomains` 仍不在官方文档沙箱表中 | ✋ 待处理 (保留在报告中等待验证 — 从 2026-03-05 开始重复) |
| 12 | 低 | 可疑键 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 仍在 JSON schema 中但不在官方设置页面上 | ✋ 待处理 (保留在报告中 — 根据 schema 有效，从 2026-03-05 开始重复) |

---

## [2026-03-17 12:54 PM PKT] Claude Code v2.1.77

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新设置 | 添加 `sandbox.filesystem.allowRead` 到沙箱设置表 — 在 `denyRead` 区域内重新允许读取访问 (数组，默认 `[]`)。在 v2.1.77 变更日志中确认 | ✅ 完成 (添加到沙箱设置表，在 denyRead 行后) |
| 2 | 高 | 更改描述 | 更新 `CLAUDE_CODE_MAX_OUTPUT_TOKENS` 描述：Opus 4.6 的默认值增加到 64k，Opus 4.6 和 Sonnet 4.6 的上限增加到 128k (v2.1.77 变更日志) | ✅ 完成 (描述用模型特定的默认值和限制更新) |
| 3 | 高 | 缺失环保变量 | 添加 `CLAUDECODE` 到常见环保变量表 — 在生成的 shell 环境中设置为 `1`。在官方 /en/env-vars 页面上确认 | ✅ 完成 (添加到环保变量表) |
| 4 | 高 | 缺失环保变量 | 添加 `CLAUDE_CODE_SKIP_FAST_MODE_NETWORK_ERRORS` 到常见环保变量表 — 当组织状态检查失败时允许快速模式。在官方 /en/env-vars 页面上确认 | ✅ 完成 (添加到环保变量表) |
| 5 | 中 | 环保变量表 | 将 `ANTHROPIC_MODEL` 和 `ANTHROPIC_DEFAULT_HAIKU_MODEL` 从仅代码块移到常见环保变量表。两者都在官方 /en/env-vars 页面上确认 | ✅ 完成 (添加两者到环保变量表，靠近其他 ANTHROPIC_ 变量) |
| 6 | 中 | 可疑键升级 | `sandbox.network.deniedDomains` — 8 个连续的待处理 run (从 2026-03-05 开始)。不在官方文档页面或 JSON schema 中。根据规则 10B：标记为"不在官方文档中 — 未验证" | ✅ 完成 (添加未验证注释到描述) |
| 7 | 中 | 可疑键升级 | `allow_remote_sessions` — 不在官方文档页面或 JSON schema 中。标记为"不在官方文档中 — 未验证" | ✅ 完成 (添加未验证注释到描述) |
| 8 | 低 | 可疑键解决 | `sandbox.ignoreViolations` — 8 个连续的待处理 run。在 JSON schema 中确认。注释："在 JSON schema 中，不在官方设置页面上" | ✅ 完成 (添加 schema 注释到描述) |
| 9 | 低 | 可疑键解决 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 8 个连续的待处理 run。全部在 JSON schema 中确认。注释："在 JSON schema 中，不在官方设置页面上" | ✅ 完成 (为所有 4 个描述添加 schema 注释) |
| 10 | 低 | 标题计数 | 将标题环保变量计数从"160+"更新到"100+" — 实际表有 97 个环保变量 | ✅ 完成 (标题更新为"100+ 个环保变量"，版本改为 v2.1.77) |

---

## [2026-03-18 11:53 PM PKT] Claude Code v2.1.78

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 缺失设置 | 添加 `voiceEnabled` 到常规设置表 — 启用按键说话语音听写 (布尔值，由 `/voice` 编写，需要 Claude.ai 帐户)。在官方设置页面上确认 | ✅ 完成 (在 feedbackSurveyRate 前添加到常规设置表) |
| 2 | 高 | 缺失设置 | 添加 `filesystem.allowManagedReadPathsOnly` 到沙箱设置表 — 仅托管，仅托管 `allowRead` 路径被尊重 (布尔值，默认 false)。在官方设置页面上确认 | ✅ 完成 (在 enableWeakerNetworkIsolation 前添加到沙箱设置表) |
| 3 | 高 | 显示位置 | 将 `showTurnDuration` 和 `terminalProgressBarEnabled` 从显示设置表移到一个单独的"全局配置设置 (~/.claude.json)"子部分。官方文档声明："在 settings.json 中添加它们会触发 schema 验证错误" | ✅ 完成 (创建新子部分包含表；从 settings.json 显示设置表和示例中移除) |
| 4 | 高 | 更改默认值 | 修复 `MAX_MCP_OUTPUT_TOKENS` 默认值从 50000 改为 25000。官方 /en/env-vars 页面确认默认值: 25000 | ✅ 完成 (默认值已更新，添加警告阈值注释) |
| 5 | 高 | 缺失环保变量 | 添加 `CLAUDE_CODE_NEW_INIT`, `CLAUDE_CODE_PLUGIN_SEED_DIR`, `DISABLE_FEEDBACK_COMMAND` 到环保变量表。全部在官方 /en/env-vars 页面上确认 | ✅ 完成 (添加全部 3 个环保变量到表) |
| 6 | 中 | 验证修复 | 从 `allow_remote_sessions` 移除"未验证"注释 — 现在在官方权限页面上确认为仅托管设置。前一个 run (v2.1.77 #7) 错误地标记为未验证 | ✅ 完成 (移除"未验证"注释) |
| 7 | 中 | 环保变量重命名 | 更新 `DISABLE_BUG_COMMAND` 为 `DISABLE_FEEDBACK_COMMAND` — 官方文档说 `DISABLE_FEEDBACK_COMMAND` 是当前名称，`DISABLE_BUG_COMMAND` 是"旧名称" | ✅ 完成 (用别名注释重命名) |
| 8 | 中 | 更改描述 | 更新 `CLAUDE_CODE_EFFORT_LEVEL` 以包括 `max` (仅 Opus 4.6) 和 `auto` 值。官方 /en/env-vars 页面确认："值：low, medium, high, max (仅 Opus 4.6), 或 auto" | ✅ 完成 (描述用所有值和优先级注释更新) |
| 9 | 中 | 更改描述 | 修复 `CLAUDE_CODE_ENABLE_TASKS` 描述 — 官方："设置为 true 以在非交互模式 (-p 标记) 中启用任务跟踪。任务在交互模式中默认打开。" 报告当前说"设置为 false 以禁用" | ✅ 完成 (描述更正以匹配官方文档) |
| 10 | 中 | 更改描述 | 更新 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` 以注明："等效于设置 DISABLE_AUTOUPDATER, DISABLE_FEEDBACK_COMMAND, DISABLE_ERROR_REPORTING, 和 DISABLE_TELEMETRY" | ✅ 完成 (描述用等效变量列表更新) |
| 11 | 中 | 示例更新 | 从快速参考示例移除 `showTurnDuration` — 不属于 settings.json 根据官方文档 | ✅ 完成 (从快速参考和显示和 UX 示例中移除) |
| 12 | 低 | 环保变量默认值 | 验证 `MCP_TIMEOUT` 默认值 (报告说 10000) — 官方文档未指定默认值 | ✅ 完成 (移除未验证的默认值 — 官方文档省略了) |

---

## [2026-03-19 12:38 PM PKT] Claude Code v2.1.79

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 缺失环保变量 | 添加 `ANTHROPIC_CUSTOM_MODEL_OPTION`, `ANTHROPIC_CUSTOM_MODEL_OPTION_NAME`, `ANTHROPIC_CUSTOM_MODEL_OPTION_DESCRIPTION` 到常见环保变量表 — `/model` 选择器的模型配置变量以添加自定义条目。在官方 /en/env-vars 页面上确认 | ✅ 完成 (在表中的 ANTHROPIC_BASE_URL 后添加 3 个环保变量) |
| 2 | 高 | 更改描述 | 更新 `CLAUDE_CODE_PLUGIN_SEED_DIR` 从单数改为复数："一个或多个只读插件种子目录的路径，在 Unix 上由 `:` 或在 Windows 上由 `;` 分隔"。在 v2.1.79 变更日志中更改。在官方 /en/env-vars 页面上确认 | ✅ 完成 (描述用多目录支持更新) |
| 3 | 高 | 沙箱路径前缀 | 修复沙箱.filesystem 路径前缀文档：`/` = 绝对 (标准 Unix), `./` = 项目相对, `//` = 遗留仍有效。报告当前显示反向约定。官方文档明确注明："此语法不同于读取和编辑权限规则" | ✅ 完成 (用正确的前缀约定更新全部 4 个 sandbox.filesystem 条目，添加到读取/编辑权限规则的交叉引用注释，添加跨范围合并详情) |
| 4 | 中 | 更改描述 | 扩展 `CLAUDE_CODE_AUTO_COMPACT_WINDOW` 描述 — 当前"自动紧凑窗口行为配置"太最小。官方文档描述：令牌容量、默认值 (200K 标准 / 1M 扩展)、与 `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` 的交互、状态行解耦 | ✅ 完成 (用令牌容量、模型默认值、AUTOCOMPACT_PCT 交互和状态行解耦扩展描述) |

---

## [2026-03-20 08:41 AM PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新设置 | 添加 `channelsEnabled` 到 MCP 设置表 — 仅托管布尔值，控制团队和企业用户的频道消息交付。在官方设置页面上确认 | ✅ 完成 (添加到 MCP 设置表，在 allowManagedMcpServersOnly 后) |
| 2 | 中 | 版本徽章 | 将报告版本从 v2.1.79 更新到 v2.1.80 | ✅ 完成 (徽章和标题已更新) |

---

## [2026-03-21 09:17 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 缺失设置 (~/.claude.json) | 添加 `autoConnectIde` (布尔值，默认 `false`) 和 `autoInstallIdeExtension` (布尔值，默认 `true`) 到全局配置设置表。在官方设置页面的"全局配置设置"下确认 | ✅ 完成 (在 showTurnDuration 前将两个键添加到 ~/.claude.json 表) |
| 2 | 高 | 不正确的设置 | `allow_remote_sessions` 在权限键表中列为仅托管布尔值，但官方权限页面声明："远程控制和网络会话的访问不由托管设置键控制。" 标记为未验证或移除 | ✅ 完成 (重新添加未验证注释，包含官方文档引用和管理 UI 链接) |
| 3 | 中 | 版本凸起 | 将报告版本徽章从 v2.1.80 更新到 v2.1.81 | ✅ 完成 (徽章、标题版本和标题文本已更新) |
| 4 | 中 | 新设置 | 添加 `showClearContextOnPlanAccept` — 在 v2.1.81 变更日志中确认。当 `true` 时，在计划接受上恢复"清除上下文"选项 (默认隐藏)。不在官方设置页面上 — 可能是 `~/.claude.json` 键 | ✅ 完成 (用变更日志来源注释添加到全局配置设置表) |
| 5 | 中 | 插件文档 | 记录 `source: 'settings'` 作为市场来源类型（在插件设置部分）。官方设置页面列出它作为 `extraKnownMarketplaces` 的 7 种来源类型之一 | ✅ 完成 (添加所有 7 个来源类型列表，内联市场示例) |
| 6 | 中 | 状态行字段 | 将 `rate_limits` 字段组添加到状态行输入字段表 — 包括 `five_hour.used_percentage`, `five_hour.resets_at`, `seven_day.used_percentage`, `seven_day.resets_at`。在 v2.1.80 中添加 | ✅ 完成 (添加 4 个 rate_limits 字段到状态行输入字段表) |

---

## [2026-03-23 10:02 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 缺失设置 (~/.claude.json) | 添加 `editorMode` (字符串，默认 `"normal"`, 值: `"normal"` 或 `"vim"`) 到全局配置设置表。运行 `/vim` 时自动编写。在官方设置页面上确认 | ✅ 完成 (在 autoInstallIdeExtension 后添加到全局配置设置表) |
| 2 | 高 | 文件范围修复 | 将 `showClearContextOnPlanAccept` 从全局配置设置 (~/.claude.json) 移到常规设置 (settings.json)。官方文档现在在主可用设置表中列出它，不在全局配置表中。移除陈旧的注释"不在官方设置页面上" | ✅ 完成 (移到常规设置表，在 feedbackSurveyRate 前，移除陈旧注释) |
| 3 | 中 | 更改描述 | 修复 `terminalProgressBarEnabled` 支持的终端从"Windows Terminal, iTerm2"改为"ConEmu, Ghostty 1.2.0+, 和 iTerm2 3.6.6+" 根据官方文档 | ✅ 完成 (终端列表已更新) |
| 4 | 中 | 更改描述 | 将"配置工具"添加到 `availableModels` 描述 — 官方文档说"通过 `/model`, `--model`, 配置工具, 或 `ANTHROPIC_MODEL`"。报告当前省略了"配置工具" | ✅ 完成 (添加"配置工具"到描述) |

---

## [2026-03-25 08:16 PM PKT] Claude Code v2.1.83

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新设置 | 添加 `autoMode` 到权限部分 — 具有 `environment`, `allow`, `soft_deny` 数组的对象，用于配置自动模式分类器。不从共享项目设置 (`.claude/settings.json`) 读取。在用户、本地和托管设置中可用。在官方设置 + 权限页面上确认 | ✅ 完成 (添加到权限键表，包含完整描述、范围限制和 `claude auto-mode defaults` 注释) |
| 2 | 高 | 新设置 | 添加 `disableAutoMode` 到权限部分 — 字符串，设置为 `"disable"` 以防止自动模式激活。从 Shift+Tab 周期中移除 `auto`。可在任何设置级别设置，在托管设置中最有用。在官方设置 + 权限页面上确认 | ✅ 完成 (添加到权限键表，在 `autoMode` 后) |
| 3 | 高 | 新权限模式 | 向权限模式表添加 `auto` — 后台分类器替代手动提示。研究预览。需要团队计划 + Sonnet/Opus 4.6。在官方权限模式页面上确认 | ✅ 完成 (添加到权限模式表，包含分类器详情和回退行为) |
| 4 | 高 | 新设置 | 添加 `sandbox.failIfUnavailable` 到沙箱设置表 — 布尔值，默认 `false`，沙箱启用但无法启动时以错误退出，而不是无沙箱运行。在 v2.1.83 变更日志中确认 | ✅ 完成 (添加到沙箱设置表，在 `sandbox.enabled` 后) |
| 5 | 高 | 新设置 | 添加 `disableDeepLinkRegistration` 到常规设置表 — 布尔值，防止 `claude-cli://` 协议处理程序注册。在 v2.1.83 变更日志中确认 | ✅ 完成 (在 `feedbackSurveyRate` 前添加到常规设置表) |
| 6 | 高 | 缺失环保变量 | 添加 `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` 到常见环保变量表 — 设置为 `1` 以从子流程环境中去除 Anthropic 和云提供商凭证 (Bash 工具、钩子、MCP stdio 服务器)。在 v2.1.83 变更日志中确认 | ✅ 完成 (在 `CLAUDE_CODE_SUBAGENT_MODEL` 后添加到环保变量表) |
| 7 | 高 | 设置层次结构 | 添加 `managed-settings.d/` drop-in 目录到托管设置部分 — 与 `managed-settings.json` 并行的独立政策片段按字母顺序合并。在 v2.1.83 变更日志中确认 | ✅ 完成 (作为托管设置交付方法下的项目符号添加) |
| 8 | 高 | 损坏的链接 | 修复来源中的 `https://claudelog.com/configuration/` — 返回 403 禁止。用工作来源替换或移除 | ✅ 完成 (用 `https://claudelog.com/claude-code-changelog/` 替换，已验证工作) |
| 9 | 中 | 版本徽章 | 将报告版本从 v2.1.81 更新到 v2.1.83 | ✅ 完成 (在第 2.6 阶段更新徽章和标题) |
| 10 | 中 | 示例更新 | 添加 `autoMode` 到快速参考示例以演示自动模式分类器配置 | ✅ 完成 (在 `permissions` 块前添加 `autoMode` 块) |
| 11 | 中 | 更改路径 | 修复 Windows 注册表路径从 `Software\Anthropic\ClaudeCode` 改为 `SOFTWARE\Policies\ClaudeCode` (HKLM 和 HKCU)。官方文档更新以使用 `Policies` 子键 | ✅ 完成 (更新为 `HKLM\SOFTWARE\Policies\ClaudeCode` 和 `HKCU\SOFTWARE\Policies\ClaudeCode`，带优先级注释) |
| 12 | 低 | 缺失别名 | 添加 `opus[1m]` 到模型别名表 — Opus 4.6 具有 1M 上下文，自 v2.1.75 以来在 Max/Team/Enterprise 上默认可用 | ✅ 完成 (在 `sonnet[1m]` 后添加到模型别名表) |

---

## [2026-03-26 01:04 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新设置 | 添加 `defaultShell` 到常规设置 — 字符串，默认 `"bash"`, 接受 `"bash"` 或 `"powershell"`。在 Windows 上通过 PowerShell 路由交互式 `!` 命令。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`。在官方设置页面上确认 | ✅ 完成 (在 jammateMode 后添加到常规设置表) |
| 2 | 高 | 新设置 | 添加 `allowedChannelPlugins` 到 MCP 设置 — 数组，仅托管。可能推送消息的频道插件的允许列表。当设置时替换默认 Anthropic 允许列表。需要 `channelsEnabled: true`。在官方设置页面上确认 | ✅ 完成 (在 channelsEnabled 后添加到 MCP 设置表) |
| 3 | 高 | 新设置 | 添加 `useAutoModeDuringPlan` 到权限键 — 布尔值，默认 `true`。计划模式在自动模式可用时使用自动模式语义。不从共享项目设置读取。在官方设置页面上确认 | ✅ 完成 (在 disableAutoMode 后添加到权限键表) |
| 4 | 高 | 缺失环保变量 | 添加 9 个模型自定义环保变量：`ANTHROPIC_DEFAULT_{OPUS,SONNET,HAIKU}_MODEL_{NAME,DESCRIPTION,SUPPORTED_CAPABILITIES}` 对于 `/model` 选择器自定义在 Bedrock/Vertex/Foundry 上。在官方 /en/env-vars 页面上确认 | ✅ 完成 (为每个基础模型变量后添加 3 个变量：Haiku、Opus、Sonnet) |
| 5 | 高 | 缺失环保变量 | 添加 `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` — 流失败时禁用非流回退。防止通过代理的重复工具执行。在官方 /en/env-vars 页面上确认 (在 v2.1.83 中添加，在前一个 run 中遗漏) | ✅ 完成 (在 CLAUDE_CODE_DISABLE_FAST_MODE 后添加) |
| 6 | 高 | 缺失环保变量 | 添加 `CLAUDE_CODE_USE_POWERSHELL_TOOL` — 在 Windows 上启用 PowerShell 工具 (选择进预览)。仅原生 Windows，不是 WSL。在官方 /en/env-vars 页面上确认 | ✅ 完成 (在 CLAUDE_CODE_USE_FOUNDRY 后添加) |
| 7 | 高 | 损坏的链接 | 修复来源中的 `https://claudelog.com/claude-code-changelog/` — 返回 403 禁止。用官方 GitHub 变更日志 URL 替换 | ✅ 完成 (用 github.com/anthropics/claude-code/blob/main/CHANGELOG.md 替换) |
| 8 | 中 | 设置层次结构 | 更新托管层优先级："基于文件 (`managed-settings.d/*.json` + `managed-settings.json`)"并添加"跨层"限定符。根据官方文档添加层内合并注释 | ✅ 完成 (用基于文件的层和跨层限定符更新优先级描述) |
| 9 | 中 | 设置层次结构 | 扩展 drop-in 目录合并语义：systemd 约定、标量覆盖、数组连接+去重、深度合并、隐藏文件排除、数值前缀提示。根据官方设置页面 | ✅ 完成 (用完整的 systemd 约定详情和数值前缀提示扩展) |
| 10 | 中 | 注释 | 添加"在变更日志中，不在官方设置页面上"注释到 `disableDeepLinkRegistration` 根据规则 1F 反向完整性检查 | ✅ 完成 (添加注释到描述) |
| 11 | 中 | 示例更新 | 添加 `defaultShell` 到快速参考示例以演示 PowerShell 配置 | ✅ 完成 (添加"defaultShell": "bash" 到示例) |

---

## [2026-03-27 06:32 PM PKT] Claude Code v2.1.85

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 缺失环保变量 | 添加 `CLAUDE_STREAM_IDLE_TIMEOUT_MS` 到常见环保变量表 — 流闲置监察狗关闭停滞连接前的超时毫秒数 (默认: 90000)。在官方 /en/env-vars 页面上确认。在 v2.1.84 中添加但在前一个 run 中遗漏 | ✅ 完成 (在 CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS 后添加到环保变量表) |
| 2 | 高 | 版本凸起 | 将报告版本徽章从 v2.1.84 更新到 v2.1.85 | ✅ 完成 (在第 2.6 阶段更新徽章、标题版本和标题文本) |
| 3 | 中 | 新环保变量 | 添加 `OTEL_LOG_TOOL_DETAILS` 到环保变量表 — 在 OpenTelemetry 事件中控制 `tool_parameters`。v2.1.85 变更日志仅有 (尚不在官方 env-vars 页面)。用变更日志来源注释添加 | ✅ 完成 (用"在 v2.1.85 变更日志中，尚不在官方 env-vars 页面上"注释添加) |
| 4 | 中 | 新环保变量 (所有权) | 决定 `CLAUDE_CODE_MCP_SERVER_NAME` 和 `CLAUDE_CODE_MCP_SERVER_URL` 的所有权 — 传递给 MCP `headersHelper` 脚本的环保变量 (v2.1.85 变更日志)。可能属于钩子仓库而不是设置报告 | ✅ 完成 (添加到设置报告，带变更日志注释 — 这些是通过 `env` 键环保可配置的，不仅限钩子) |

---

## [2026-03-28 06:10 PM PKT] Claude Code v2.1.86

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 文件范围 | 将 `jammateMode` 从常规设置 (settings.json) 移到全局配置设置 (~/.claude.json)。官方设置页面在"全局配置设置"下列出它 — 添加到 settings.json 会触发 schema 验证错误 (规则 1H)。与 v2.1.78 `showTurnDuration` 修复相同模式 | ✅ 完成 (从常规设置表移除，添加到全局配置设置表，在 terminalProgressBarEnabled 后，带代理团队文档链接) |
| 2 | 高 | 类型 + 注释 | 修复 `disableDeepLinkRegistration`：将类型从 `boolean` 改为 `string` (值: `"disable"`), 更新描述以匹配官方文档，移除陈旧的"(在变更日志中，不在官方设置页面上)"注释。现在在官方设置页面上确认 (第 169 行) | ✅ 完成 (类型改为字符串，描述更新以匹配官方文档，移除变更日志注释) |
| 3 | 高 | 版本凸起 | 将报告版本徽章从 v2.1.85 更新到 v2.1.86 | ✅ 完成 (在第 2.6 阶段更新徽章和标题) |
