# HOOKS-README
包含钩子的所有详情、脚本和说明

## 钩子事件概览 - [官方22个钩子](https://code.claude.com/docs/en/hooks)
Claude Code提供在工作流中的不同点运行的几个钩子事件：

| # | 钩子 | 描述 | 选项 |
|:-:|------|-------------|---------|
| 1 | `PreToolUse` | 在工具调用前运行 (可以阻止它们) | `async`, `timeout: 5000`, `tool_use_id` |
| 2 | `PermissionRequest` | 当Claude Code从用户请求权限时运行 | `async`, `timeout: 5000`, `permission_suggestions` |
| 3 | `PostToolUse` | 在工具调用成功完成后运行 | `async`, `timeout: 5000`, `tool_response`, `tool_use_id` |
| 4 | `PostToolUseFailure` | 在工具调用失败后运行 | `async`, `timeout: 5000`, `error`, `is_interrupt`, `tool_use_id` |
| 5 | `UserPromptSubmit` | 当用户提交提示时运行，在Claude处理它之前 | `async`, `timeout: 5000`, `prompt` |
| 6 | `Notification` | 当Claude Code发送通知时运行 | `async`, `timeout: 5000`, `notification_type`, `message`, `title` |
| 7 | `Stop` | 当Claude Code完成响应时运行 | `async`, `timeout: 5000`, `last_assistant_message`, `stop_hook_active` |
| 8 | `SubagentStart` | 当代理任务启动时运行 | `async`, `timeout: 5000`, `agent_id`, `agent_type` |
| 9 | `SubagentStop` | 当代理任务完成时运行 | `async`, `timeout: 5000`, `agent_id`, `agent_type`, `last_assistant_message`, `agent_transcript_path`, `stop_hook_active` |
| 10 | `PreCompact` | 在Claude Code即将运行紧凑操作时运行 | `async`, `timeout: 5000`, `once`, `trigger`, `custom_instructions` |
| 11 | `PostCompact` | 在Claude Code完成紧凑操作后运行 | `async`, `timeout: 5000`, `trigger`, `compact_summary` |
| 12 | `SessionStart` | 当Claude Code启动新会话或恢复现有会话时运行 | `async`, `timeout: 5000`, `once`, `agent_type`, `model`, `source` |
| 13 | `SessionEnd` | 当Claude Code会话结束时运行 | `async`, `timeout: 5000`, `once`, `reason` |
| 14 | `Setup` | 当Claude Code为项目初始化运行/setup命令时运行 | `async`, `timeout: 30000` |
| 15 | `TeammateIdle` | 当队友代理变成空闲时运行 (实验性代理团队) | `async`, `timeout: 5000`, `teammate_name`, `team_name` |
| 16 | `TaskCompleted` | 当后台任务完成时运行 (实验性代理团队) | `async`, `timeout: 5000`, `task_id`, `task_subject`, `task_description`, `teammate_name`, `team_name` |
| 17 | `ConfigChange` | 当会话中配置文件变化时运行 | `async`, `timeout: 5000`, `file_path`, `source` |
| 18 | `WorktreeCreate` | 当代理工作树隔离为自定义VCS设置创建工作树时运行 | `async`, `timeout: 5000`, `name` |
| 19 | `WorktreeRemove` | 当代理工作树隔离移除工作树用于自定义VCS清理时运行 | `async`, `timeout: 5000`, `worktree_path` |
| 20 | `InstructionsLoaded` | 当CLAUDE.md或 `.claude/rules/*.md` 文件加载到上下文中时运行 | `async`, `timeout: 5000`, `file_path`, `memory_type`, `load_reason`, `globs`, `trigger_file_path`, `parent_file_path` |
| 21 | `Elicitation` | 当MCP服务器在工具调用中请求用户输入时运行 | `async`, `timeout: 5000`, `mcp_server_name`, `message`, `mode`, `url`, `elicitation_id`, `requested_schema` |
| 22 | `ElicitationResult` | 在用户响应MCP询问后，在响应发送回服务器之前运行 | `async`, `timeout: 5000`, `mcp_server_name`, `action`, `content`, `mode`, `elicitation_id` |

> **注意**: 钩子15-16 (`TeammateIdle` 和 `TaskCompleted`) 需要实验性代理团队功能。启动Claude Code时设置 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 以启用它们。

### 不在官方文档中

