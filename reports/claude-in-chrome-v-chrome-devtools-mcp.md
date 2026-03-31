# 综合浏览器自动化 MCP 比较报告

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 执行摘要

基于广泛研究，我分析了你的截图中的两个工具加上第三个主要竞争者。这是一个综合分解，帮助你为自动化测试工作选择最佳选项。

---

## 1. 三个竞争者

### **A. Chrome DevTools MCP**（你的截图 #1）
- **来源：** 官方谷歌 Chrome 团队
- **发布：** 2025 年 9 月公开预览
- **架构：** 基于 Chrome DevTools Protocol (CDP) + Puppeteer
- **令牌使用：** ~19.0k 令牌（9.5% 上下文）
- **工具：** 6 个类别中 26 个专门工具

### **B. Chrome 中的 Claude**（你的截图 #2）
- **来源：** 官方 Anthropic 扩展
- **发布：** Beta，向所有付费计划推出（Pro、Max、Team、Enterprise）
- **架构：** 具有计算机使用功能的浏览器扩展
- **令牌使用：** ~15.4k 令牌（7.7% 上下文）
- **工具：** 16 个工具，包括计算机使用功能

### **C. Playwright MCP**（强大替代方案）
- **来源：** 微软（官方 + 社区实现）
- **架构：** 基于可访问性树的自动化
- **令牌使用：** ~13.7k 令牌（6.8% 上下文）
- **工具：** 21 个工具

---

## 2. 详细特性比较

| 特性 | Chrome DevTools MCP | Chrome 中的 Claude | Playwright MCP |
|---------|---------------------|------------------|----------------|
| **主要目的** | 调试和性能 | 通用浏览器自动化 | UI 测试和 E2E |
| **浏览器支持** | 仅 Chrome | 仅 Chrome | Chromium、Firefox、WebKit |
| **令牌效率** | 19.0k（9.5%） | 15.4k（7.7%） | 13.7k（6.8%） |
| **元素选择** | CSS/XPath 选择器 | 视觉 + DOM | 可访问性树（语义） |
| **性能跟踪** | ✅ 优秀 | ❌ 否 | ⚠️ 有限 |
| **网络检查** | ✅ 深度分析 | ⚠️ 基本 | ⚠️ 基本 |
| **控制台日志** | ✅ 完整访问 | ✅ 完整访问 | ⚠️ 有限 |
| **跨浏览器** | ❌ 否 | ❌ 否 | ✅ 是 |
| **CI/CD 集成** | ✅ 优秀 | ❌ 差（需要登录） | ✅ 优秀 |
| **无头模式** | ✅ 是 | ❌ 否 | ✅ 是 |
| **身份验证** | 需要设置 | 使用你的会话 | 需要设置 |
| **计划任务** | ❌ 否 | ✅ 是 | ❌ 否 |
| **成本** | 免费 | 需要付费计划 | 免费 |
| **本地设置** | 需要 Node.js | 浏览器扩展 | 需要 Node.js |

---

## 3. 工具分解

### Chrome DevTools MCP（26 个工具）

```
输入自动化 (8):     click、drag、fill、fill_form、handle_dialog、
                  hover、press_key、upload_file

导航 (6):           close_page、list_pages、navigate_page、
                  new_page、select_page、wait_for

模拟 (2):            emulate、resize_page

性能 (3):          performance_analyze_insight、
                  performance_start_trace、performance_stop_trace

网络 (2):              get_network_request、list_network_requests

调试 (5):            evaluate_script、get_console_message、
                  list_console_messages、take_screenshot、
                  take_snapshot
```

### Chrome 中的 Claude（16 个工具）

```
浏览器控制:          navigate、read_page、find、computer
                  （click、type、scroll）

表单交互:         form_input、javascript_tool

媒体:                    upload_image、get_page_text、gif_creator

标签管理:           tabs_context_mcp、tabs_create_mcp

开发:              read_console_messages、read_network_requests

实用工具:                shortcuts_list、shortcuts_execute、
                  resize_window、update_plan
```

### Playwright MCP（21 个工具）

