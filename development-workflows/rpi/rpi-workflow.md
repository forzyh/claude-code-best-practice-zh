# RPI工作流

**RPI** = **研究** → **计划** → **实现**

一个系统化的开发工作流，在每个阶段都有验证门关。防止在不可行的功能上浪费精力，并确保全面的文档。

<table width="100%">
<tr>
<td><a href="../../">← 返回Claude Code最佳实践</a></td>
<td align="right"><img src="../../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

![RPI工作流](rpi-workflow.svg)

---

## 安装

将`.claude`文件夹（包含`agents/`和`commands/rpi/`）复制到您的存储库根目录，然后创建`rpi/plans`目录。

---

## 工作流示例

### 功能：用户身份验证

**步骤 1: 描述**
```
用户: "添加具有Google和GitHub提供商的OAuth2身份验证"

1. Claude生成计划
   → 输出: rpi/plans/oauth2-authentication.md
2. 创建功能文件夹: rpi/oauth2-authentication/
3. 将计划复制到功能文件夹中
4. 将计划重命名为REQUEST.md
   → 最终: rpi/oauth2-authentication/REQUEST.md
```

**步骤 2: 研究**
```bash
/rpi:research rpi/oauth2-authentication/REQUEST.md
```
输出:
- `research/RESEARCH.md` 包含分析
- 结论: **继续**（可行，符合策略）

**步骤 3: 计划**
```bash
/rpi:plan oauth2-authentication
```
输出:
- `plan/pm.md` - 用户故事和验收标准
- `plan/ux.md` - 登录UI流程
- `plan/eng.md` - 技术架构
- `plan/PLAN.md` - 3个阶段，15个任务

**步骤 4: 实现**
```bash
/rpi:implement oauth2-authentication
```
进度:
- 阶段 1: 后端基础 → 通过
- 阶段 2: 前端集成 → 通过
- 阶段 3: 测试和打磨 → 通过

结果：功能完成，准备好进行PR。

---

## 功能文件夹结构

所有功能工作都在`rpi/{feature-slug}/`中：

```
rpi/{feature-slug}/
├── REQUEST.md              # 步骤 1: 初始功能描述
├── research/
│   └── RESEARCH.md         # 步骤 2: 继续/停止分析
├── plan/
│   ├── PLAN.md             # 步骤 3: 实现路线图
│   ├── pm.md               # 产品需求
│   ├── ux.md               # 用户体验设计
│   └── eng.md              # 技术规范
└── implement/
    └── IMPLEMENT.md        # 步骤 4: 实现记录
```

---

## 代理和命令

| 命令 | 使用的代理 |
|---------|-------------|
| `/rpi:research` | requirement-parser、product-manager、Explore、senior-software-engineer、technical-cto-advisor、documentation-analyst-writer |
| `/rpi:plan` | senior-software-engineer、product-manager、ux-designer、documentation-analyst-writer |
| `/rpi:implement` | Explore、senior-software-engineer、code-reviewer |