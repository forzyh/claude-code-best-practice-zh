---
name: presentation-structure
description: 关于演示文稿幻灯片格式、权重系统、导航和部分结构的知识
---

# 演示文稿结构技能

关于 `presentation/index.html` 演示文稿如何构建的知识。

## 文件位置

`presentation/index.html` — 单个文件HTML演示文稿，带内联CSS和JS。

## 幻灯片格式

每个幻灯片是一个带 `data-slide` (顺序号) 和可选 `data-level` (过渡点处的旅程等级) 的div：

```html
<!-- 常规幻灯片 — 从上一个data-level幻灯片继承等级 -->
<div class="slide" data-slide="12">
    <h1>幻灯片标题</h1>
    <!-- 内容 -->
</div>

<!-- 等级过渡幻灯片 — 为此幻灯片及之后的幻灯片设置新等级 -->
<div class="slide section-slide" data-slide="10" data-level="low">
    <h1>部分名称</h1>
    <p class="section-desc">等级: Low — 此部分的描述</p>
</div>

<!-- 标题幻灯片 (居中) -->
<div class="slide title-slide" data-slide="1">
    <h1>演示文稿标题</h1>
    <p class="subtitle">副标题文本</p>
</div>
```

## 旅程栏等级系统

演示文稿使用4级系统而不是累积百分比：

- 等级通过关键过渡幻灯片 (部分分隔符) 上的 `data-level` 属性设置
- `data-level` 幻灯片之后的所有幻灯片继承该等级，直到下一个过渡
- 旅程栏填充到 25% / 50% / 75% / 100% 对应 Low / Medium / High / Pro
- 栏在幻灯片1 (标题幻灯片) 上隐藏；从幻灯片2开始显示栏
- 第一个 `data-level` 之前的幻灯片 (幻灯片2–9) 显示空栏 (尚未设置等级)
- `.level-badge` 在 `data-level` 幻灯片的 `<h1>` 上由JS注入 — 不在HTML中硬编码

### 等级按部分过渡

| 部分 | 幻灯片范围 | data-level | 栏高度 |
|---------|-------------|------------|------------|
| 第0部分: 介绍 | 幻灯片 1-4 | (无) | 隐藏 / 空 |
| 第1部分: 先决条件 | 幻灯片 5-9 | (无) | 空 |
| 第2部分: 更好的提示 | 幻灯片 10-17 | `low` | 25% |
| 第3部分: 项目记忆 | 幻灯片 18-24 | `medium` | 50% |
| 第4部分: 结构化工作流 | 幻灯片 25-28 | (继承medium) | 50% |
| 第5部分: 域知识 | 幻灯片 29-33 | `high` | 75% |
| 第6部分: 代理工程 | 幻灯片 34-46 | `high` | 75% |
| 附录 | 幻灯片 47+ | (继承high) | 75% |

## 导航系统

- `goToSlide(n)` — 用于TOC链接，必须匹配实际 `data-slide` 号
- `totalSlides` 自动从DOM计算 (`document.querySelectorAll('[data-slide]').length`)
- 箭头键、空格和触摸滑动用于导航
- 幻灯片计数器显示左下角 `current / total`

## 重新编号规则

添加、删除或重新排序幻灯片后：
1. 重新编号所有 `data-slide` 属性，从1开始顺序编号
2. 更新TOC/旅程地图幻灯片中的所有 `goToSlide()` 调用
3. JS的 `totalSlides` 自动计算 — 无需手动更新
4. 验证不存在间隙或重复

## 部分分隔符格式

部分分隔符使用 `section-slide` 类。等级过渡部分分隔符携带 `data-level` 并在描述中显示等级名称：

```html
<div class="slide section-slide" data-slide="10" data-level="low">
    <p class="section-number">第2部分</p>
    <h1>更好的提示</h1>
    <p class="section-desc">等级: Low — 获得真实结果的有效提示。</p>
</div>
```

JS将在运行时将 `.level-badge` (例如，"→ Low") 注入 `<h1>` 当等级过渡时 — 不在HTML中手动添加这些。
