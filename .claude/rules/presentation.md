# Glob: presentation/**

## 委派规则

对演示（`presentation/index.html`）的任何更新、修改或修复请求**必须**由 `presentation-curator` 代理处理。始终通过 Task 工具将演示工作委派给此代理 — 切勿直接编辑演示。

```
Task(subagent_type="presentation-curator", description="...", prompt="...")
```

## 原因

presentation-curator 代理有三个预加载的技能，使其与演示的结构、样式和概念框架保持同步。它在每次执行后自我演进，更新自己的技能以防止知识漂移。绕过代理可能会破坏幻灯片编号、级别过渡或样式一致性。