以下项目存在于 [Claude Code更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md) 但**未列在** [官方钩子参考](https://code.claude.com/docs/en/hooks) 中：

| 项目 | 添加于 | 更新日志参考 | 注释 |
|------|----------|-------------------|-------|
| `Setup` 钩子 | [v2.1.10](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#2110) | "添加新的Setup钩子事件，可通过 `--init`、`--init-only` 或 `--maintenance` CLI标志触发用于存储库设置和维护操作" | 未在官方钩子参考页面上列出 (21个钩子列出，Setup排除) |
| 代理前置钩子 | [v2.1.0](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#210) | "添加对代理前置钩子的支持，允许代理在其前置中定义PreToolUse、PostToolUse和Stop钩子作用域到代理的生命周期" | 更新日志只提及3个钩子，但测试确认**6个钩子**在代理会话中实际触发：PreToolUse、PostToolUse、PermissionRequest、PostToolUseFailure、Stop、SubagentStop。不是所有16个钩子都支持。 |

## 前置条件

在使用钩子前，确保您在系统上安装了**Python 3**。

### 必需软件

#### 所有平台 (Windows、macOS、Linux)
- **Python 3**: 运行钩子脚本所需
- 验证安装: `python3 --version`

**安装说明:**
- **Windows**: 从 [python.org](https://www.python.org/downloads/) 下载或通过 `winget install Python.Python.3` 安装
- **macOS**: 通过 `brew install python3` 安装 (需要 [Homebrew](https://brew.sh/))
- **Linux**: 通过 `sudo apt install python3` 安装 (Ubuntu/Debian) 或 `sudo yum install python3` (RHEL/CentOS)

### 音频播放器 (可选 - 自动检测)

钩子脚本自动检测并使用适当的音频播放器：

- **macOS**: 使用 `afplay` (内置，无需安装)
- **Linux**: 使用来自 `pulseaudio-utils` 的 `paplay` - 通过 `sudo apt install pulseaudio-utils` 安装
- **Windows**: 使用内置 `winsound` 模块 (随Python附带)

### 钩子如何执行

钩子在 `.claude/settings.json` 中配置为直接与Python 3运行：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py"
}
```

## 配置钩子 (启用/禁用)

钩子可在全局和单个级别轻松启用或禁用。

### 一次性禁用所有钩子

编辑 `.claude/settings.local.json` 并设置：
```json
{
  "disableAllHooks": true
}
```

**注意**: `.claude/settings.local.json` 文件是git忽略的，所以每个用户可以配置自己的钩子偏好而不影响团队在 `.claude/settings.json` 中的共享设置。

> **托管设置**: 如果管理员通过托管策略设置配置了钩子，在用户、项目或本地设置中设置的 `disableAllHooks` 无法禁用那些托管钩子 (在v2.1.49中修复)。

### 禁用单个钩子

为了获得细粒度控制，您可以通过编辑钩子配置文件禁用特定钩子。

#### 配置文件

有两个用于管理单个钩子的配置文件：

1. **`.claude/hooks/config/hooks-config.json`** - 提交到git的共享/默认配置
2. **`.claude/hooks/config/hooks-config.local.json`** - 您的个人覆盖 (git忽略)

本地配置文件 (`.local.json`) 优先于共享配置，允许每个开发者自定义他们的钩子行为而不影响团队。

#### 共享配置

编辑 `.claude/hooks/config/hooks-config.json` 用于团队范围的默认值：

```json
{
  "disableLogging": false,
  "disablePreToolUseHook": false,
  "disablePermissionRequestHook": false,
  "disablePostToolUseHook": false,
  "disablePostToolUseFailureHook": false,
  "disableUserPromptSubmitHook": false,
  "disableNotificationHook": false,
  "disableStopHook": false,
  "disableSubagentStartHook": false,
  "disableSubagentStopHook": false,
  "disablePreCompactHook": false,
  "disablePostCompactHook": false,
  "disableElicitationHook": false,
  "disableElicitationResultHook": false,
  "disableSessionStartHook": false,
  "disableSessionEndHook": false,
  "disableSetupHook": false,
  "disableTeammateIdleHook": false,
  "disableTaskCompletedHook": false,
  "disableConfigChangeHook": false,
  "disableWorktreeCreateHook": false,
  "disableWorktreeRemoveHook": false,
  "disableInstructionsLoadedHook": false
}
```

**配置选项:**
- `disableLogging`: 设置为 `true` 禁用钩子事件记录到 `.claude/hooks/logs/hooks-log.jsonl` (有助于防止日志文件增长)

#### 本地配置 (个人覆盖)

创建或编辑 `.claude/hooks/config/hooks-config.local.json` 用于个人偏好：

```json
{
  "disableLogging": true,
  "disablePostToolUseHook": true,
  "disableSessionStartHook": true
}
```

在此示例中，记录被禁用，PostToolUse和SessionStart钩子被覆盖本地。所有其他钩子将使用共享配置值。

**注意**: 单个钩子切换由钩子脚本 (`.claude/hooks/scripts/hooks.py`) 检查。本地设置覆盖共享设置，如果钩子被禁用，脚本以静默方式退出而不播放任何声音或执行钩子逻辑。

### 文本转语音 (TTS)
用于生成声音的网站: https://elevenlabs.io/
使用的声音: Samara X


## 代理前置钩子

Claude Code 2.1.0引入了对代理前置文件中代理特定钩子的支持。这些钩子仅在代理的生命周期内运行并支持钩子事件的子集。

### 支持的代理钩子

代理前置钩子支持**6个钩子** (不是所有16个)。更新日志最初仅提及3个，但测试确认6个钩子实际在代理会话中触发：
- `PreToolUse`: 在代理使用工具前运行
- `PostToolUse`: 在代理完成工具使用后运行
- `PermissionRequest`: 当工具需要用户权限时运行
- `PostToolUseFailure`: 在工具调用失败后运行
- `Stop`: 当代理完成时运行
- `SubagentStop`: 当代理完成时运行

> **注意**: [v2.1.0更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#210) 仅提及3个钩子：*"添加对代理前置钩子的支持，允许代理在其前置中定义PreToolUse、PostToolUse和Stop钩子作用域到代理的生命周期"*。然而，使用 `claude-code-voice-hook-agent` 的测试确认6个钩子实际在代理会话中触发。其他10个钩子 (例如Notification、SessionStart、SessionEnd等) 在代理上下文中不触发。
>
> **更新 (Feb 2026)**: [官方钩子参考](https://code.claude.com/docs/en/hooks) 现在声明*"所有钩子事件都支持"*用于技能/代理前置钩子。这可能意味着支持已超越最初测试的6个钩子。建议重新测试以验证其他钩子现在是否在代理会话中触发。

### 代理声音文件夹

代理特定的声音存储在单独的文件夹中：
- `.claude/hooks/sounds/agent_pretooluse/`
- `.claude/hooks/sounds/agent_posttooluse/`
- `.claude/hooks/sounds/agent_permissionrequest/`
- `.claude/hooks/sounds/agent_posttoolusefailure/`
- `.claude/hooks/sounds/agent_stop/`
- `.claude/hooks/sounds/agent_subagentstop/`

### 创建带钩子的代理

1. 在 `.claude/agents/` 中创建代理定义文件：

```markdown
---
name: my-agent
description: 此代理所做的描述
hooks:
  PreToolUse:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: PreToolUse
  PostToolUse:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: PostToolUse
  PermissionRequest:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: PermissionRequest
  PostToolUseFailure:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: PostToolUseFailure
  Stop:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: Stop
  SubagentStop:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: SubagentStop
---

您的代理说明在这里...
```

2. 添加声音文件到代理声音文件夹中：
   - `agent_pretooluse/agent_pretooluse.wav`
   - `agent_posttooluse/agent_posttooluse.wav`
   - `agent_permissionrequest/agent_permissionrequest.wav`
   - `agent_posttoolusefailure/agent_posttoolusefailure.wav`
   - `agent_stop/agent_stop.wav`
   - `agent_subagentstop/agent_subagentstop.wav`

### 示例：天气获取代理

参见 `.claude/agents/claude-code-voice-hook-agent.md` 用于配置了钩子的完整代理示例。

### 钩子选项：`once: true`

`once: true` 选项确保钩子在每个会话中仅运行一次：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "once": true
}
```

这对于只应触发一次的钩子很有用如 `SessionStart`、`SessionEnd` 和 `PreCompact`。

> **注意**: `once` 选项仅用于**技能，不是代理**。它在基于设置的钩子和技能前置中工作，但在代理前置钩子中不支持。

### 钩子选项：`async: true`

钩子可通过添加 `"async": true` 在后台运行而不阻止Claude Code的执行：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true
}
```

**何时使用异步钩子：**
- 记录和分析
- 通知和声音效果
- 任何不应该减慢Claude Code的副作用

此项目对所有钩子使用 `async: true` 因为语音通知是不需要阻止执行的副作用。`timeout` 指定异步钩子在被终止前可运行多长时间。

### 钩子选项：`statusMessage`

`statusMessage` 字段设置钩子运行时显示给用户的自定义旋转器消息：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true,
  "statusMessage": "PreToolUse"
}
```

此项目在所有钩子上设置 `statusMessage` 到钩子事件名称，所以旋转器简要显示哪个钩子正在触发 (例如，"PreToolUse"、"SessionStart"、"Stop")。这对于同步钩子最可见；对于异步钩子，消息在钩子在后台运行前闪烁。

## 钩子类型

Claude Code支持四种钩子处理程序类型。此项目对所有声音播放使用 `command` 钩子。

### `type: "command"` (由此项目使用)

运行shell命令。通过stdin接收JSON输入，通过退出代码和stdout通信结果。

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true
}
```

### `type: "prompt"`

发送提示到Claude模型用于单轮评估。模型返回是/否决定作为JSON (`{"ok": true/false, "reason": "..."}`)。对于需要判断而不是确定规则的决定很有用。

```json
{
  "type": "prompt",
  "prompt": "检查所有任务是否完成。$ARGUMENTS",
  "timeout": 30
}
```

**支持的事件**: PreToolUse、PostToolUse、PostToolUseFailure、PermissionRequest、UserPromptSubmit、Stop、SubagentStop、TaskCompleted。**仅命令事件 (不支持prompt/代理类型)**: ConfigChange、Elicitation、ElicitationResult、InstructionsLoaded、Notification、PostCompact、PreCompact、SessionEnd、SessionStart、Setup、SubagentStart、TeammateIdle、WorktreeCreate、WorktreeRemove。

### `type: "agent"`

生成具有多轮工具访问权限的代理 (Read、Grep、Glob) 以在返回决定前验证条件。响应格式与提示钩子相同。当验证需要检查实际文件或测试输出时很有用。

```json
{
  "type": "agent",
  "prompt": "验证所有单元测试通过。$ARGUMENTS",
  "timeout": 120
}
```

### `type: "http"` (自v2.1.63)

将JSON发送到URL并接收JSON响应，而不是运行shell命令。对于与外部服务或webhooks集成很有用。启用沙箱时，HTTP钩子通过沙箱网络代理路由。

```json
{
  "type": "http",
  "url": "http://localhost:8080/hooks/pre-tool-use",
  "timeout": 30,
  "headers": {
    "Authorization": "Bearer $MY_TOKEN"
  },
  "allowedEnvVars": ["MY_TOKEN"]
}
```

**不支持**: ConfigChange、Elicitation、ElicitationResult、InstructionsLoaded、Notification、PostCompact、PreCompact、SessionEnd、SessionStart、Setup、SubagentStart、TeammateIdle、WorktreeCreate、WorktreeRemove (仅命令事件)。标头支持环境变量内插 `$VAR_NAME`，但仅用于在 `allowedEnvVars` 中明确列出的变量。

## 环境变量

Claude Code为钩子脚本提供这些环境变量：

| 变量 | 可用性 | 描述 |
|----------|-------------|-------------|
| `$CLAUDE_PROJECT_DIR` | 所有钩子 | 项目根目录。用引号包围带空格的路径 |
| `$CLAUDE_ENV_FILE` | 仅SessionStart | 用于持久化后续Bash命令的环境变量的文件路径。使用追加 (`>>`) 以保留来自其他钩子的变量 |
| `${CLAUDE_PLUGIN_ROOT}` | 插件钩子 | 插件根目录，对于与插件捆绑的脚本 |
| `$CLAUDE_CODE_REMOTE` | 所有钩子 | 在远程Web环境中设置为 `"true"`，在本地CLI中不设置 |
| `${CLAUDE_SKILL_DIR}` | 技能钩子 | 技能自己的目录，对于与技能捆绑的脚本 (自v2.1.69) |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd钩子 | 覆盖SessionEnd钩子超时 (毫秒)。在v2.1.74之前，SessionEnd钩子被无条件地在1.5秒后杀死。现在遵循钩子的 `timeout` 值或此环境变量 (自v2.1.74) |
| `session_id` (通过stdin JSON) | 所有钩子 | 当前会话ID，作为stdin上JSON输入的一部分接收 (不是环境变量) |

### 常见输入字段 (stdin JSON)

每个钩子在stdin上接收包含这些常见字段的JSON对象，除了"选项"列中列出的任何钩子特定字段：

| 字段 | 类型 | 描述 |
|-------|------|-------------|
| `hook_event_name` | string | 触发的钩子事件的名称 (例如 `"PreToolUse"`、`"Stop"`) |
| `session_id` | string | 当前会话标识符 |
| `transcript_path` | string | 对话文字记录JSON文件的路径 |
| `cwd` | string | 当前工作目录 |
| `permission_mode` | string | 当前权限模式: `default`、`plan`、`acceptEdits`、`dontAsk` 或 `bypassPermissions` |
| `agent_id` | string | 唯一代理标识符。当钩子在代理上下文内触发时出现 (自v2.1.69) |
| `agent_type` | string | 代理类型名称 (例如 `Bash`、`Explore`、`Plan` 或自定义)。使用 `--agent <name>` 标志或在代理内时出现 (自v2.1.69) |

> **注意**: 钩子特定字段 (例如 PreToolUse的 `tool_name`、Stop的 `last_assistant_message`) 在上述 [钩子事件概览](#钩子事件概览---官方22个钩子) 表的"选项"列中列出。

## 钩子管理命令

Claude Code提供用于管理钩子的内置命令：

- **`/hooks`** — 交互式钩子管理UI。查看、添加和删除钩子而无需编辑JSON文件。钩子按来源标记：`[User]`、`[Project]`、`[Local]`、`[Plugin]`。您还可以从此菜单切换 `disableAllHooks`。
- **`claude hooks reload`** — 重新加载钩子配置而无需重启会话。在编辑设置文件后很有用 (自v2.0.47)。

## MCP工具匹配器

对于 `PreToolUse`、`PostToolUse` 和 `PermissionRequest` 钩子，您可以使用模式 `mcp__<server>__<tool>` 匹配MCP (模型上下文协议) 工具：

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "mcp__memory__.*",
      "hooks": [{ "type": "command", "command": "echo '使用了MCP记忆工具'" }]
    }]
  }
}
```

完整正则表达式支持: `mcp__memory__.*` (来自记忆服务器的所有工具), `mcp__.*__write.*` (来自任何服务器的任何写工具)。

### 按钩子的匹配器参考

匹配器过滤哪些事件触发钩子。不是所有钩子都支持匹配器 — 不支持匹配器的钩子总是触发。

| 钩子 | 匹配器字段 | 可能值 | 示例 |
|------|--------------|-----------------|---------|
| `PreToolUse` | `tool_name` | 任何工具名称: `Bash`、`Read`、`Edit`、`Write`、`Glob`、`Grep`、`mcp__*` | `"matcher": "Bash"` |
| `PermissionRequest` | `tool_name` | 与PreToolUse相同 | `"matcher": "mcp__memory__.*"` |
| `PostToolUse` | `tool_name` | 与PreToolUse相同 | `"matcher": "Write"` |
| `PostToolUseFailure` | `tool_name` | 与PreToolUse相同 | `"matcher": "Bash"` |
| `Notification` | `notification_type` | `permission_prompt`、`idle_prompt`、`auth_success`、`elicitation_dialog` | `"matcher": "permission_prompt"` |
| `SubagentStart` | `agent_type` | `Bash`、`Explore`、`Plan` 或自定义代理名称 | `"matcher": "Bash"` |
| `SubagentStop` | `agent_type` | `Bash`、`Explore`、`Plan` 或自定义代理名称 | `"matcher": "Bash"` |
| `SessionStart` | `source` | `startup`、`resume`、`clear`、`compact` | `"matcher": "startup"` |
| `SessionEnd` | `reason` | `clear`、`logout`、`prompt_input_exit`、`bypass_permissions_disabled`、`other` | `"matcher": "logout"` |
| `PreCompact` | `trigger` | `manual`、`auto` | `"matcher": "auto"` |
| `PostCompact` | `trigger` | `manual`、`auto` | `"matcher": "manual"` |
| `Elicitation` | `mcp_server_name` | MCP服务器名称 | `"matcher": "my-mcp-server"` |
| `ElicitationResult` | `mcp_server_name` | MCP服务器名称 | `"matcher": "my-mcp-server"` |
| `ConfigChange` | `source` | `user_settings`、`project_settings`、`local_settings`、`policy_settings`、`skills` | `"matcher": "project_settings"` |
| `UserPromptSubmit` | — | 不支持匹配器 | 始终触发 |
| `Stop` | — | 不支持匹配器 | 始终触发 |
| `TeammateIdle` | — | 不支持匹配器 | 始终触发 |
| `TaskCompleted` | — | 不支持匹配器 | 始终触发 |
| `WorktreeCreate` | — | 不支持匹配器 | 始终触发 |
| `WorktreeRemove` | — | 不支持匹配器 | 始终触发 |
| `InstructionsLoaded` | — | 不支持匹配器 | 始终触发 |
| `Setup` | — | 不支持匹配器 | 始终触发 |

## 已知问题 & 解决方案

### 代理停止钩子错误 (SubagentStop 与 Stop)

**错误报告**: [GitHub Issue #19220](https://github.com/anthropics/claude-code/issues/19220)

**问题**: 在代理前置中定义 `Stop` 钩子时，传递到钩子脚本的 `hook_event_name` 是 `"SubagentStop"` 而不是 `"Stop"`。这与官方文档矛盾并破坏与其他代理钩子 (`PreToolUse` 和 `PostToolUse`) 的一致性，这些正确传递其配置的名称。

| 钩子 | 定义为 | 接收为 | 状态 |
|------|------------|-------------|--------|
| PreToolUse | `PreToolUse:` | `"PreToolUse"` | ✅ 正确 |
| PostToolUse | `PostToolUse:` | `"PostToolUse"` | ✅ 正确 |
| Stop | `Stop:` | `"SubagentStop"` | ❌ 不一致 |

**状态**: [官方钩子参考](https://code.claude.com/docs/en/hooks#hooks-in-skills-and-agents) 现在将此记录为预期行为：*"对于代理，Stop钩子自动转换为SubagentStop，因为那是代理完成时触发的事件。"* 此项目通过 `hooks.py` 中的 `AGENT_HOOK_SOUND_MAP` 处理它，该文件有一个单独的 `SubagentStop` 条目映射到 `agent_subagentstop` 声音文件夹。

### PreToolUse决定控制弃用

`PreToolUse` 钩子先前使用顶级 `decision` 和 `reason` 字段用于阻止工具调用。这些现在已**弃用**。改用 `hookSpecificOutput.permissionDecision` 和 `hookSpecificOutput.permissionDecisionReason`：

| 弃用 | 当前 |
|-----------|---------|
| `"decision": "approve"` | `"hookSpecificOutput": { "permissionDecision": "allow" }` |
| `"decision": "block"` | `"hookSpecificOutput": { "permissionDecision": "deny" }` |

这不影响此项目因为 `hooks.py` 使用异步声音播放并不使用决定控制。

## 决定控制模式

不同的钩子使用不同的输出模式用于阻止或控制执行。此项目不使用决定控制 (所有钩子是异步声音播放)，但供参考：

| 钩子 | 控制方法 | 值 |
|---------|---------------|--------|
| PreToolUse | `hookSpecificOutput.permissionDecision` | `allow`、`deny`、`ask` |
| PreToolUse | `hookSpecificOutput.autoAllow` | `true` — 自动批准此工具的未来使用 (自v2.0.76) |
| PermissionRequest | `hookSpecificOutput.decision.behavior` | `allow`、`deny` |
| PostToolUse、PostToolUseFailure、Stop、SubagentStop、ConfigChange | 顶级 `decision` | `block` |
| TeammateIdle、TaskCompleted | `continue` + 退出代码 2 | `{"continue": false, "stopReason": "..."}` — JSON决定控制添加于v2.1.70 |
| UserPromptSubmit | 可修改 `prompt` 字段 | 通过stdout返回修改的提示 |
| WorktreeCreate | 非零退出 + stdout路径 | 非零退出失败创建；stdout提供工作树路径 |
| Elicitation | `hookSpecificOutput.action` + `hookSpecificOutput.content` | `accept`、`decline`、`cancel` — 控制MCP询问响应 |
| ElicitationResult | `hookSpecificOutput.action` + `hookSpecificOutput.content` | `accept`、`decline`、`cancel` — 在发送到服务器前覆盖用户响应 |

### 通用JSON输出字段

所有钩子可通过stdout JSON返回这些字段：

| 字段 | 类型 | 描述 |
|-------|------|-------------|
| `continue` | bool | 如果 `false`，完全停止Claude |
| `stopReason` | string | `continue` 为false时显示的消息 |
| `suppressOutput` | bool | 在详细模式中隐藏stdout |
| `systemMessage` | string | 显示给用户的警告消息 |
| `additionalContext` | string | 添加到Claude对话的上下文 |

## 钩子重复数据删除和外部更改

- **钩子重复数据删除**: 在多个设置位置中定义的相同钩子处理程序并行运行仅一次，防止重复执行。
- **外部更改检测**: Claude Code在活跃会话中钩子被外部修改 (例如另一个进程编辑设置文件) 时发出警告。
