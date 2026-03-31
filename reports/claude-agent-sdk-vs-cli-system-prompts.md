# Claude 代理 SDK vs Claude CLI：系统提示和输出一致性

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

![SDK vs CLI 系统提示图](assets/sdk-vs-cli-diagram.svg)

---

## 执行摘要

通过 **Claude 代理 SDK** 与 **Claude CLI（Claude Code）** 发送相同消息（例如"挪威的首都是什么？"）时，这些消息附随的系统提示从根本上不同。CLI 使用**模块化系统提示架构**（~269 个基础令牌，根据特性条件加载额外上下文），而 SDK 默认使用最小提示。**即使配置匹配，也无法保证相同输出**，由于没有种子参数且 Claude 架构固有的非确定性。

---

## 1. 系统提示比较

### Claude CLI（Claude Code）

Claude CLI 使用**模块化系统提示架构**，带有 ~269 令牌基础提示，条件加载额外上下文：

| 组件 | 描述 | 加载 |
|-----------|-------------|---------|
| **基础系统提示** | 核心说明和行为 | 始终（~269 令牌） |
| **工具说明** | 18+ 内置工具（Write、Read、Edit、Bash、TodoWrite 等） | 始终 |
| **代码指南** | 代码样式、格式化规则、安全实践 | 始终 |
| **安全规则** | 拒绝规则、注入防御、伤害防止 | 始终 |
| **响应样式** | 语气、冗长度、解释深度、表情符号使用 | 始终 |
| **环境上下文** | 工作目录、git 状态、平台信息 | 始终 |
| **项目上下文** | CLAUDE.md 内容、设置、钩子配置 | 条件 |
| **子代理提示** | 计划模式、探索代理、任务代理 | 条件 |
| **安全审查** | 扩展安全说明（~2,610 令牌） | 条件 |

**关键特征：**
- **模块化架构**，带 110+ 条件加载的系统提示字符串
- 基础提示适度（~269 令牌），总计因启用的特性而异
- 包括广泛的安全和注入防御层
- 自动在工作目录中加载 CLAUDE.md 文件
- 交互模式中的会话持久上下文

### Claude 代理 SDK

代理 SDK 默认使用**最小系统提示**，包含：

| 组件 | 描述 | 令牌影响 |
|-----------|-------------|--------------|
| **必需工具说明** | 仅显式提供的工具 | 最小 |
| **基本安全** | 最小安全说明 | 最小 |

**关键特征：**
- 默认无代码指南或样式偏好
- 除非明确配置否则无项目上下文
- 无广泛的工具描述
- 需要明确配置以匹配 CLI 行为

---

## 2. 每个接口发送的内容

### 示例："挪威的首都是什么？"

#### 通过 Claude CLI

```
系统提示：[模块化，~269+ 基础令牌]
├── 基础系统提示（~269 令牌）
├── 工具说明（Write、Read、Edit、Bash、Grep、Glob 等）
├── Git 安全协议
├── 代码参考指南
├── 专业客观说明
├── 安全和注入防御规则
├── 环境上下文（OS、目录、日期）
├── CLAUDE.md 内容（如果存在）[条件]
├── MCP 工具描述（如果配置）[条件]
├── 计划/探索模式提示 [条件]
└── 会话/对话上下文

用户消息："挪威的首都是什么？"
```

#### 通过 Claude 代理 SDK（默认）

```
系统提示：[最小]
├── 必需工具说明（如果提供任何工具）
└── 基本操作上下文

用户消息："挪威的首都是什么？"
```

#### 通过代理 SDK（使用 `claude_code` 预设）

```typescript
const response = await query({
  prompt: "挪威的首都是什么？",
  options: {
    systemPrompt: {
      type: "preset",
      preset: "claude_code"
    }
  }
});
```

```
系统提示：[模块化，匹配 CLI]
├── 完整 Claude Code 系统提示
├── 工具说明
├── 代码指南
└── 安全规则

// 注意：仍然不包括 CLAUDE.md，除非配置了 settingSources
```

---

## 3. 自定义方法

### Claude CLI 自定义