```
导航:               navigate、goBack、goForward、reload

交互:              click、fill、select、hover、press、
                  drag、uploadFile

元素查询:          getElement、getElements、waitForSelector

断言:               assertVisible、assertText、assertTitle

页面状态:           screenshot、getAccessibilityTree、
                  evaluateScript

浏览器管理:             newPage、closePage
```

---

## 4. 自动化测试用途分析

### **Chrome DevTools MCP 最适合：**

✅ **性能测试**
- 用 Core Web Vitals 记录性能跟踪
- 识别渲染瓶颈和布局移位
- 内存泄漏检测和 CPU 分析

✅ **深度调试**
- 网络请求检查（标头、有效负载、计时）
- 控制台错误分析和堆栈跟踪
- 实时 DOM 检查

✅ **CI/CD 管道**
- 无头执行支持
- 稳定、基于脚本的自动化
- 无身份验证状态依赖

**理想工作流：** "找出这个页面为什么很慢"或"调试这个 API 调用"

---

### **Chrome 中的 Claude 最适合：**

✅ **手动测试协助**
- 登录你的账户时测试
- 带视觉上下文的探索性测试
- 记录可重放的工作流

✅ **快速验证**
- 设计验证（将 Figma 与输出比较）
- 发现新特性
- 在开发期间读取控制台错误

✅ **重复浏览器任务**
- 计划自动化检查
- 多标签工作流管理
- 从记录的操作学习

**理想工作流：** "检查我的更改是否看起来正确"或"用我的登录测试此表单"

---

### **Playwright MCP 最适合：**

✅ **E2E 测试自动化**
- 跨浏览器测试（Chrome、Firefox、Safari）
- 生成可重用的测试脚本
- 页面对象模型生成

✅ **可靠 UI 测试**
- 可访问性树 = 无片状选择器
- 确定性交互
- 不易因 UI 更改破裂

✅ **CI/CD 集成**
- 管道无头模式
- 从自然语言生成 Playwright 测试文件
- 与测试管理工具集成

**理想工作流：** "为此用户流编写 E2E 测试"或"在浏览器间测试此"

---

## 5. 令牌效率分析

| 工具 | 令牌使用 | % 上下文 | 效率等级 |
|------|-------------|--------------|-------------------|
| Playwright MCP | ~13.7k | 6.8% | ⭐⭐⭐⭐⭐ 最佳 |
| Chrome 中的 Claude | ~15.4k | 7.7% | ⭐⭐⭐⭐ 良好 |
| Chrome DevTools MCP | ~19.0k | 9.5% | ⭐⭐⭐ 可接受 |

**影响：** 使用 200k 令牌上下文：
- Playwright 留下 186.3k 令牌用于你的工作
- Chrome 中的 Claude 留下 184.6k 令牌
- Chrome DevTools 留下 181k 令牌

Playwright 和 Chrome DevTools 之间 ~5.3k 令牌的差异对于有大量代码上下文的复杂会话可能很重要。

---

## 6. 安全考虑

### Chrome DevTools MCP
- ✅ 默认隔离浏览器配置文件
- ✅ 无云依赖
- ✅ 完整本地控制
- ⚠️ 远程调试端口安全（使用隔离配置文件）

### Chrome 中的 Claude
- ⚠️ **23.6% 攻击成功率**无缓解（带防御减至 11.2%）
- ⚠️ 使用你的实际浏览器会话（cookie 暴露风险）
- ⚠️ 被阻止访问金融/成人/盗版网站
- ⚠️ 仍在 beta，已知漏洞

### Playwright MCP
- ✅ 隔离浏览器上下文
- ✅ 无云依赖
- ✅ 成熟安全模型（Microsoft 支持）
- ✅ 可安全处理身份验证

---

## 7. 安装命令

### Chrome DevTools MCP

```bash
claude mcp add chrome-devtools npx chrome-devtools-mcp@latest
```

### Chrome 中的 Claude

```
从 Chrome Web Store 安装（需要 Pro/Max/Team/Enterprise 计划）
```

### Playwright MCP（推荐）

```bash
# 首先，安装浏览器
npx playwright install

# 然后添加到 Claude Code（用户范围 = 所有项目）
claude mcp add playwright -s user -- npx @playwright/mcp@latest
```

