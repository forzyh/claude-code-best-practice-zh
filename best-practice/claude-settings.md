# Claude Code 设置参考

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2028%2C%202026%206%3A10%20PM%20PKT-white?style=flat&labelColor=555) ![Version](https://img.shields.io/badge/Claude_Code-v2.1.86-blue?style=flat&labelColor=555)

Claude Code 的 `settings.json` 文件中所有可用配置选项的综合指南。至 v2.1.86 版本，Claude Code 公开了 **60+ 个设置**和 **100+ 个环境变量**（使用 `settings.json` 中的 `"env"` 字段以避免包装脚本）。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [设置层次](#设置层次)
2. [核心配置](#核心配置)
3. [权限](#权限)
4. [钩子](#钩子)
5. [MCP 服务器](#mcp-服务器)
6. [沙箱](#沙箱)
7. [插件](#插件)
8. [模型配置](#模型配置)
9. [显示和用户体验](#显示和用户体验)
10. [AWS 和云凭证](#aws-和云凭证)
11. [环境变量](#通过-env-的环境变量)
12. [有用的命令](#有用的命令)

---

## 设置层次

设置按优先级顺序应用（从高到低）：

| 优先级 | 位置 | 范围 | 共享? | 目的 |
|----------|----------|-------|---------|---------|
| 1 | 托管设置 | 组织 | 是（由 IT 部署） | 无法覆盖的安全策略 |
| 2 | 命令行参数 | 会话 | N/A | 临时单会话覆盖 |
| 3 | `.claude/settings.local.json` | 项目 | 否（git-ignored） | 个人项目特定 |
| 4 | `.claude/settings.json` | 项目 | 是（已提交） | 团队共享设置 |
| 5 | `~/.claude/settings.json` | 用户 | N/A | 全局个人默认值 |

**托管设置**由组织强制，无法通过任何其他级别（包括命令行参数）覆盖。交付方法：
- **服务器管理**设置（远程交付）
- **MDM 配置文件** — macOS plist，位置为 `com.anthropic.claudecode`
- **注册表策略** — Windows `HKLM\SOFTWARE\Policies\ClaudeCode`（管理员）和 `HKCU\SOFTWARE\Policies\ClaudeCode`（用户级别，最低策略优先级）
- **文件** — `managed-settings.json`（macOS：`/Library/Application Support/ClaudeCode/`，Linux/WSL：`/etc/claude-code/`，Windows：`C:\Program Files\ClaudeCode\`）
- **放置目录** — `managed-settings.d/`，与 `managed-settings.json` 同位置（v2.1.83）。遵循 systemd 约定，`managed-settings.json` 首先作为基础合并，然后所有 `managed-settings.d/` 中的 `*.json` 文件按字母顺序排序并合并。对于标量值，后面的文件覆盖前面的；数组被连接并去重；对象被深度合并。忽略以 `.` 开头的隐藏文件。使用数字前缀控制合并顺序（例如 `10-telemetry.json`、`20-security.json`）

在托管层中，优先级为：服务器管理 > MDM/OS 级别策略 > 基于文件（`managed-settings.d/*.json` + `managed-settings.json`）> HKCU 注册表（仅限 Windows）。仅使用一个托管来源；来源不跨层合并。在基于文件的层中，放置文件和基础文件被合并在一起。

> **注意：** 至 v2.1.75 版本，已弃用的 Windows 备用路径 `C:\ProgramData\ClaudeCode\managed-settings.json` 已删除。改用 `C:\Program Files\ClaudeCode\managed-settings.json`。

**重要：**
- `deny` 规则具有最高安全优先级，无法被较低优先级的 allow/ask 规则覆盖。
- 托管设置可能会锁定或覆盖本地行为，即使本地文件指定不同的值。
- 数组设置（例如 `permissions.allow`）跨范围**连接和去重** — 来自所有级别的条目被合并，而不是替换。

---

## 核心配置

### 常规设置

| 键 | 类型 | 默认 | 说明 |
|-----|------|---------|-------------|
| `$schema` | string | - | IDE 验证和自动完成的 JSON 架构 URL（例如 `"https://json.schemastore.org/claude-code-settings.json"`） |
| `model` | string | `"default"` | 覆盖默认模型。接受别名（`sonnet`、`opus`、`haiku`）或完整模型 ID |
| `agent` | string | - | 为主对话设置默认代理。值是来自 `.claude/agents/` 的代理名称。也可通过 `--agent` CLI 标志获得 |
| `language` | string | `"english"` | Claude 的首选响应语言 |
| `cleanupPeriodDays` | number | `30` | 比此值不活跃的会话在启动时被删除。设置为 `0` 删除所有现有记录并完全禁用会话持久化（不写 `.jsonl` 文件，`/resume` 显示无会话，钩子接收空 `transcript_path`） |
| `autoUpdatesChannel` | string | `"latest"` | 发行渠道：`"stable"` 或 `"latest"` |
| `alwaysThinkingEnabled` | boolean | `false` | 默认为所有会话启用扩展思考 |
| `skipWebFetchPreflight` | boolean | `false` | 跳过 WebFetch 前置检查（在 JSON 架构中，不在官方设置页面上） |
| `availableModels` | array | - | 限制用户通过 `/model`、`--model`、Config 工具或 `ANTHROPIC_MODEL` 可以选择的模型。不影响"默认"选项。示例：`["sonnet", "haiku"]` |
| `fastModePerSessionOptIn` | boolean | `false` | 要求用户每个会话选择加入快速模式 |
| `defaultShell` | string | `"bash"` | 输入框 `!` 命令的默认 shell。接受 `"bash"`（默认）或 `"powershell"`。设置 `"powershell"` 在 Windows 上通过 PowerShell 路由交互 `!` 命令。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`（v2.1.84） |
| `includeGitInstructions` | boolean | `true` | 在系统提示中包括 git 相关指令 |
| `voiceEnabled` | boolean | - | 启用按住即说语音输入。运行 `/voice` 时自动写入。需要 Claude.ai 账户 |
| `showClearContextOnPlanAccept` | boolean | `false` | 在计划接受屏幕上显示"清空上下文"选项。设置为 `true` 以恢复该选项（自 v2.1.81 起默认隐藏） |
| `disableDeepLinkRegistration` | string | - | 设置为 `"disable"` 以防止 Claude Code 在启动时使用操作系统注册 `claude-cli://` 协议处理程序。深层链接允许外部工具通过 `claude-cli://open?q=...` 打开预填充提示的 Claude Code 会话。在限制或单独管理协议处理程序注册的环境中很有用 |
| `feedbackSurveyRate` | number | - | 会话质量调查出现时的概率（0-1）。企业管理员可以控制显示频率。示例：`0.05` = 符合条件的会话的 5% |

**示例：**
```json
{
  "model": "opus",
  "agent": "code-reviewer",
  "language": "japanese",
  "cleanupPeriodDays": 60,
  "autoUpdatesChannel": "stable",
  "alwaysThinkingEnabled": true
}
```

### 计划和内存目录

在自定义位置存储计划和自动内存文件。

| 键 | 类型 | 默认 | 说明 |
|-----|------|---------|-------------|
| `plansDirectory` | string | `~/.claude/plans` | 存储 `/plan` 输出的目录 |
| `autoMemoryDirectory` | string | - | 自动内存存储的自定义目录。接受 `~/` 扩展路径。不接受项目设置（`.claude/settings.json`）以防止将内存写入敏感位置；接受策略、本地和用户设置 |

**示例：**
```json
{
  "plansDirectory": "./my-plans"
}
```

**用例：** 用于将计划工件组织到与 Claude 内部文件分开的位置，或将计划保存在共享团队位置。

### Worktree 设置

配置 `--worktree` 如何创建和管理 git worktree。对于减少大型 monorepo 中的磁盘使用量和启动时间很有用。

| 键 | 类型 | 默认 | 说明 |
|-----|------|---------|-------------|
| `worktree.symlinkDirectories` | array | `[]` | 要从主仓库符号链接到每个 worktree 的目录，以避免在磁盘上重复大型目录 |
| `worktree.sparsePaths` | array | `[]` | 通过 git sparse-checkout（cone 模式）在每个 worktree 中检出的目录。仅将列出的路径写入磁盘 |

**示例：**
```json
{
  "worktree": {
    "symlinkDirectories": ["node_modules", ".cache"],
    "sparsePaths": ["packages/my-app", "shared/utils"]
  }
}
```

### 归属设置

自定义 git 提交和拉取请求的归属消息。

| 键 | 类型 | 默认 | 说明 |
|-----|------|---------|-------------|
| `attribution.commit` | string | Co-authored-by | Git 提交归属（支持尾部） |
| `attribution.pr` | string | 生成消息 | 拉取请求描述归属 |
| `includeCoAuthoredBy` | boolean | `true` | **已弃用** - 改用 `attribution` |

**示例：**
```json
{
  "attribution": {
    "commit": "Generated with AI\n\nCo-Authored-By: Claude <noreply@anthropic.com>",
    "pr": "Generated with Claude Code"
  }
}
```

**注意：** 设置为空字符串（`""`）以完全隐藏归属。

### 身份验证帮助程序

用于动态身份验证令牌生成的脚本。

| 键 | 类型 | 说明 |
|-----|------|-------------|
| `apiKeyHelper` | string | 输出身份验证令牌的 Shell 脚本路径（作为 `X-Api-Key` 头发送） |
| `forceLoginMethod` | string | 将登录限制为 `"claudeai"` 或 `"console"` 账户 |
| `forceLoginOrgUUID` | string | UUID 在登录期间自动选择组织 |

**示例：**
```json
{
  "apiKeyHelper": "/bin/generate_temp_api_key.sh",
  "forceLoginMethod": "console",
  "forceLoginOrgUUID": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

### 公司公告

在启动时向用户显示自定义公告（随机轮播）。

| 键 | 类型 | 说明 |
|-----|------|-------------|
| `companyAnnouncements` | array | 启动时显示的字符串数组 |

**示例：**
```json
{
  "companyAnnouncements": [
    "Welcome to Acme Corp!",
    "Remember to run tests before committing!",
    "Check the wiki for coding standards"
  ]
}
```

---

## 权限

控制 Claude 可以执行的工具和操作。

### 权限结构

```json
{
  "permissions": {
    "allow": [],
    "ask": [],
    "deny": [],
    "additionalDirectories": [],
    "defaultMode": "acceptEdits",
    "disableBypassPermissionsMode": "disable"
  }
}
```

### 权限键

| 键 | 类型 | 说明 |
|-----|------|-------------|
| `permissions.allow` | array | 允许工具使用而无需提示的规则 |
| `permissions.ask` | array | 需要用户确认的规则 |
| `permissions.deny` | array | 阻止工具使用的规则（最高优先级） |
| `permissions.additionalDirectories` | array | Claude 可以访问的额外目录 |
| `permissions.defaultMode` | string | 默认权限模式。在远程环境中，仅 `acceptEdits` 和 `plan` 被采用（v2.1.70+） |
| `permissions.disableBypassPermissionsMode` | string | 防止绕过模式激活 |
| `allowManagedPermissionRulesOnly` | boolean | **（仅限托管）** 仅托管权限规则适用；用户/项目 `allow`、`ask`、`deny` 规则被忽略 |
| `allow_remote_sessions` | boolean | **（仅限托管）** 允许用户启动远程控制和网页会话。默认为 `true`。设置为 `false` 以防止远程会话访问 |
| `autoMode` | object | 自定义[自动模式](/en/permission-modes#eliminate-prompts-with-auto-mode)分类器阻止和允许的内容。包含 `environment`（可信基础设施说明）、`allow`（块规则的例外）和 `soft_deny`（块规则）— 所有数组都是散文字符串。**不从共享项目设置读取**（`.claude/settings.json`）以防止仓库注入。可在用户、本地和托管设置中获得。设置 `allow` 或 `soft_deny` **替换**该部分的整个默认列表。运行 `claude auto-mode defaults` 以查看自定义前的内置规则 |
| `disableAutoMode` | string | 设置为 `"disable"` 以防止[自动模式](/en/permission-modes#eliminate-prompts-with-auto-mode)被激活。从 `Shift+Tab` 循环中删除 `auto` 并在启动时拒绝 `--permission-mode auto`。可在任何设置级别设置；在托管设置中最有用，用户无法覆盖 |
| `useAutoModeDuringPlan` | boolean | 计划模式在自动模式可用时是否使用自动模式语义。默认：`true`。不从共享项目设置读取（`.claude/settings.json`）。显示在 `/config` 中作为"在计划期间使用自动模式" |

### 权限模式

| 模式 | 行为 |
|------|----------|
| `"default"` | 标准权限检查和提示 |
| `"acceptEdits"` | 自动接受文件编辑而无需询问 |
| `"askEdits"` | 在每个操作前询问 |
| `"dontAsk"` | 自动拒绝工具，除非通过 `/permissions` 或 `permissions.allow` 规则预批准 |
| `"viewOnly"` | 只读模式，无修改 |
| `"bypassPermissions"` | 跳过所有权限检查（危险） |
| `"auto"` | 后台分类器替代手动提示。研究预览 — 需要 Team 计划 + Sonnet/Opus 4.6。分类器自动批准只读和文件编辑；将其他所有内容通过安全检查。在 3 个连续或 20 个总块后回退到提示。使用 `autoMode` 设置配置 |
| `"plan"` | 只读探索模式 |

### 工具权限语法

| 工具 | 语法 | 示例 |
|------|--------|----------|
| `Bash` | `Bash(command pattern)` | `Bash(npm run *)`、`Bash(* install)`、`Bash(git * main)` |
| `Read` | `Read(path pattern)` | `Read(.env)`、`Read(./secrets/**)` |
| `Edit` | `Edit(path pattern)` | `Edit(src/**)`、`Edit(*.ts)` |
| `Write` | `Write(path pattern)` | `Write(*.md)`、`Write(./docs/**)` |
| `NotebookEdit` | `NotebookEdit(pattern)` | `NotebookEdit(*)` |
| `WebFetch` | `WebFetch(domain:pattern)` | `WebFetch(domain:example.com)` |
| `WebSearch` | `WebSearch` | 全局网页搜索 |
| `Task` | `Task(agent-name)` | `Task(Explore)`、`Task(my-agent)` |
| `Agent` | `Agent(name)` | `Agent(researcher)`、`Agent(*)` — 权限范围限于子代理生成 |
| `Skill` | `Skill(skill-name)` | `Skill(weather-fetcher)` |
| `MCP` | `mcp__server__tool` or `MCP(server:tool)` | `mcp__memory__*`、`MCP(github:*)` |

**评估顺序：** 规则按顺序评估：先 deny 规则，然后 ask，然后 allow。第一条匹配规则获胜。

**读/编辑路径模式：** `Read`、`Edit` 和 `Write` 的权限规则支持 gitignore 风格的模式，具有四个前缀类型：

| 前缀 | 含义 | 示例 |
|--------|---------|---------|
| `//` | 文件系统根目录的绝对路径 | `Read(//Users/alice/file)` |
| `~/` | 相对于主目录 | `Read(~/.zshrc)` |
| `/` | 相对于项目根目录 | `Edit(/src/**)` |
| `./` 或无 | 相对路径（当前目录） | `Read(.env)`、`Read(*.ts)` |

**Bash 通配符注意：**
- `*` 可以在**任何位置**出现：前缀（`Bash(* install)`）、后缀（`Bash(npm *)`）或中间（`Bash(git * main)`）
- **词边界：** `Bash(ls *)`（空格在 `*` 前）匹配 `ls -la` 但不匹配 `lsof`；`Bash(ls*)`（无空格）两个都匹配
- `Bash(*)` 被视为等同于 `Bash`（匹配所有 bash 命令）
- 权限规则支持输出重定向：`Bash(python:*)` 匹配 `python script.py > output.txt`
- 旧的 `:*` 后缀语法（例如 `Bash(npm:*)`）等同于 ` *` 但已弃用

**示例：**
```json
{
  "permissions": {
    "allow": [
      "Edit(*)",
      "Write(*)",
      "Bash(npm run *)",
      "Bash(git *)",
      "WebFetch(domain:*)",
      "mcp__*"
    ],
    "ask": [
      "Bash(rm *)",
      "Bash(git push *)"
    ],
    "deny": [
      "Read(.env)",
      "Read(./secrets/**)",
      "Bash(curl *)"
    ],
    "additionalDirectories": ["../shared-libs/"]
  }
}
```

---

## 钩子

钩子配置（事件、属性、匹配器、退出代码、环境变量和 HTTP 钩子）在专门的仓库中维护：

> **[claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks)** — 完整的钩子参考，包括声音通知系统、所有 19 个钩子事件、HTTP 钩子、匹配器模式、退出代码和环境变量。

钩子相关设置键（`hooks`、`disableAllHooks`、`allowManagedHooksOnly`、`allowedHttpHookUrls`、`httpHookAllowedEnvVars`）在那里有文档记录。

关于官方钩子参考，见[Claude Code 钩子文档](https://code.claude.com/docs/en/hooks)。

---

## MCP 服务器

配置模型上下文协议服务器以获得扩展功能。

### MCP 设置

| 键 | 类型 | 范围 | 说明 |
|-----|------|-------|-------------|
| `enableAllProjectMcpServers` | boolean | 任何 | 自动批准所有 `.mcp.json` 服务器 |
| `enabledMcpjsonServers` | array | 任何 | 允许列表特定服务器名称 |
| `disabledMcpjsonServers` | array | 任何 | 阻止列表特定服务器名称 |
| `allowedMcpServers` | array | 仅托管 | 使用名称/命令/URL 匹配的允许列表 |
| `deniedMcpServers` | array | 仅托管 | 使用匹配的阻止列表 |
| `allowManagedMcpServersOnly` | boolean | 仅托管 | 仅允许在托管允许列表中明确列出的 MCP 服务器 |
| `channelsEnabled` | boolean | 仅托管 | 允许 Team 和 Enterprise 用户的[频道](https://code.claude.com/docs/en/channels)。未设置或 `false` 时，无论 `--channels` 标志如何，频道消息传递都被阻止 |
| `allowedChannelPlugins` | array | 仅托管 | 可能推送消息的频道插件的允许列表。设置时替换默认 Anthropic 允许列表。未定义 = 回退到默认值，空数组 = 阻止所有频道插件。需要 `channelsEnabled: true`。每个条目是具有 `marketplace` 和 `plugin` 字段的对象（v2.1.84） |

### MCP 服务器匹配（托管设置）

```json
{
  "allowedMcpServers": [
    { "serverName": "github" },
    { "serverCommand": "npx @modelcontextprotocol/*" },
    { "serverUrl": "https://mcp.company.com/*" }
  ],
  "deniedMcpServers": [
    { "serverName": "dangerous-server" }
  ]
}
```

**示例：**
```json
{
  "enableAllProjectMcpServers": true,
  "enabledMcpjsonServers": ["memory", "github", "filesystem"],
  "disabledMcpjsonServers": ["experimental-server"]
}
```

---

## 沙箱

为安全性配置 bash 命令沙箱。

### 沙箱设置

| 键 | 类型 | 默认 | 说明 |
|-----|------|---------|-------------|
| `sandbox.enabled` | boolean | `false` | 启用 bash 沙箱 |
| `sandbox.failIfUnavailable` | boolean | `false` | 当沙箱已启用但无法启动时以错误退出，而不是运行非沙箱化。对于需要严格沙箱的企业策略很有用（v2.1.83） |
| `sandbox.autoAllowBashIfSandboxed` | boolean | `true` | 沙箱化时自动批准 bash |
| `sandbox.excludedCommands` | array | `[]` | 在沙箱外运行的命令 |
| `sandbox.allowUnsandboxedCommands` | boolean | `true` | 允许 `dangerouslyDisableSandbox` |
| `sandbox.ignoreViolations` | object | `{}` | 命令模式到路径数组的映射 — 抑制违规警告 |
| `sandbox.enableWeakerNestedSandbox` | boolean | `false` | 为 Docker 提供较弱的沙箱（降低安全性） |
| `sandbox.network.allowUnixSockets` | array | `[]` | 沙箱中可访问的特定 Unix 套接字路径 |
| `sandbox.network.allowAllUnixSockets` | boolean | `false` | 允许所有 Unix 套接字（覆盖 allowUnixSockets） |
| `sandbox.network.allowLocalBinding` | boolean | `false` | 允许绑定到 localhost 端口（macOS） |
| `sandbox.network.allowedDomains` | array | `[]` | 沙箱的网络域允许列表 |
| `sandbox.network.deniedDomains` | array | `[]` | 沙箱的网络域阻止列表 |
| `sandbox.network.httpProxyPort` | number | - | HTTP 代理端口 1-65535（自定义代理） |
| `sandbox.network.socksProxyPort` | number | - | SOCKS5 代理端口 1-65535（自定义代理） |
| `sandbox.network.allowManagedDomainsOnly` | boolean | `false` | 仅允许托管允许列表中的域（托管设置） |
| `sandbox.filesystem.allowWrite` | array | `[]` | 沙箱化命令可以写入的其他路径。数组跨所有设置范围合并。前缀：`/`（绝对）、`~/`（主目录）、`./` 或无（项目设置中的项目相对，用户设置中的 `~/.claude` 相对）。旧的 `//` 前缀用于绝对路径仍然有效。**注意：** 这不同于[读/编辑权限规则](#工具权限语法)，后者使用 `//` 表示绝对，`/` 表示项目相对 |
| `sandbox.filesystem.denyWrite` | array | `[]` | 沙箱化命令无法写入的路径。数组跨所有设置范围合并。与 `allowWrite` 相同的路径前缀约定 |
| `sandbox.filesystem.denyRead` | array | `[]` | 沙箱化命令无法读取的路径。数组跨所有设置范围合并。与 `allowWrite` 相同的路径前缀约定 |
| `sandbox.filesystem.allowRead` | array | `[]` | 在 `denyRead` 区域内重新允许读取访问的路径。优先于 `denyRead`。数组跨所有设置范围合并。与 `allowWrite` 相同的路径前缀约定 |
| `sandbox.filesystem.allowManagedReadPathsOnly` | boolean | `false` | **（仅托管）** 仅来自托管设置的 `allowRead` 路径被尊重。来自用户、项目和本地设置的 `allowRead` 条目被忽略 |
| `sandbox.enableWeakerNetworkIsolation` | boolean | `false` | （仅 macOS）允许访问系统 TLS 信任（`com.apple.trustd.agent`）；降低安全性 |

**示例：**
```json
{
  "sandbox": {
    "enabled": true,
    "autoAllowBashIfSandboxed": true,
    "excludedCommands": ["git", "docker", "gh"],
    "allowUnsandboxedCommands": false,
    "network": {
      "allowUnixSockets": ["/var/run/docker.sock"],
      "allowLocalBinding": true
    }
  }
}
```

---

## 插件

配置 Claude Code 插件和市场。

### 插件设置

| 键 | 类型 | 范围 | 说明 |
|-----|------|-------|-------------|
| `enabledPlugins` | object | 任何 | 启用/禁用特定插件 |
| `extraKnownMarketplaces` | object | 项目 | 添加自定义插件市场（通过 `.claude/settings.json` 进行团队共享） |
| `strictKnownMarketplaces` | array | 仅托管 | 允许的市场的允许列表 |
| `skippedMarketplaces` | array | 任何 | 用户拒绝安装的市场 |
| `skippedPlugins` | array | 任何 | 用户拒绝安装的插件 |
| `pluginConfigs` | object | 任何 | 按插件 MCP 服务器配置（由 `plugin@marketplace` 键入） |
| `blockedMarketplaces` | array | 仅托管 | 阻止特定插件市场 |
| `pluginTrustMessage` | string | 仅托管 | 提示用户信任插件时显示的自定义消息 |

**市场来源类型：** `github`、`git`、`directory`、`hostPattern`、`settings`、`url`、`npm`、`file`。使用 `source: 'settings'` 声明少量内联插件而无需设置托管市场仓库。

**示例：**
```json
{
  "enabledPlugins": {
    "formatter@acme-tools": true,
    "deployer@acme-tools": true,
    "experimental@acme-tools": false
  },
  "extraKnownMarketplaces": {
    "acme-tools": {
      "source": {
        "source": "github",
        "repo": "acme-corp/claude-plugins"
      }
    },
    "inline-tools": {
      "source": {
        "source": "settings",
        "name": "inline-tools",
        "plugins": [
          {
            "name": "code-formatter",
            "source": { "source": "github", "repo": "acme-corp/code-formatter" }
          }
        ]
      }
    }
  }
}
```

---

## 模型配置

### 模型别名

| 别名 | 说明 |
|-------|-------------|
| `"default"` | 为您的账户类型推荐 |
| `"sonnet"` | 最新 Sonnet 模型（Claude Sonnet 4.6） |
| `"opus"` | 最新 Opus 模型（Claude Opus 4.6） |
| `"haiku"` | 快速 Haiku 模型 |
| `"sonnet[1m]"` | Sonnet，1M 令牌上下文 |
| `"opus[1m]"` | Opus，1M 令牌上下文（自 v2.1.75 起在 Max、Team 和 Enterprise 上默认） |
| `"opusplan"` | Opus 用于计划，Sonnet 用于执行 |

**示例：**
```json
{
  "model": "opus"
}
```

### 模型覆盖

将 Anthropic 模型 ID 映射到 Bedrock、Vertex 或 Foundry 部署的提供商特定模型 ID。

| 键 | 类型 | 默认 | 说明 |
|-----|------|---------|-------------|
| `effortLevel` | string | - | 跨会话持久化效能级别。接受 `"low"`、`"medium"` 或 `"high"`。运行 `/effort low`、`/effort medium` 或 `/effort high` 时自动写入。在 Opus 4.6 和 Sonnet 4.6 上支持 |
| `modelOverrides` | object | - | 将模型选择器条目映射到提供商特定 ID（例如 Bedrock 推理配置文件 ARN）。每个键是模型选择器条目名称，每个值是提供商模型 ID |

**示例：**
```json
{
  "modelOverrides": {
    "claude-opus-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-opus-4-6-v1:0",
    "claude-sonnet-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-sonnet-4-6-v1:0"
  }
}
```

### 效能级别

`/model` 命令公开一个**效能级别**控制，调整模型对每个响应应用的推理量。在 `/model` UI 中使用 ← → 箭头键循环切换效能级别。

| 效能级别 | 说明 |
|-------------|-------------|
| 高 | 完整推理深度，最适合复杂任务 |
| 中（默认） | 平衡推理，适合日常任务 |
| 低 | 最少推理，最快响应 |

**如何使用：**
1. 运行 `/effort low`、`/effort medium` 或 `/effort high` 直接设置（v2.1.76+）
2. 或运行 `/model` → 选择模型 → 使用 **← →** 箭头键调整
3. 该设置通过 `settings.json` 中的 `effortLevel` 键持久化

**注意：** 效能级别在 Max 和 Team 计划上对 Opus 4.6 和 Sonnet 4.6 可用。默认在 v2.1.68 中从高更改为中。至 v2.1.75，Opus 4.6 的 1M 上下文窗口默认在 Max、Team 和 Enterprise 计划上可用。

### 模型环境变量

通过 `env` 键配置：

```json
{
  "env": {
    "ANTHROPIC_MODEL": "sonnet",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "custom-haiku-model",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "custom-sonnet-model",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "custom-opus-model",
    "CLAUDE_CODE_SUBAGENT_MODEL": "haiku",
    "MAX_THINKING_TOKENS": "10000"
  }
}
```

---

## 显示和用户体验

### 显示设置

| 键 | 类型 | 默认 | 说明 |
|-----|------|---------|-------------|
| `statusLine` | object | - | 自定义状态行配置 |
| `outputStyle` | string | `"default"` | 输出风格（例如 `"Explanatory"`） |
| `spinnerTipsEnabled` | boolean | `true` | 等待时显示提示 |
| `spinnerVerbs` | object | - | 自定义加载动词，使用 `mode`（"append" 或 "replace"）和 `verbs` 数组 |
| `spinnerTipsOverride` | object | - | 自定义加载提示，使用 `tips`（字符串数组）和可选 `excludeDefault`（布尔值） |
| `respectGitignore` | boolean | `true` | 在文件选择器中尊重 .gitignore |
| `prefersReducedMotion` | boolean | `false` | 减少 UI 中的动画和运动效果 |
| `fileSuggestion` | object | - | 自定义文件建议命令（见下面的文件建议配置） |

### 全局配置设置（`~/.claude.json`）

这些显示偏好存储在 `~/.claude.json`，**不是** `settings.json`。将它们添加到 `settings.json` 将触发架构验证错误。

| 键 | 类型 | 默认 | 说明 |
|-----|------|---------|-------------|
| `autoConnectIde` | boolean | `false` | Claude Code 从外部终端启动时自动连接到运行的 IDE。在外部终端运行时显示在 `/config` 中作为**自动连接到 IDE（外部终端）** |
| `autoInstallIdeExtension` | boolean | `true` | 从 VS Code 终端运行时自动安装 Claude Code IDE 扩展。显示在 `/config` 中作为**自动安装 IDE 扩展**。也可通过 `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL` 环境变量禁用 |
| `editorMode` | string | `"normal"` | 输入提示的关键绑定模式：`"normal"` 或 `"vim"`。运行 `/vim` 时自动写入。显示在 `/config` 中作为**关键绑定模式** |
| `showTurnDuration` | boolean | `true` | 在响应后显示转数时间消息（例如"Cooked for 1m 6s"）。直接编辑 `~/.claude.json` 以更改 |
| `terminalProgressBarEnabled` | boolean | `true` | 在受支持的终端中显示终端进度条（ConEmu、Ghostty 1.2.0+ 和 iTerm2 3.6.6+）。显示在 `/config` 中作为**终端进度条** |
| `teammateMode` | string | `"auto"` | [代理团队](https://code.claude.com/docs/en/agent-teams)队友如何显示：`"auto"`（在 tmux 或 iTerm2 中选择分割窗格，否则进程内）、`"in-process"` 或 `"tmux"`。见[选择显示模式](https://code.claude.com/docs/en/agent-teams#choose-a-display-mode) |

### 状态行配置

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline.sh",
    "padding": 0
  }
}
```

**状态行输入字段：**

状态行命令在 stdin 上接收一个 JSON 对象，具有这些值得注意的字段：

| 字段 | 说明 |
|-------|-------------|
| `workspace.added_dirs` | 通过 `/add-dir` 添加的目录 |
| `context_window.used_percentage` | 上下文窗口使用百分比 |
| `context_window.remaining_percentage` | 上下文窗口剩余百分比 |
| `current_usage` | 当前上下文窗口令牌计数 |
| `exceeds_200k_tokens` | 上下文是否超过 200k 令牌 |
| `rate_limits.five_hour.used_percentage` | 五小时速率限制使用百分比（v2.1.80+） |
| `rate_limits.five_hour.resets_at` | 五小时速率限制重置时间戳 |
| `rate_limits.seven_day.used_percentage` | 七天速率限制使用百分比 |
| `rate_limits.seven_day.resets_at` | 七天速率限制重置时间戳 |

### 文件建议配置

文件建议脚本在 stdin 上接收一个 JSON 对象（例如 `{"query": "src/comp"}`），必须输出最多 15 条文件路径（每行一条）。

```json
{
  "fileSuggestion": {
    "type": "command",
    "command": "~/.claude/file-suggestion.sh"
  },
  "respectGitignore": true
}
```

**示例：**
```json
{
  "statusLine": {
    "type": "command",
    "command": "git branch --show-current 2>/dev/null || echo 'no-branch'"
  },
  "spinnerTipsEnabled": true,
  "spinnerVerbs": {
    "mode": "replace",
    "verbs": ["Cooking", "Brewing", "Crafting", "Conjuring"]
  },
  "spinnerTipsOverride": {
    "tips": ["Use /compact at ~50% context", "Start with plan mode for complex tasks"],
    "excludeDefault": true
  }
}
```

---

## AWS 和云凭证

### AWS 设置

| 键 | 类型 | 说明 |
|-----|------|-------------|
| `awsAuthRefresh` | string | 用于刷新 AWS 身份验证的脚本（修改 `.aws` 目录） |
| `awsCredentialExport` | string | 输出带 AWS 凭证的 JSON 的脚本 |

**示例：**
```json
{
  "awsAuthRefresh": "aws sso login --profile myprofile",
  "awsCredentialExport": "/bin/generate_aws_grant.sh"
}
```

### OpenTelemetry

| 键 | 类型 | 说明 |
|-----|------|-------------|
| `otelHeadersHelper` | string | 用于生成动态 OpenTelemetry 头的脚本 |

**示例：**
```json
{
  "otelHeadersHelper": "/bin/generate_otel_headers.sh"
}
```

---

## 通过 `env` 的环境变量

为所有 Claude Code 会话设置环境变量。

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "...",
    "NODE_ENV": "development",
    "DEBUG": "true"
  }
}
```

### 常见环境变量

| 变量 | 说明 |
|----------|-------------|
| `ANTHROPIC_API_KEY` | 身份验证的 API 密钥 |
| `ANTHROPIC_AUTH_TOKEN` | OAuth 令牌 |
| `ANTHROPIC_BASE_URL` | 自定义 API 端点 |
| `ANTHROPIC_CUSTOM_MODEL_OPTION` | 模型 ID 作为自定义条目添加到 `/model` 选择器。用于使非标准或网关特定的模型可选择，而无需替换内置别名 |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_NAME` | `/model` 选择器中自定义模型条目的显示名称。未设置时默认为模型 ID |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_DESCRIPTION` | `/model` 选择器中自定义模型条目的显示描述。未设置时默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_MODEL` | 要使用的模型名称。接受别名（`sonnet`、`opus`、`haiku`）或完整模型 ID。覆盖 `model` 设置 |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | 使用自定义模型 ID 覆盖 Haiku 模型别名（例如用于第三方部署） |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用固定模型时自定义 `/model` 选择器中的 Haiku 条目标签。默认为模型 ID |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中的 Haiku 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_SUPPORTED_CAPABILITIES` | 为固定 Haiku 模型覆盖功能检测。逗号分隔值（例如 `effort,thinking`）。当固定模型支持自动检测无法确认的功能时需要 |
| `CLAUDECODE` | 在 Claude Code 生成的 shell 环境中设置为 `1`（Bash 工具、tmux 会话）。未在钩子或状态行命令中设置。用于检测脚本何时在 Claude Code shell 内运行 |
| `CLAUDE_CODE_SKIP_FAST_MODE_NETWORK_ERRORS` | 设置为 `1` 以允许组织状态检查由于网络错误失败时的快速模式。当公司代理阻止状态端点时很有用 |
| `CLAUDE_CODE_USE_BEDROCK` | 使用 AWS Bedrock（`1` 启用） |
| `CLAUDE_CODE_USE_VERTEX` | 使用 Google Vertex AI（`1` 启用） |
| `CLAUDE_CODE_USE_FOUNDRY` | 使用 Microsoft Foundry（`1` 启用） |
| `CLAUDE_CODE_USE_POWERSHELL_TOOL` | 设置为 `1` 以在 Windows 上启用 PowerShell 工具（选择加入预览）。启用后，Claude 可以原生运行 PowerShell 命令而不是通过 Git Bash 路由。仅在本机 Windows 上受支持，不支持 WSL（v2.1.84） |
| `CLAUDE_CODE_ENABLE_TELEMETRY` | 启用/禁用遥测（`0` 或 `1`） |
| `DISABLE_ERROR_REPORTING` | 禁用错误报告（`1` 禁用） |
| `DISABLE_TELEMETRY` | 禁用遥测（`1` 禁用） |
| `MCP_TIMEOUT` | MCP 启动超时（ms） |
| `MAX_MCP_OUTPUT_TOKENS` | 最大 MCP 输出令牌（默认：25000）。输出超过 10,000 令牌时显示警告 |
| `BASH_MAX_TIMEOUT_MS` | Bash 命令超时 |
| `BASH_MAX_OUTPUT_LENGTH` | 最大 bash 输出长度 |
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | 自动压缩阈值百分比（1-100）。默认约为 95%。设置为更低值（例如 `50`）以更早触发压缩。高于 95% 的值无效。使用 `/context` 监视当前使用情况。示例：`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=50 claude` |
| `CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR` | 保持 bash 调用之间的 cwd（`1` 启用） |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | 禁用后台任务（`1` 禁用） |
| `ENABLE_TOOL_SEARCH` | MCP 工具搜索阈值（例如 `auto:5`） |
| `DISABLE_PROMPT_CACHING` | 禁用所有提示缓存（`1` 禁用） |
| `DISABLE_PROMPT_CACHING_HAIKU` | 禁用 Haiku 提示缓存 |
| `DISABLE_PROMPT_CACHING_SONNET` | 禁用 Sonnet 提示缓存 |
| `DISABLE_PROMPT_CACHING_OPUS` | 禁用 Opus 提示缓存 |
| `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` | 禁用实验性 beta 功能（`1` 禁用） |
| `CLAUDE_CODE_SHELL` | 覆盖自动 shell 检测 |
| `CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS` | 覆盖默认文件读取令牌限制 |
| `CLAUDE_CODE_ENABLE_TASKS` | 设置为 `true` 以在非交互模式（`-p` 标志）中启用任务跟踪。任务默认在交互模式中启用 |
| `CLAUDE_CODE_EXIT_AFTER_STOP_DELAY` | SDK 模式空闲后自动退出持续时间（ms） |
| `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING` | 禁用自适应思考（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_1M_CONTEXT` | 禁用 1M 令牌上下文窗口（`1` 禁用） |
| `CLAUDE_CODE_ACCOUNT_UUID` | 覆盖身份验证的账户 UUID |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` | 禁用 git 相关系统提示指令 |
| `CLAUDE_CODE_NEW_INIT` | 设置为 `true` 以使 `/init` 运行交互式设置流。询问在探索代码库前要生成哪些文件（CLAUDE.md、技能、钩子）。没有这个，`/init` 自动生成 CLAUDE.md |
| `CLAUDE_CODE_PLUGIN_SEED_DIR` | 一个或多个只读插件种子目录的路径，在 Unix 上用 `:` 分隔，在 Windows 上用 `;` 分隔。将预填充的插件捆绑到容器镜像中。Claude Code 在启动时从这些目录注册市场，并使用预缓存的插件而无需重新克隆 |
| `ENABLE_CLAUDEAI_MCP_SERVERS` | 启用 Claude.ai MCP 服务器 |
| `CLAUDE_CODE_EFFORT_LEVEL` | 设置效能级别：`low`、`medium`、`high`、`max`（仅 Opus 4.6）或 `auto`（使用模型默认）。优先于 `/effort` 和 `effortLevel` 设置 |
| `CLAUDE_CODE_MAX_TURNS` | 停止前的最大代理转数 |
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | 相当于设置 `DISABLE_AUTOUPDATER`、`DISABLE_FEEDBACK_COMMAND`、`DISABLE_ERROR_REPORTING` 和 `DISABLE_TELEMETRY` |
| `CLAUDE_CODE_SKIP_SETTINGS_SETUP` | 跳过首次运行设置流程 |
| `CLAUDE_CODE_PROMPT_CACHING_ENABLED` | 覆盖提示缓存行为 |
| `CLAUDE_CODE_DISABLE_TOOLS` | 逗号分隔的工具列表禁用 |
| `CLAUDE_CODE_DISABLE_MCP` | 禁用所有 MCP 服务器（`1` 禁用） |
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS` | 每个响应的最大输出令牌。默认：32,000（Opus 4.6 至 v2.1.77 为 64,000）。上限：64,000（Opus 4.6 和 Sonnet 4.6 至 v2.1.77 为 128,000） |
| `CLAUDE_CODE_DISABLE_FAST_MODE` | 完全禁用快速模式（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` | 设置为 `1` 以禁用流式请求流中失败时的非流式备用。流式错误传播到重试层。当代理或网关导致备用产生重复工具执行时很有用（v2.1.83） |
| `CLAUDE_CODE_DISABLE_AUTO_MEMORY` | 禁用自动内存（`1` 禁用） |
| `CLAUDE_CODE_USER_EMAIL` | 同步提供用户电子邮件以进行身份验证 |
| `CLAUDE_CODE_ORGANIZATION_UUID` | 同步提供组织 UUID 以进行身份验证 |
| `CLAUDE_CONFIG_DIR` | 自定义配置目录（覆盖默认 `~/.claude`） |
| `ANTHROPIC_CUSTOM_HEADERS` | API 请求的自定义头（`Name: Value` 格式，多个头用换行分隔） |
| `ANTHROPIC_FOUNDRY_API_KEY` | Microsoft Foundry 身份验证的 API 密钥 |
| `ANTHROPIC_FOUNDRY_BASE_URL` | Foundry 资源的基础 URL |
| `ANTHROPIC_FOUNDRY_RESOURCE` | Foundry 资源名称 |
| `AWS_BEARER_TOKEN_BEDROCK` | Bedrock API 密钥用于身份验证 |
| `ANTHROPIC_SMALL_FAST_MODEL` | **已弃用** — 改用 `ANTHROPIC_DEFAULT_HAIKU_MODEL` |
| `ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION` | 已弃用 Haiku 级模型覆盖的 AWS 区域 |
| `CLAUDE_CODE_SHELL_PREFIX` | 前缀到 bash 命令的命令 |
| `BASH_DEFAULT_TIMEOUT_MS` | 默认 bash 命令超时（ms） |
| `CLAUDE_CODE_SKIP_BEDROCK_AUTH` | 跳过 Bedrock 的 AWS 身份验证（`1` 跳过） |
| `CLAUDE_CODE_SKIP_FOUNDRY_AUTH` | 跳过 Foundry 的 Azure 身份验证（`1` 跳过） |
| `CLAUDE_CODE_SKIP_VERTEX_AUTH` | 跳过 Vertex 的 Google 身份验证（`1` 跳过） |
| `CLAUDE_CODE_PROXY_RESOLVES_HOSTS` | 允许代理执行 DNS 解析 |
| `CLAUDE_CODE_API_KEY_HELPER_TTL_MS` | `apiKeyHelper` 的凭证刷新间隔（ms） |
| `CLAUDE_CODE_CLIENT_CERT` | mTLS 的客户端证书路径 |
| `CLAUDE_CODE_CLIENT_KEY` | mTLS 的客户端私钥路径 |
| `CLAUDE_CODE_CLIENT_KEY_PASSPHRASE` | 加密 mTLS 密钥的密码 |
| `CLAUDE_CODE_PLUGIN_GIT_TIMEOUT_MS` | 插件市场 git 克隆超时（ms）（默认：120000） |
| `CLAUDE_CODE_HIDE_ACCOUNT_INFO` | 从 UI 隐藏电子邮件/组织信息 |
| `CLAUDE_CODE_DISABLE_CRON` | 禁用计划/cron 任务（`1` 禁用） |
| `DISABLE_INSTALLATION_CHECKS` | 禁用安装警告 |
| `DISABLE_FEEDBACK_COMMAND` | 禁用 `/feedback` 命令。旧名称 `DISABLE_BUG_COMMAND` 也被接受 |
| `DISABLE_NON_ESSENTIAL_MODEL_CALLS` | 禁用风味文本和非必要模型调用 |
| `DISABLE_COST_WARNINGS` | 禁用成本警告消息 |
| `CLAUDE_CODE_SUBAGENT_MODEL` | 子代理的覆盖模型（例如 `haiku`、`sonnet`） |
| `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` | 设置为 `1` 以从子进程环境中清除 Anthropic 和云提供商凭证（Bash 工具、钩子、MCP stdio 服务器）。当子进程不应继承 API 密钥时用于纵深防御（v2.1.83） |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd 钩子超时（ms）（替换硬 1.5 秒限制） |
| `CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY` | 禁用反馈调查提示（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_TERMINAL_TITLE` | 禁用终端标题更新（`1` 禁用） |
| `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL` | 跳过自动 IDE 扩展安装（`1` 跳过） |
| `CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS` | OTel 头助手脚本的去抖动间隔（ms） |
| `CLAUDE_STREAM_IDLE_TIMEOUT_MS` | 空闲监视程序关闭停滞连接前的超时（ms）。默认：`90000`（90 秒）。如果长期运行的工具或缓慢网络导致提前超时错误，增加 |
| `OTEL_LOG_TOOL_DETAILS` | 设置为 `1` 以在 OpenTelemetry 事件中包括 `tool_parameters`。默认为隐私而省略 |
| `CLAUDE_CODE_MCP_SERVER_NAME` | MCP 服务器名称，作为环境变量传递给 `headersHelper` 脚本，以便它们可以生成服务器特定的身份验证头 |
| `CLAUDE_CODE_MCP_SERVER_URL` | MCP 服务器的 URL，作为环境变量传递给 `headersHelper` 脚本，与 `CLAUDE_CODE_MCP_SERVER_NAME` 相同 |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | 覆盖 Opus 模型别名（例如 `claude-opus-4-6[1m]`） |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用固定模型时自定义 `/model` 选择器中的 Opus 条目标签。默认为模型 ID |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中的 Opus 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_SUPPORTED_CAPABILITIES` | 为固定 Opus 模型覆盖功能检测。逗号分隔值（例如 `effort,thinking`）。当固定模型支持自动检测无法确认的功能时需要 |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | 覆盖 Sonnet 模型别名（例如 `claude-sonnet-4-6`） |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用固定模型时自定义 `/model` 选择器中的 Sonnet 条目标签。默认为模型 ID |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中的 Sonnet 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_SUPPORTED_CAPABILITIES` | 为固定 Sonnet 模型覆盖功能检测。逗号分隔值（例如 `effort,thinking`）。当固定模型支持自动检测无法确认的功能时需要 |
| `MAX_THINKING_TOKENS` | 每个响应的最大扩展思考令牌 |
| `CLAUDE_CODE_AUTO_COMPACT_WINDOW` | 为自动压缩计算设置上下文容量（令牌）。默认为模型的上下文窗口（200K 标准，1M 用于扩展上下文模型）。在 1M 模型上使用较低值（例如 `500000`）将其视为 500K。限制在实际上下文窗口。`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` 作为此值的百分比应用。设置此解耦压缩阈值与状态行的 `used_percentage` |
| `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` | 启用提示建议 |
| `CLAUDE_CODE_PLAN_MODE_REQUIRED` | 需要会话的计划模式 |
| `CLAUDE_CODE_TEAM_NAME` | 代理团队的团队名称 |
| `CLAUDE_CODE_TASK_LIST_ID` | 任务集成的任务列表 ID |
| `CLAUDE_ENV_FILE` | 自定义环境文件路径 |
| `FORCE_AUTOUPDATE_PLUGINS` | 强制插件自动更新（`1` 启用） |
| `HTTP_PROXY` | 网络请求的 HTTP 代理 URL |
| `HTTPS_PROXY` | 网络请求的 HTTPS 代理 URL |
| `NO_PROXY` | 绕过代理的主机的逗号分隔列表 |
| `MCP_TOOL_TIMEOUT` | MCP 工具执行超时（ms） |
| `MCP_CLIENT_SECRET` | MCP OAuth 客户端密钥 |
| `MCP_OAUTH_CALLBACK_PORT` | MCP OAuth 回调端口 |
| `IS_DEMO` | 启用演示模式 |
| `SLASH_COMMAND_TOOL_CHAR_BUDGET` | 斜杠命令工具输出的字符预算 |
| `VERTEX_REGION_CLAUDE_3_5_HAIKU` | Claude 3.5 Haiku 的 Vertex AI 区域覆盖 |
| `VERTEX_REGION_CLAUDE_3_7_SONNET` | Claude 3.7 Sonnet 的 Vertex AI 区域覆盖 |
| `VERTEX_REGION_CLAUDE_4_0_OPUS` | Claude 4.0 Opus 的 Vertex AI 区域覆盖 |
| `VERTEX_REGION_CLAUDE_4_0_SONNET` | Claude 4.0 Sonnet 的 Vertex AI 区域覆盖 |
| `VERTEX_REGION_CLAUDE_4_1_OPUS` | Claude 4.1 Opus 的 Vertex AI 区域覆盖 |

---

## 有用的命令

| 命令 | 说明 |
|---------|-------------|
| `/model` | 切换模型并调整 Opus 4.6 效能级别 |
| `/effort` | 直接设置效能级别：`low`、`medium`、`high`（v2.1.76+） |
| `/config` | 交互式配置 UI |
| `/memory` | 查看/编辑所有内存文件 |
| `/agents` | 管理子代理 |
| `/mcp` | 管理 MCP 服务器 |
| `/hooks` | 查看配置的钩子 |
| `/plugin` | 管理插件 |
| `/keybindings` | 配置自定义键盘快捷方式 |
| `/skills` | 查看和管理技能 |
| `/permissions` | 查看和管理权限规则 |
| `--doctor` | 诊断配置问题 |
| `--debug` | 调试模式，具有钩子执行细节 |

---

## 快速参考：完整示例

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "model": "sonnet",
  "agent": "code-reviewer",
  "language": "english",
  "cleanupPeriodDays": 30,
  "autoUpdatesChannel": "stable",
  "alwaysThinkingEnabled": true,
  "includeGitInstructions": true,
  "defaultShell": "bash",
  "plansDirectory": "./plans",
  "effortLevel": "medium",

  "worktree": {
    "symlinkDirectories": ["node_modules"],
    "sparsePaths": ["packages/my-app", "shared/utils"]
  },

  "modelOverrides": {
    "claude-opus-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-opus-4-6-v1:0"
  },

  "autoMode": {
    "environment": [
      "Source control: github.example.com/acme-corp and all repos under it",
      "Trusted internal domains: *.internal.example.com"
    ]
  },

  "permissions": {
    "allow": [
      "Edit(*)",
      "Write(*)",
      "Bash(npm run *)",
      "Bash(git *)",
      "WebFetch(domain:*)",
      "mcp__*",
      "Agent(*)"
    ],
    "deny": [
      "Read(.env)",
      "Read(./secrets/**)"
    ],
    "additionalDirectories": ["../shared/"],
    "defaultMode": "acceptEdits"
  },

  "enableAllProjectMcpServers": true,

  "sandbox": {
    "enabled": true,
    "excludedCommands": ["git", "docker"],
    "filesystem": {
      "denyRead": ["./secrets/"],
      "denyWrite": ["./.env"]
    }
  },

  "attribution": {
    "commit": "Generated with Claude Code",
    "pr": ""
  },

  "statusLine": {
    "type": "command",
    "command": "git branch --show-current"
  },

  "spinnerTipsEnabled": true,
  "spinnerTipsOverride": {
    "tips": ["Custom tip 1", "Custom tip 2"],
    "excludeDefault": false
  },
  "prefersReducedMotion": false,

  "env": {
    "NODE_ENV": "development",
    "CLAUDE_CODE_EFFORT_LEVEL": "medium"
  }
}
```

---

## 来源

- [Claude Code 设置文档](https://code.claude.com/docs/en/settings)
- [Claude Code 设置 JSON 架构](https://json.schemastore.org/claude-code-settings.json)
- [Claude Code 更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code GitHub 设置示例](https://github.com/feiskyer/claude-code-settings)
- [Eesel AI - 开发者指南 settings.json](https://www.eesel.ai/blog/settings-json-claude-code)
- [Shipyard - Claude Code CLI 速查表](https://shipyard.build/blog/claude-code-cheat-sheet/)
- [Claude Code 环境变量参考](https://code.claude.com/docs/en/env-vars)
- [Claude Code 权限参考](https://code.claude.com/docs/en/permissions)
