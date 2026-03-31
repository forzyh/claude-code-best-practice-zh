# 第 0 天 — Claude Code设置

本指南将指导您在计算机上安装Claude Code并进行身份验证，以便您可以开始使用它。

## 步骤 1: 安装Claude Code

选择您的操作系统：

| 操作系统 | 指南 |
|----|-------|
| Windows | [windows.md](windows.md) |
| Linux | [linux.md](linux.md) |
| macOS | [mac.md](mac.md) |

按照您的操作系统指南进行操作，然后回到这里进行身份验证。

---

## 步骤 2: 验证安装

按照您的操作系统特定指南后，确认一切正常：

```bash
node --version    # 应显示 v18.x 或更高版本
claude --version  # 应显示安装的Claude Code版本
```

---

## 步骤 3: 登录

<img src="assets/login.png" alt="Claude Code登录屏幕" width="50%">

在终端中运行`claude`。首次启动时，它会要求您选择登录方法。

### 方法 1: 订阅（Claude Pro / Max）

- 选择**Claude.ai账户**
- 浏览器打开 — 登录并授权
- 返回终端，您已登录

### 方法 2a: API密钥（团队邀请）

您的团队管理员从Anthropic仪表板邀请您。

- 您会收到一封**邀请邮件** — 接受并创建您的Anthropic账户
- 在终端中运行`claude`
- 选择**Anthropic API密钥**
- 您的密钥在仪表板上**自动生成** — 无需手动设置
- Claude Code立即开始工作

### 方法 2b: API密钥（您有密钥）

如果有人与您共享密钥（通过Slack、电子邮件等）或您创建了自己的密钥：

- 在终端中运行`claude`
- 选择**Anthropic API密钥**
- 粘贴您的密钥（以`sk-ant-`开头）
- 密钥**永久存储** — 不会再次要求您

---