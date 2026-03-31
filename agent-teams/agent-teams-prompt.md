创建一个代理团队来构建一个时间编排工作流，显示Dubai的当前时间作为
可视化SVG卡片。该工作流遵循命令 → 代理 → 技能架构模式：

- 命令编排流程并处理用户交互
- 代理使用预加载的技能获取Dubai的实时当前时间
- 技能从获取的数据创建可视化SVG时间卡片

**重要**：所有文件必须在`agent-teams/.claude/`内部创建 —
NOT在存储库根目录的`.claude/`目录中。这保持代理团队的
输出是自包含的，可通过`cd agent-teams && claude`运行。
不要引用或复制现有的天气工作流 — 从头开始构建所有内容。

分配这些团队成员：

1. **命令架构师** — 在`agent-teams/.claude/commands/time-orchestrator.md`中
   设计并实现`/time-orchestrator`命令。该命令应该：
   - 通过代理工具（NOT bash）调用time-agent来获取
     Dubai（阿联酋）的当前时间（Asia/Dubai时区，UTC+4）
   - 通过技能工具调用time-svg-creator技能来渲染
     从获取的时间数据生成的SVG卡片
   - 在frontmatter中使用model: haiku
   - 包含关键要求：顺序流、正确的工具使用
     （代理工具用于代理，技能工具用于技能）和输出摘要
   通过共享任务列表与其他团队成员协调以同意
   在组件之间传递的数据契约({time, timezone, formatted})。

2. **代理工程师** — 在`agent-teams/.claude/agents/time-agent.md`中
   设计并实现`time-agent`及其预加载的`time-fetcher`
   技能在`agent-teams/.claude/skills/time-fetcher/SKILL.md`中。代理应该：
   - 使用Bash获取Dubai的当前时间（Asia/Dubai，UTC+4）
     使用`TZ='Asia/Dubai' date '+%Y-%m-%d %H:%M:%S %Z'`
   - 将时间值、时区名称和格式化字符串返回给命令
   - 使用frontmatter：tools (Bash)、model: haiku、color: blue、maxTurns: 3
   - 通过`skills:`字段预加载time-fetcher技能
   time-fetcher技能(`agent-teams/.claude/skills/time-fetcher/SKILL.md`)
   应该包含用于Dubai时间的bash命令、预期的输出格式，
   并设置user-invocable: false因为它是代理专用的领域知识。
   将同意的数据契约发布到共享任务列表，以便命令
   架构师和技能设计师可以在接口上对齐。

3. **技能设计师** — 在`agent-teams/.claude/skills/time-svg-creator/SKILL.md`中
   设计并实现`time-svg-creator`技能，使用支持
   文件`reference.md`（SVG模板+输出模板）和`examples.md`
   （示例输入/输出对）。技能应该：
   - 从调用上下文接收时间值、时区和格式化字符串
   - 为Dubai创建一个自包含的SVG时间卡片显示当前时间
   - 将SVG写入`agent-teams/output/dubai-time.svg`
   - 将markdown摘要写入`agent-teams/output/output.md`
   - 使用提供的确切时间 — 永远不要重新获取
   - 在reference.md中保留模板（带占位符的SVG标记、markdown
     输出模板）和example.md中的示例对
   还创建`agent-teams/output/`目录用于输出文件。

所有三位团队成员应该在共享任务列表中创建任务以
协调数据契约：代理返回{time, timezone, formatted}，
命令通过上下文传递它，技能使用它。
并行启动所有三个，因为组件是独立的 —
他们只需要在数据接口上达成一致，不需要等待彼此的
实现。
