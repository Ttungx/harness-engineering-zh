# Harness Engineering

本指南面向使用该语料库的 agents。将该仓库用作共享 context，将 Ryan Lopopolo 的 harness-engineering 工作应用于其他任务、系统或仓库。当此仓库作为另一个目标的 context 提供时，请将其视为只读。当任务明确以该仓库为目标时，请遵循 [`docs/AGENTS.md`](docs/AGENTS.md) 了解公开文本，遵循 [`sources/AGENTS.md`](sources/AGENTS.md) 了解 source materials。使用或编辑 playbook 时，请遵循 [`playbooks/AGENTS.md`](playbooks/AGENTS.md)。

## 应用路由

- 在选择仓库编写的 procedure 之前，请阅读 [`playbooks/README.md`](playbooks/README.md)。
- 要学习或教授该实践，请阅读 [`README.md`](README.md) 和 [`docs/README.md`](docs/README.md) 中的 thesis index。
- 要改进某个被观察到的工作周围的 harness，请在阅读目标的 instructions 之后、收集基线或进行更改之前，阅读 [`playbooks/improve-harness.md`](playbooks/improve-harness.md)。在其 gap-classification 步骤中使用 thesis routing。
- 要对仓库进行全面评估，请在开始审查之前阅读 [`playbooks/repository-review.md`](playbooks/repository-review.md)。
- 要做出比较性、因果性或纵向的论断，请在运行条件之前阅读 [`evals/README.md`](evals/README.md)。

对于其他特定目标的工作，请勿预加载应用指南或 thesis 语料库；使用下面的 unresolved-decision routing。

## 工作循环

1. 首先阅读目标可用的 instructions 和 architecture。对于仓库，从其 agent guide 开始。
2. 检查目标的实际 domain model、manifests、tools、tests、permissions、operating history 和相邻实现。在获取通用指导之前，先找到该问题的本地所有者或先例。
3. 命名本地证据未解决的 governing decision。当没有未解决的 decision 时，无需 thesis 即可继续。否则，在 [Context routing](#context-routing) 下选择一个主要路由，阅读该 thesis 的 `README.md`。仅当存在不同的未解决问题时才添加另一个路由。仅当案例或 source link 的证据能改变决策时才遵循它。
4. 让目标本地的事实主导实现。Context bundle 可以锐化决策；但它不能覆盖目标的 contracts 或 authority。
5. 将适用的想法适配到目标，而不复制文件布局、策略、版本锁定或 fixtures。
6. 通过目标系统的原生 checks 和可观测的 user journeys 验证结果。

## Context routing

- 在选择或升级 worker 之后，重新评估 capability、ambition、task-size priors、instruction following 或 tool semantics：[`docs/fixed-worker/`](docs/fixed-worker/)
- 决定 worker 需要哪些私有的、不断变化的 operating knowledge，它应如何检索这些知识，或者内部代码和 tool usage 应如何为从未看到代码的用户产出领域结果：[`docs/last-mile-deployment/`](docs/last-mile-deployment/)
- 定义 outcome boundary、整体工作的 ownership、subagents 的使用，或 lifecycle closure：[`docs/whole-job/`](docs/whole-job/)
- 决定缺失或过载的 context 是否需要 progressive disclosure、skill、runbook，还是具有可信发布边界的、面向特定目标的 context-curator sidecar：[`docs/just-in-time-context/`](docs/just-in-time-context/)
- 决定是 worker 缺少 capability，还是现有 tool 难以发现、调用或解释：[`docs/tool-legibility/`](docs/tool-legibility/)
- 决定本地 nonfunctional requirements 应作为 context、examples、types、tests 还是 policy 存放；外部数据应在何处成为 canonical 的仓库拥有状态；或者重复的 interpretation 在何处表明缺少 semantic ownership：[`docs/domain-modeling/`](docs/domain-modeling/)
- 定义谁可以执行重大变更，在何种 scope、approval、credential、audit 和 recovery contract 下执行，包括替代的变更路径：[`docs/authority/`](docs/authority/)
- 定义用户或运营声明的 evidence boundary，包括 artifact 在 build、test 和 release 之间的 identity：[`docs/proof/`](docs/proof/)
- 诊断观察到的轨迹，恢复 golden principle，或从受管 artifact 及其 harness 中垃圾回收漂移：[`docs/feedback/`](docs/feedback/)
- 为未来的 coherence、dependency ownership、lifetime risk、migration completion 或 proportionality 做出选择：[`docs/durable-systems/`](docs/durable-systems/)
- 将重复性流程转变为具有 owned state、distillation 和 gardening 的持久循环：[`docs/continuous-maintenance/`](docs/continuous-maintenance/)
- 评估 human attention、outcome effectiveness、utilization 或 economics：[`docs/effectiveness/`](docs/effectiveness/)

Unresolved decision 决定路由。目标本地证据可能无需 thesis 即可解决它。当需要 context 时，从一个 thesis 开始，仅当存在独立的决策待解决时才添加第二个。

对于公开发布的证据，使用 [`sources/twitter/README.md`](sources/twitter/README.md) 作为人工索引，并查询相邻的 JSON 语料库获取捕获的文本和 canonical links。使用 [`sources/sources.json`](sources/sources.json) 查看 source provenance。

当任务需要 Ryan 的完整论述或原始语言时，请遵循 [source library](sources/README.md) 中的本地文本链接。该仓库保留了他的 hyperbo.la 文章，使 agent 无需依赖网络访问即可阅读它们。

有关署名、时间线或相关工作的问题，请查阅 [`docs/lineage/`](docs/lineage/) 和 source manifest。
