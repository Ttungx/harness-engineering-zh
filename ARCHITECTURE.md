# Architecture

该仓库是一个检索优化的 context bundle。根目录引入该实践并路由读者，agent guide 路由特定目标的决策，thesis index 命名已展开的论据，playbooks 将其转化为提出的 procedures，source library 保留证据。每一层指向下一层深度而不重复其工作。

## 检索边界

应用该语料库的 agent 从 [`AGENTS.md`] 开始，阅读目标系统的本地 instructions 和当前状态，然后仅加载与任务相关的 theses。案例和 primary sources 保留供更深入的推理、声明溯源或署名使用。原始语料库不属于默认工作集。

[`AGENTS.md`]: AGENTS.md

编辑者遵循 `docs/`、`playbooks/` 和 `sources/` 中的嵌套指南。这些指令管理综合、仓库编写的 procedures、来源、再分发和 source refreshes，而无需进入普通的消费轨迹。

## 所有权

| 路径                             | 拥有内容                                                              |
| -------------------------------- | --------------------------------------------------------------------- |
| `README.md`                      | 公开定义、承诺和入口点                                                 |
| `AGENTS.md`                      | 应用和 unresolved-decision routing，面向消费 agents                    |
| `ARCHITECTURE.md`                | 文档所有权和路由不变量                                                 |
| `docs/README.md`                 | Canonical thesis index                                                |
| `docs/<thesis>/`                 | 展开的论据、案例和应用                                                |
| `evals/`                         | 比较评估方法和纵向公开案例                                             |
| `playbooks/README.md`            | Playbook 选择、来源和证据状态                                          |
| `playbooks/improve-harness.md`   | 一个有界的从基线到重运行的干预循环                                     |
| `playbooks/repository-review.md` | 仓库级评估和就绪性发现                                                 |
| `playbooks/AGENTS.md`            | 使用和编辑编写的 procedures 的 instructions                            |
| `docs/AGENTS.md`                 | 编写和审查综合的 instructions                                          |
| `assets/`                        | 仓库展示资源                                                           |
| `sources/`                       | 参考文献、source-specific indexes、快照和 provenance                   |
| `sources/scripts/`               | 类型化的 source boundaries、validation、tests 和 retrieval helpers     |
| `sources/AGENTS.md`              | Source capture、refresh 和再分发规则                                    |
| `COPYING.md`                     | 许可范围、署名和其他贡献者持有的权利                                    |

## 不变量

1. **保持一个导航契约。** 根目录引入和路由，`docs/README.md` 建索引，thesis pages 综合，`evals/` 和 `playbooks/` 放置应用，`sources/` 保留证据。
2. **按需路由。** 普通任务接收最小的相关 thesis 集合。仅当案例和 source corpora 能改变决策时才加载。
3. **将论据与证据分离。** 一个 source 或前提可能支持多个 theses。完整的论据存在于 thesis pages 中；source library 路由或保留原始语言和 provenance。
4. **在边界上保护权利。** 本地快照需要明确的再分发依据。否则 source library 保留 canonical 和 archive links、metadata、有界引用和 source notes。
5. **测试行为效果。** 当该 bundle 改变了 engineering decisions 时才算成功，而非 agent 重复其术语。
6. **保持 bundle 的适度性。** 该文档语料库不需要构建系统或依赖图仅仅是为了验证自身的 links 和数据。