| 方法 | 命令 | 效果 |
|--------|---------|--------|
| **追加到提示** | `claude -p "..." --append-system-prompt "..."` | 在保留默认值的同时添加说明 |
| **替换提示** | `claude -p "..." --system-prompt "..."` | 完全替换系统提示 |
| **项目上下文** | CLAUDE.md 文件 | 自动加载，持久 |
| **输出样式** | `/output-style [name]` | 应用预定义响应样式 |

### 代理 SDK 自定义

| 方法 | 配置 | 效果 |
|--------|---------------|--------|
| **自定义提示** | `systemPrompt: "..."` | 完全替换默认值（丧失工具） |
| **预设与追加** | `systemPrompt: { type: "preset", preset: "claude_code", append: "..." }` | 保留 CLI 功能 + 自定义说明 |
| **CLAUDE.md 加载** | `settingSources: ["project"]` | 加载项目级说明 |
| **输出样式** | `settingSources: ["user"]` 或 `settingSources: ["project"]` | 加载保存的输出样式 |

### 配置比较表

| 特性 | CLI 默认 | SDK 默认 | SDK 预设 |
|---------|-------------|-------------|-----------------|
| 工具说明 | ✅ 完整 | ❌ 最小 | ✅ 完整 |
| 代码指南 | ✅ 是 | ❌ 否 | ✅ 是 |
| 安全规则 | ✅ 是 | ❌ 基本 | ✅ 是 |
| CLAUDE.md 自动加载 | ✅ 是 | ❌ 否 | ❌ 否* |
| 项目上下文 | ✅ 自动 | ❌ 否 | ❌ 否* |

*需要显式 `settingSources: ["project"]` 配置

---

## 4. 输出一致性保证

### 关键发现：不保证确定性

**Claude 消息 API 不提供用于重现的种子参数。** 这是一个根本架构限制。

### 防止相同输出的因素

| 因素 | 描述 | 可控吗？ |
|--------|-------------|--------------|
| **不同系统提示** | CLI vs SDK 默认不同 | ✅ 是（通过配置） |
| **浮点算术** | 并行硬件怪癖 | ❌ 否 |
| **MoE 路由** | 混合专家架构变化 | ❌ 否 |
| **批处理/调度** | 云基础设施差异 | ❌ 否 |
| **数值精度** | 推理引擎变化 | ❌ 否 |
| **模型快照** | 版本更新/更改 | ❌ 否 |

### 温度和采样

