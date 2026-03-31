# 天气SVG创建器 — 参考

## SVG模板

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 300 160" width="300" height="160">
  <rect width="300" height="160" rx="12" fill="#1a1a2e"/>
  <text x="150" y="45" text-anchor="middle" fill="#8892b0" font-family="system-ui" font-size="14">单位: [摄氏度/华氏度]</text>
  <text x="150" y="100" text-anchor="middle" fill="#ccd6f6" font-family="system-ui" font-size="42" font-weight="bold">[value]°[C/F]</text>
  <text x="150" y="140" text-anchor="middle" fill="#64ffda" font-family="system-ui" font-size="16">迪拜，阿联酋</text>
</svg>
```

### 占位符

| 占位符 | 替换为 | 示例 |
|-------------|-------------|---------|
| `[摄氏度/华氏度]` | 输入中的完整单位名称 | `摄氏度` |
| `[value]` | 输入中的数值温度 | `26.2` |
| `[C/F]` | 单位缩写 | `C` 或 `F` |

### 设计规格

| 属性 | 值 |
|----------|-------|
| 尺寸 | 300 x 160 px |
| 圆角半径 | 12 px |
| 背景 | `#1a1a2e` (深海军蓝) |
| 单位标签 | `#8892b0` (柔和蓝), 14px |
| 温度 | `#ccd6f6` (浅蓝), 42px 加粗 |
| 位置 | `#64ffda` (青色重点), 16px |
| 字体 | `system-ui` |
| 所有文本 | 居中 (`text-anchor="middle"` 在 x=150) |

---

## 输出Markdown模板

```markdown
# 天气结果

## 温度
[value]°[C/F]

## 位置
迪拜，阿联酋

## 单位
[摄氏度/华氏度]

## SVG卡
![天气卡](weather.svg)
```

---

## 输出路径

| 文件 | 路径 |
|------|------|
| SVG卡 | `orchestration-workflow/weather.svg` |
| Markdown摘要 | `orchestration-workflow/output.md` |
