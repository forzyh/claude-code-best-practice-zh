# Linux设置

[返回第0天](README.md)

## 前置条件

您需要**Node.js v18或更高版本**和**npm**。

## 步骤 1: 安装Node.js

### 选项A：通过nodejs.org下载页面使用fnm（推荐）

**fnm**（Fast Node Manager）得到Node.js官方推荐。它快速、轻量级，如果需要，可以轻松切换Node版本。

1. 打开浏览器，转到[nodejs.org/en/download](https://nodejs.org/en/download)。

2. 您将看到一行下拉菜单，显示：**"Get Node.js® vXX.XX.X (LTS) for __ using __ with __"**。按如下设置下拉菜单：

   | 下拉菜单 | 选择 |
   |----------|--------|
   | 版本 | **vXX.XX.X (LTS)** — 保持默认LTS版本，不要更改 |
   | 操作系统 | **Linux** |
   | 包管理器 | **fnm**（在"Recommended (Official)"下） |
   | 包格式 | **npm** — 保持默认 |

3. 页面将显示要运行的确切命令。打开终端并复制粘贴它们。它们看起来像这样：

   ```bash
   # 步骤 1 — 安装fnm
   curl -fsSL https://fnm.vercel.app/install | bash

   # 步骤 2 — 重启终端或重新加载shell配置文件
   source ~/.bashrc   # 或: source ~/.zshrc (如果您使用zsh)

   # 步骤 3 — 安装Node.js
   fnm install 24   # 页面将显示确切的版本号
   ```

   > 版本号可能与上面不同 — 始终使用网站显示的版本。

4. **关闭并重新打开终端**（或运行上面的`source`命令），以便`fnm`、`node`和`npm`可用。

> **为什么选fnm？** 它在Node.js下载页面的"Recommended (Official)"类别中。与nvm一样，它将Node安装到您的主目录中，因此您永远不需要为npm全局安装使用`sudo` — 但fnm明显更快（用Rust编写），并且在Windows、macOS和Linux上工作相同。

### 选项B：使用您的发行版的包管理器

这更快，但可能会安装较旧版本的Node.js。**安装后检查版本** — 如果低于v18，请改为选项A。

**Ubuntu / Debian:**

```bash
sudo apt update
sudo apt install -y nodejs npm

# 检查版本
node --version   # 必须是 v18 或更高版本
```

**Fedora:**

```bash
sudo dnf install -y nodejs npm
```

**Arch Linux:**

```bash
sudo pacman -S nodejs npm
```

### 选项C：NodeSource（通过apt获取最新LTS，无nvm）

对于想要最新LTS但不使用nvm的Ubuntu/Debian用户：

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

## 步骤 2: 验证Node.js

```bash
node --version
npm --version
```

两者都应打印版本号。`node --version`必须显示v18.x或更高版本。

## 步骤 3: 安装Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

> **权限错误？**
> - 如果您使用**fnm**或**nvm**：这不应该发生。检查它是否活跃（`which node`应该指向主目录内的路径，而不是`/usr/...`）。
> - 如果您使用系统安装：要么使用`sudo npm install -g @anthropic-ai/claude-code`，要么修复npm的全局目录权限：
>   ```bash
>   mkdir -p ~/.npm-global
>   npm config set prefix '~/.npm-global'
>   echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
>   source ~/.bashrc
>   ```

## 步骤 4: 验证Claude Code

```bash
claude --version
```

您应该看到打印的Claude Code版本。现在回到[README.md](README.md)进行身份验证设置。

---

## 注意事项

- **WSL（Windows Subsystem for Linux）：** 本指南也适用于WSL内部。只需从您的WSL终端执行这些步骤。
- **PATH问题：** 如果安装后找不到`claude`，请确保npm的全局bin在您的PATH中。运行`npm config get prefix` — 该路径的`bin/`子目录需要在您的PATH中。
