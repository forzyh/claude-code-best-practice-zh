# Glob: **/*.md

## 文档标准

- 保持文件专注和简洁 — 每个文件一个主题
- 在文档之间使用相对链接（例如 `../best-practice/claude-memory.md`），而不是绝对 GitHub URL
- 在最佳实践和报告文档的顶部包含后向导航链接（见现有文件以了解模式）
- 添加新概念或报告时，更新 README.md 中的相应表格（CONCEPTS 或 REPORTS）

## 结构约定

- 最佳实践文档放在 `best-practice/`
- 实现文档放在 `implementation/`
- 报告放在 `reports/`
- 技巧放在 `tips/`
- 变更日志跟踪放在 `changelog/<category>/`

## 格式

- 在结构化比较中使用表格（以 README CONCEPTS 表格作为参考）
- 在链接最佳实践或实现文档时使用 `!/tags/` 中的徽章图像以保持视觉一致性
- 保持标题的层级关系 — 不要跳过级别（例如，不要从 `##` 直接跳到 `####`）
