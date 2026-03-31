# 跨模型（Claude Code + Codex）工作流

基于 [claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) 和 [codex-cli-best-practice](https://github.com/shanraisshan/codex-cli-best-practice)

## 工作流

```
┌─────────────────────────────────────────────────────────────────────────┐
│              跨模型 CLAUDE CODE + CODEX 工作流                          │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  步骤 1: 规划                                           Claude Code      │
│  ──────────                                           Opus 4.6          │
│  在规划模式下打开Claude Code（终端1）。                  规划模式        │
│  Claude通过AskUserQuestion与您进行交互。                                │
│  生成具有测试门关的分阶段计划。                                          │
│                                                                         │
│  输出: plans/{feature-name}.md                                          │
│                                                                         │
│                              ▼                                          │
│                                                                         │
│  步骤 2: 质量检查                                       Codex CLI        │
│  ──────────────                                       GPT-5.4           │
│  在另一个终端中打开Codex CLI（终端2）。                  质量检查        │
│  Codex根据实际代码库审查计划。                                          │
│  插入中间阶段（"第2.5阶段"）                                            │
│  带有"Codex发现"标题。                                   │
│  添加到计划 — 从不重写原始阶段。                                        │
│                                                                         │
│  输出: plans/{feature-name}.md（已更新）                                │
│                                                                         │
│                              ▼                                          │
│                                                                         │
│  步骤 3: 实现                                           Claude Code      │
│  ──────────                                           Opus 4.6          │
│  启动新的Claude Code会话（终端1）。                      实现            │
│  您逐阶段实现                                                          │
│  在每个阶段有测试门关。                                                  │
│                                                                         │
│                              ▼                                          │
│                                                                         │
│  步骤 4: 验证                                           Codex CLI        │
│  ──────────                                           GPT-5.4           │
│  启动新的Codex CLI会话（终端2）。                        验证            │
│  Codex根据计划验证实现                                                  │
│  对照计划。                                                             │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 跨模型工作流在生产环境中的实际样子

![跨模型工作流](assets/cross-model-workflow.png)

*最后更新: 2026-03-06*