即使 `temperature=0.0`（贪心解码）：
- 完整确定性**不保证**
- 由于基础设施因素仍可能发生细微变化
- 已知错误：[Claude CLI 对相同输入产生非确定性输出](https://github.com/anthropics/claude-code/issues/3370)

---

## 5. 实现最大一致性

为了获得 SDK 和 CLI 之间**最接近的**相同输出：

### 代理 SDK 配置

```typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();

// 选项 1：使用 claude_code 预设
const response = await client.messages.create({
  model: "claude-sonnet-4-20250514",
  max_tokens: 1024,
  // 尽可能匹配 CLI 系统提示
  system: "你的与 CLI 匹配的确切系统提示",
  messages: [
    { role: "user", content: "挪威的首都是什么？" }
  ],
  // 使用贪心解码获得最大一致性
  temperature: 0
});

// 选项 2：使用代理 SDK 查询函数
import { query } from "@anthropic-ai/agent-sdk";

for await (const message of query({
  prompt: "挪威的首都是什么？",
  options: {
    systemPrompt: {
      type: "preset",
      preset: "claude_code"
    },
    temperature: 0,
    model: "claude-sonnet-4-20250514",
    // 像 CLI 一样加载项目上下文
    settingSources: ["project"]
  }
})) {
  // 处理响应
}
```

### CLI 配置

```bash
# 尽可能匹配 SDK 配置
claude -p "挪威的首都是什么？" \
  --model claude-sonnet-4-20250514 \
  --temperature 0
```

### 仍不保证

即使配置完全匹配：
- 输出可能在运行之间不同
- 输出可能在 SDK 和 CLI 之间不同
- 无种子参数存在以强制重现性

---

## 6. 实际含义

### 何时使用各种接口

| 用途 | 推荐接口 | 原因 |
|----------|----------------------|--------|
| 交互式开发 | Claude CLI | 完整工具套件、项目上下文 |
| 程序化集成 | 代理 SDK | 细粒度控制、嵌入 |
| 一致 API 响应 | 代理 SDK + 自定义提示 | 更多系统提示控制 |
| 批处理 | 代理 SDK | 更适合自动化管道 |
| 一次性任务 | Claude CLI | 更快设置、即时上下文 |

### 设计建议

1. **不依赖完全可重现性**
   - 构建对轻微输出变化稳健的应用程序
   - 使用结构化输出和验证

2. **对于需要一致性的生产管道：**
   - 尽可能缓存结果
   - 使用带 JSON 模式验证的结构化输出
   - 结合确定性逻辑和验证层
   - 考虑用共识进行多次生成

3. **为了在 SDK 中匹配 CLI 行为：**
   ```typescript
   systemPrompt: {
     type: "preset",
     preset: "claude_code",
     append: "你的额外说明"
   },
   settingSources: ["project", "user"]
   ```

---

## 7. 系统提示令牌影响

| 配置 | 架构 | 注释 |
|---------------|-------------|-------|
| SDK（最小） | 最小默认 | 仅基本工具说明 |
| SDK（claude_code 预设） | 模块化（~269+ 基础） | 匹配 CLI，因特性而异 |
| CLI（默认） | 模块化（~269+ 基础） | 条件加载额外上下文 |
| CLI（带 MCP 工具） | 模块化 + MCP | MCP 工具描述添加显著令牌 |

**注意：** Claude Code 使用模块化架构，110+ 系统提示字符串。基础提示 ~269 令牌，个别组件范围从 18 到 2,610 令牌，取决于启用的特性。

**含义：** SDK 的最小默认为你的实际任务提供更多上下文，但代价是失去 Claude Code 的完整功能。

---

## 8. 总结表

| 方面 | Claude CLI | 代理 SDK（默认） | 代理 SDK（预设） |
|--------|------------|--------------------|--------------------|
| **系统提示** | 模块化（~269+ 基础） | 最小 | 模块化（匹配 CLI） |
| **包含的工具** | 18+ 内置 | 仅所提供 | 18+ 内置 |
| **CLAUDE.md 自动加载** | 是 | 否 | 否（需要配置） |
| **代码指南** | 是 | 否 | 是 |
| **安全规则** | 完整 | 基本 | 完整 |
| **温度控制** | 是 | 是 | 是 |
| **确定性保证** | 否 | 否 | 否 |
| **相同输出?** | N/A | 否（vs CLI） | 更接近，但否 |

---

## 9. 结论

**问：SDK vs CLI 中同一消息附随的系统提示是什么？**

CLI 使用**模块化系统提示架构**，~269 令牌基础提示和 110+ 条件加载组件（工具说明、代码指南、安全规则、项目上下文）。SDK 使用**最小默认值**，仅有必要工具说明，尽管可配置为使用 `claude_code` 预设匹配 CLI 行为。

**问：是否保证相同输出？**

**否。** 即使具有匹配的系统提示、相同输入和 `temperature=0`，由于以下原因，不保证相同输出：
- Claude 的 API 中没有种子参数
- 浮点算术变化
- 基础设施级非确定性
- 模型架构（混合专家）路由变化

**建议：** 设计系统以对输出变化稳健，而非依赖确定性行为。对于一致性关键应用，使用结构化输出、缓存和验证层。

---

## 来源

- [修改系统提示 - 代理 SDK](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/sdk#modifying-system-prompts)
- [Claude Code CLI 参考](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/cli)
- [Claude Code 无头模式](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/headless)
- [Claude Code 最佳实践 - Anthropic 工程](https://www.anthropic.com/engineering/claude-code-best-practices)
- [Claude 消息 API 参考](https://docs.anthropic.com/en/api/messages)
- [GitHub 问题 #3370：非确定性输出](https://github.com/anthropics/claude-code/issues/3370)
- [Claude Code 系统提示存储库](https://github.com/Piebald-AI/claude-code-system-prompts) - 模块化提示架构分析
- [为什么 LLM 的确定性输出几乎不可能](https://unstract.com/blog/understanding-why-deterministic-output-from-llms-is-nearly-impossible/)

---

*此报告由 Claude Code 使用 Opus 4.5 模型在 2026 年 2 月 3 日生成。*