---

## 8. 建议

### **针对你的自动化测试工作流：**

#### 🥇 **主要工具：Playwright MCP**

**用于：** 日常 E2E 测试、跨浏览器验证、生成测试脚本

**为什么：**
- 令牌使用最低（为你的代码提供更多上下文）
- 跨浏览器支持（Chrome、Firefox、Safari）
- 可访问性树方法 = 更可靠的选择器
- 优秀的 CI/CD 集成
- 可生成实际 Playwright 测试文件
- 免费，无需订阅

#### 🥈 **辅助工具：Chrome DevTools MCP**

**用于：** 性能调试、网络分析、Core Web Vitals

**为什么：**
- 性能跟踪和调试无与伦比
- 深度网络请求检查
- 官方 Google 工具，长期支持
- 当你需要回答"为什么这很慢？"时必需

#### 🥉 **情景化：Chrome 中的 Claude**

**用于：** 登录时快速手动验证、探索性测试、设计验证

**为什么：**
- 好用于开发期间的快速视觉检查
- 可读取你的登录状态
- 有用于"这看起来对吗？"验证
- 跳过 CI/CD 或严肃测试自动化

---

## 9. 推荐设置

```bash
# 同时安装 Playwright 和 Chrome DevTools MCP
npx playwright install
claude mcp add playwright -s user -- npx @playwright/mcp@latest
claude mcp add chrome-devtools -s user -- npx chrome-devtools-mcp@latest
```

### 建议工作流

```
1. 开发      → Claude Code（终端）
2. 测试         → Playwright MCP（E2E、跨浏览器）
3. 调试        → Chrome DevTools MCP（性能、网络）
4. 验证       → Chrome 中的 Claude（快速视觉检查）
5. CI/CD        → Playwright MCP（无头、自动化）
```

---

## 10. 最终判决

| 如果你需要... | 使用此工具 |
|----------------|----------|
| 跨浏览器 E2E 测试 | **Playwright MCP** |
| 性能分析 | **Chrome DevTools MCP** |
| 网络调试 | **Chrome DevTools MCP** |
| 快速视觉验证 | **Chrome 中的 Claude** |
| CI/CD 自动化 | **Playwright MCP** |
| 测试脚本生成 | **Playwright MCP** |
| 最低令牌使用 | **Playwright MCP** |
| 登录会话测试 | **Chrome 中的 Claude** |
| 控制台日志调试 | **Chrome DevTools MCP** |

### **TL;DR 建议：**

**同时安装 Playwright MCP 和 Chrome DevTools MCP。** 将 Playwright 用作主要测试工具（令牌更高效、跨浏览器、更好用于 E2E）。当需要深度性能分析或网络调试时使用 Chrome DevTools。仅在需要登录会话的快速手动验证时使用 Chrome 中的 Claude。

---

## 来源

- [Chrome DevTools MCP - GitHub](https://github.com/ChromeDevTools/chrome-devtools-mcp)
- [Anthropic - 试行 Chrome 中的 Claude](https://claude.com/blog/claude-for-chrome)
- [Chrome 中的 Claude 帮助中心](https://support.claude.com/en/articles/12012173-getting-started-with-claude-in-chrome)
- [Playwright MCP - GitHub](https://github.com/microsoft/playwright-mcp)
- [Simon Willison - 在 Claude Code 中使用 Playwright MCP](https://til.simonwillison.net/claude-code/playwright-mcp-claude-code/)
- [Testomat.io - Playwright MCP Claude Code](https://testomat.io/blog/playwright-mcp-claude-code/)
- [MCP 集成指南 - Scrapeless](https://www.scrapeless.com/en/blog/mcp-integration-guide)
- [Chrome DevTools MCP 指南 - Vladimir Siedykh](https://vladimirsiedykh.com/blog/chrome-devtools-mcp-ai-browser-debugging-complete-guide-2025)
- [Addy Osmani - 给你的 AI 眼睛](https://addyosmani.com/blog/devtools-mcp/)

---

*此报告由 Claude Code 使用 Opus 4.5 模型在 2025 年 12 月 19 日生成。*
