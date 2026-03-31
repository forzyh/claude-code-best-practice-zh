# Claude 高级工具使用模式

API 级别特性（现已正式发布），可降低令牌消耗和延迟，提高工具精准度。随 Opus/Sonnet 4.6 一起发布。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [概述](#概述)
2. [程序化工具调用 (PTC)](#程序化工具调用-ptc)
3. [Web 搜索/获取的动态过滤](#web-搜索获取的动态过滤)
4. [工具搜索工具](#工具搜索工具)
5. [工具使用示例](#工具使用示例)
6. [Claude Code 相关性](#claude-code-相关性)

---

## 概述

| 特性 | 解决的问题 | 令牌节省 | 可用性 |
|---------|---------------|---------------|--------------|
| 程序化工具调用 | 多步骤代理循环消耗往返令牌 | ~37% 减少 | API、Foundry（正式发布） |
| 动态过滤 | Web 搜索/获取结果充斥无关内容 | ~24% 减少输入令牌 | API、Foundry（正式发布） |
| 工具搜索工具 | 过多工具定义占用上下文 | ~85% 减少 | API、Foundry（正式发布） |
| 工具使用示例 | 仅模式无法表达使用模式 | 72% → 90% 精准度 | API、Foundry（正式发布） |

所有特性自 2026 年 2 月 18 日起**正式可用**。

**战略分层** — 从最大瓶颈开始：
- 工具定义导致的上下文膨胀 → 工具搜索工具
- 大型中间结果 → 程序化工具调用
- Web 搜索噪音 → 动态过滤
- 参数错误 → 工具使用示例

---

## 程序化工具调用 (PTC)

<img src="assets/programmatic-tool-calling-diagram.svg" alt="PTC 图表 — 传统 vs 程序化工具调用" width="100%" />

### 范式转变

**之前（传统工具调用）：**
```
用户提示 → Claude → 工具调用 1 → 响应 1 → Claude → 工具调用 2 → 响应 2 → Claude → 工具调用 3 → 响应 3 → Claude → 最终答案
```
每个工具调用需要完整的模型往返。3 个工具 = 3 次推理通过。

**之后（程序化工具调用）：**
```
用户提示 → Claude → 编写 Python 脚本 → 脚本内部调用工具 1、2、3 → stdout → Claude → 最终答案
```
Claude 编写代码来编排所有工具。只有最后的 `stdout` 进入上下文窗口。3 个工具 = 1 次推理通过。

### 工作原理

1. 使用 `allowed_callers: ["code_execution_20250825"]` 定义工具
2. Claude 编写 Python 代码在沙箱内调用这些工具作为异步函数
3. 调用工具函数时，沙箱暂停，API 返回 `tool_use` 块
4. 你提供工具结果 — 它进入**运行中的代码**，而非 Claude 的上下文
5. 代码恢复，处理结果，根据需要调用更多工具
6. 只有最后执行的 `stdout` 到达 Claude

### 关键配置

```json
{
  "tools": [
    {
      "type": "code_execution_20250825",
      "name": "code_execution"
    },
    {
      "name": "query_database",
      "description": "执行 SQL 查询。返回 JSON 对象行，字段：id (str)、name (str)、revenue (float)。",
      "input_schema": {
        "type": "object",
        "properties": {
          "sql": { "type": "string", "description": "要执行的 SQL 查询" }
        },
        "required": ["sql"]
      },
      "allowed_callers": ["code_execution_20250825"]
    }
  ]
}
```

### `allowed_callers` 字段

| 值 | 行为 |
|-------|----------|
| `["direct"]` | 仅传统工具调用（如果省略则为默认） |
| `["code_execution_20250825"]` | 仅可从 Python 沙箱调用 |
| `["direct", "code_execution_20250825"]` | 两种模式可用 |

**建议：** 为每个工具选择一种模式，不要两者混用。这为 Claude 提供了更清晰的指导。

### 响应中的 `caller` 字段

每个工具使用块都包含 `caller` 字段，以便知道它是如何被调用的：

```json
// 直接（传统）
{ "caller": { "type": "direct" } }

// 程序化（来自代码执行）
{ "caller": { "type": "code_execution_20250825", "tool_id": "srvtoolu_abc123" } }
```

### 高级模式

**批处理** — 在 1 次推理通过中处理 N 个项：
```python
regions = ["West", "East", "Central", "North", "South"]
results = {}
for region in regions:
    data = await query_database(f"SELECT SUM(revenue) FROM sales WHERE region='{region}'")
    results[region] = data[0]["revenue"]

top = max(results.items(), key=lambda x: x[1])
print(f"Top region: {top[0]} with ${top[1]:,}")
```

**早期终止** — 一旦满足成功条件即停止：
```python
endpoints = ["us-east", "eu-west", "apac"]
for endpoint in endpoints:
    status = await check_health(endpoint)
    if status == "healthy":
        print(f"Found healthy endpoint: {endpoint}")
        break
```

**条件工具选择：**
```python
file_info = await get_file_info(path)
if file_info["size"] < 10000:
    content = await read_full_file(path)
else:
    content = await read_file_summary(path)
print(content)
```

**数据过滤** — 减少 Claude 看到的内容：
```python
logs = await fetch_logs(server_id)
errors = [log for log in logs if "ERROR" in log]
print(f"Found {len(errors)} errors")
for error in errors[-10:]:
    print(error)
```

### 模型兼容性

| 模型 | 支持 |
|-------|-----------|
| Claude Opus 4.6 | 是 |
| Claude Sonnet 4.6 | 是 |
| Claude Sonnet 4.5 | 是 |
| Claude Opus 4.5 | 是 |

### 约束

| 约束 | 详情 |
|-----------|--------|
| **不在 Bedrock/Vertex 上** | 仅 API 和 Foundry |
| **无 MCP 工具** | MCP 连接器工具无法以程序化方式调用 |
| **无 Web 搜索/获取** | PTC 中不支持 Web 工具 |
| **无结构化输出** | `strict: true` 工具不兼容 |
| **无强制工具选择** | `tool_choice` 无法强制 PTC |
| **容器生命周期** | ~4.5 分钟后过期 |
| **ZDR** | 零数据保留未涵盖 |
| **工具结果作为字符串** | 验证外部结果的代码注入风险 |

### 何时使用 PTC

| 良好用途 | 不太理想 |
|----------------|------------|
| 处理需要聚合的大型数据集 | 具有简单响应的单个工具调用 |
| 序列中的 3+ 个依赖工具调用 | 需要立即用户反馈的工具 |
| 过滤/转换结果后再让 Claude 看到 | 非常快的操作（开销 > 收益） |
| 跨多个项的并行操作 | |
| 基于中间结果的条件逻辑 | |

### 令牌效率

- 程序化调用的工具结果**不会添加到 Claude 的上下文** — 仅最终 `stdout`
- 中间处理在代码中进行，不使用模型令牌
- 10 个工具以程序化方式 ≈ 1/10 的 10 个直接调用令牌

---

## Web 搜索/获取的动态过滤

### 问题

Web 搜索和获取工具将完整 HTML 页面转储到 Claude 的上下文窗口。这些内容中的大部分是无关的 — 导航、广告、样板。Claude 然后对所有内容进行推理，浪费令牌并降低精准度。

### 解决方案

Claude 现在**编写并执行 Python 代码来在结果进入上下文窗口之前过滤 Web 结果**。Claude 不是推理原始 HTML，而是在沙箱中过滤、解析和提取仅相关内容。

### 工作原理

**之前：**
```
查询 → 搜索结果 → 获取完整 HTML × N 页 → 所有内容进入上下文 → Claude 推理一切
```

**之后：**
```
查询 → 搜索结果 → Claude 编写过滤代码 → 代码仅提取相关内容 → 过滤后的结果进入上下文
```

### API 配置

使用 beta 标头的更新工具类型版本：

```json
{
  "model": "claude-opus-4-6",
  "max_tokens": 4096,
  "tools": [
    {
      "type": "web_search_20260209",
      "name": "web_search"
    },
    {
      "type": "web_fetch_20260209",
      "name": "web_fetch"
    }
  ]
}
```

**所需标头：** `anthropic-beta: code-execution-web-tools-2026-02-09`

**默认启用**，当将新工具类型版本与 Sonnet 4.6 和 Opus 4.6 一起使用时。

### 基准结果

**BrowseComp**（在网站上查找特定信息）：

| 模型 | 无过滤 | 有过滤 | 改进 |
|-------|-------------------|----------------|-------------|
| Sonnet 4.6 | 33.3% | **46.6%** | +13.3 pp |
| Opus 4.6 | 45.3% | **61.6%** | +16.3 pp |

**DeepsearchQA**（多步骤研究，F1 分数）：

| 模型 | 无过滤 | 有过滤 | 改进 |
|-------|-------------------|----------------|-------------|
| Sonnet 4.6 | 52.6% | **59.4%** | +6.8 pp |
| Opus 4.6 | 69.8% | **77.3%** | +7.5 pp |

**令牌效率：** 平均减少 24% 输入令牌。Sonnet 4.6 看到成本减少；Opus 4.6 由于更复杂的过滤代码可能略微增加。

### 用途

- 筛选技术文档
- 验证多个来源的引文
- 交叉参考搜索结果
- 多步骤研究查询
- 查找大型页面中隐埋的特定数据点

---

## 工具搜索工具

### 问题

预先加载所有工具定义浪费上下文。如果你有 50 个 MCP 工具，每个约 1.5K 令牌，那就是 75K 令牌，用户还没有提问。

### 解决方案

用 `defer_loading: true` 标记不常使用的工具。它们从初始上下文中排除。Claude 通过工具搜索工具按需发现它们。

### 配置

```json
{
  "tools": [
    {
      "type": "mcp_toolset",
      "mcp_server_name": "google-drive",
      "default_config": { "defer_loading": true },
      "configs": {
        "search_files": { "defer_loading": false }
      }
    }
  ]
}
```

### 最佳实践

- 保持 3-5 个最常用的工具始终加载，推迟其余的
- 编写清晰、描述性的工具名称和描述（搜索依赖它们）
- 在系统提示中记录可用功能

### 何时使用

- 工具定义消耗 > 10K 令牌
- 10+ 个可用工具
- 多个 MCP 服务器
- 由于选项太多导致工具选择精准度问题

### 令牌节省

~85% 减少工具定义令牌（77K → 8.7K 在 Anthropic 的基准中）。

### Claude Code 等效

Claude Code 具有**MCP 工具搜索自动模式**（自 v2.1.7 起默认启用）。当 MCP 工具描述超过上下文 10% 时，它们被推迟并通过 `MCPSearch` 发现。使用 `ENABLE_TOOL_SEARCH=auto:N` 配置阈值，其中 N 是上下文百分比（0-100）。

---

## 工具使用示例

### 问题

JSON 模式定义结构但无法表达：
- 何时包含可选参数
- 哪些参数组合有意义
- 格式约定（日期格式、ID 模式）
- 嵌套结构使用

### 解决方案

向工具定义添加 `input_examples` — 超越模式的具体使用模式。

### 配置

```json
{
  "name": "create_ticket",
  "description": "创建支持工单",
  "input_schema": {
    "type": "object",
    "properties": {
      "title": { "type": "string" },
      "priority": { "type": "string", "enum": ["low", "medium", "high", "critical"] },
      "assignee": { "type": "string" },
      "labels": { "type": "array", "items": { "type": "string" } }
    },
    "required": ["title"]
  },
  "input_examples": [
    {
      "title": "登录页面返回 500 错误",
      "priority": "critical",
      "assignee": "oncall-team",
      "labels": ["bug", "auth", "production"]
    },
    {
      "title": "添加深色模式支持",
      "priority": "low",
      "labels": ["feature-request", "ui"]
    },
    {
      "title": "更新 v2 端点的 API 文档"
    }
  ]
}
```

### 最佳实践

- 使用**真实数据**，不是"example_value"之类的占位符字符串
- 显示**多样性**：最小、部分和完整规范
- 保持简洁：**每个工具 1-5 个示例**
- 专注于解决歧义 — 针对行为清晰度而非模式完整性
- 显示参数相关性（例如 `priority: "critical"` 倾向于有 `assignee`）

### 结果

Anthropic 基准中复杂参数处理精准度 72% → 90%。

---

## Claude Code 相关性

### 直接适用于 Claude Code 用户的内容

| 特性 | Claude Code 状态 | 行动 |
|---------|-------------------|--------|
| 工具搜索 | 自 v2.1.7 起作为 MCPSearch 自动模式内置 | 如果有许多 MCP 工具，调整 `ENABLE_TOOL_SEARCH=auto:N` |
| 动态过滤 | CLI 中不可用（API 级别 Web 工具） | 与进行 Web 研究的代理 SDK 用户相关 |
| PTC | CLI 中不可用 | 与构建自定义代理的代理 SDK 用户相关 |
| 工具使用示例 | CLI 中不可配置 | 与自定义 MCP 服务器作者相关 |

### 对于代理 SDK 开发者

如果你使用 `@anthropic-ai/claude-agent-sdk` 构建代理，PTC 立即可行：

1. 将 `code_execution_20250825` 添加到工具数组
2. 在受益于批处理/过滤的工具上设置 `allowed_callers`
3. 实现工具结果循环（暂停 → 提供结果 → 恢复）
4. 从工具返回结构化数据（JSON）以便更轻松地进行程序化解析

### 对于 MCP 服务器作者

如果你构建自定义 MCP 服务器，工具使用示例可以改进 Claude 如何使用你的工具：
- 向工具模式添加 `input_examples`
- 在描述中清晰记录返回格式（PTC 需要解析它们）

---

## 来源

- [Anthropic 工程：高级工具使用](https://www.anthropic.com/engineering/advanced-tool-use)
- [程序化工具调用文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)
- [代码执行工具文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)
- [改进的 Web 搜索与动态过滤](https://claude.com/blog/improved-web-search-with-dynamic-filtering)
