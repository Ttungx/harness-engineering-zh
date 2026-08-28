@AGENTS.md

# Claude Code

- 按需遵循导入指南中的 links。不要在分类任务之前预加载整个语料库。
- 使用 subagents 进行有界的证据收集和独立审查。让主线程负责决定性的 source reading、应用、验证和任务关闭。
- 将 auto memory 视为本地临时状态。在建立事实时，优先使用目标系统和本语料库引用的 sources。
