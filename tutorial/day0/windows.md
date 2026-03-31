# Windows设置

[返回第0天](README.md)

---

**Node.js**
- 转到[nodejs.org](https://nodejs.org)
- 点击**"Download Node.js (LTS)"**按钮 — 这会下载`.msi`安装程序
- 运行`.msi`文件，通过向导单击**Next**
- 接受默认值，点击**Install**，等待完成

**验证Node.js**
- 打开**新**终端（PowerShell或Windows Terminal）并运行：
  ```powershell
  node --version
  npm --version
  ```

**Claude Code**
- ```powershell
  npm install -g @anthropic-ai/claude-code
  ```
- 如果收到权限错误，以**管理员**身份运行您的终端（右键单击 > 以管理员身份运行）

**验证**
- ```powershell
  claude --version
  ```

---

现在回到[README.md](README.md)进行身份验证设置。
