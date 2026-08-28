# 评估 Harness

当同一个 worker 在工作真正重要的环境中产出更好的被接受的结果时，harness 才值得其承载成本。评估完整的系统：选定的模型和 coding agent 通过 context、tools、authority、feedback、proof 和 delivery 运行。Ryan 认为纯代码基准测试产出的是[产出代码的机器]，而[单次 rollout 不能代表]长期工作。

[产出代码的机器]: https://x.com/_lopopolo/status/2054960622140326312
[单次 rollout 不能代表]:
  https://x.com/_lopopolo/status/2054969017140752758

使用 [Improve One Harnessed Job] 进行有界的操作性前后对比。本指南负责那些旨在估计干预在不同条件或随时间变化的效果的比较。

[Improve One Harnessed Job]: ../playbooks/improve-harness.md

## 从决策开始

命名 harness 变更及其预期改善的结果。一个有用的评估会隔离如下决策：

- context 路由是否帮助 worker 恢复本地不变量；
- domain tool 是否使之前无法完成的工作成为可能；
- runbook 是否减少了 authority 边界上的人工中继；
- type、API、test 或 lint 是否防止了已知的失败类别；或者
- domain 级别的 proof 是否让用户无需审查中间代码即可接受 artifact。

选择一个真正测试干预的任务。从目标本地证据解决工作的 worker 并非未能使用补充 context。tool 无法改善一个从不需要其能力的工作。在运行前记录支配性决策，这样评估就不会奖励与结果脱节的术语或文件访问。

在收集结果之前，写下：

- 被接受的结果及其建立的证据；
- 目标修订和外部状态；
- 固定的模型、coding-agent host 和原生操作接口；
- 基线 instructions 和 tool surface；
- authority envelope、budget 和停止条件；
- 在不同条件下产生差异的确切 context 或 tool 干预；
- 预期的行为效果；以及
- 会削弱假设的结果。

## 保持比较稳定

给基线和处理组相同的目标、任务、authority、外部状态和 budget。保持除命名干预之外的每个 instruction 和 tool 稳定。使用新的 session 和目标状态，随机化运行顺序，并重复每个条件足够多次以暴露方差。当模型或 coding agent 发生实质性变化时，开始新的评估周期。

[Hold the Worker Constant] 阐述了为什么 worker 变更会重新打开关于雄心、指令遵循、context 使用和 tool 语义的假设。

[Hold the Worker Constant]: ../docs/fixed-worker/

记录干预是否可用、是否被检索、是否被调用以及是否相关，作为独立的事实。提供的 context bundle 估计了使 context 可用的效果。只有当 worker 实际检索它时，它才估计读取 context 的效果。callable tool 同样，当轨迹从未发现或调用它时，也无法提供关于 tool 质量的证据。

当目标本地示例解决了两个条件时，比较在该任务上无法估计任何增值。它无法支持干预从无帮助的更广泛主张。

环境对等需要直接证据。记录每次运行的有效写权限、可用可执行文件、凭据、网络可达性和关键权限。当一个条件无法编辑目标或执行其检查时，比较无法将结果差异归因于 harness。

## 让真实性跟随目标

目标拥有的语料库、当前记录和操作系统保留在其权威所有者手中，并在每个条件下均可访问。在目标仓库中行使源到输出对等。评估者不携带并行的生产语料库。

有界的 fixture 可以表示命名的边界条件。其范围应足够小，以便审查者能够看到它们测试的行为。将完整的外部语料库复制到评估者中会创建一个影子权威，它可以在仍然与自身预期输出一致的同时发生漂移。

保留的任务、预期结果和评分代码保留在 worker 可见 surface 之外。保持这些材料独立于 worker 正常工作和证明工作所需的本地数据。隐藏的评估者结果可以对结果评分；它不能替代 worker 预期产出的 proof。

## 评估合约

每个评估拥有一个结果边界。优先选择在该边界观察真实情况并接受等效实现的评分器。

| 结果 | 问题 |
| --- | --- |
| 结果 | 交付的 artifact 是否满足特定用例的行为声明？ |
| Proof | worker 的 evidence 是否在真实环境中建立了该声明？ |
| 架构和所有权 | 变更是否保持了支配性不变量和维护模型？ |
| 轨迹诊断 | 它使用了哪些延迟、重试、tool、token 和人工注意力？ |

结果、proof 和架构决定接受。轨迹诊断解释成本和失败。流畅的解释无法修复不正确的 artifact，而仅评估者的成功无法修复缺失的 worker 产出的 proof。

精确的文件名、辅助函数名、语句顺序、序列化空白和私有 fixture 布局只有在产品合约使其可观测时才是有效的需求。使用条件盲审进行定性架构和维护判断。保留评分标准和每次运行，包括失败。

## 衡量判断是否复合

核心的纵向问题是后续工作是否从早期工作中继承有用的判断。在将经验提升为 context、example、type、tool 或可执行控制之后，从同一失败类别运行新任务。衡量复发率、人工中继、修复成本、proof 质量和干预的承载成本。在效果无法在类似运行中存活时，消融 context 或 tooling。

长期迁移需要检查点，这些检查点要问：今天被接受的工作是否使最终目的地更易到达和更安全。[Future Regret in Artichoke's State Refactor] 追踪了一次失败的核心状态模型变更、它产生的准备性 pull request、后续的集成尝试和生命周期失败。这一公开历史以适当的视野支持延续性任务和维护问题，适用于关于多年一致性的声明。

[Future Regret in Artichoke's State Refactor]: artichoke-state-modeling.md

公共目标及其历史使其成为一个回溯性校准案例。其已知路线无法支持能够阅读此 bundle 的 worker 的一般能力声明。能力测量需要一个新的私有目标或一个实质性转换的 fixture，其路线在 worker 可见 context 中不存在。

评估视野应与声明匹配。确定性规则可以立即针对新示例进行测试。架构一致性、未来遗憾和维护所有权需要跨变化仓库状态的重复工作。即使单个不变量有强可执行强制，这些仍然是经验性结果。

## 识别无效结果

当出现以下情况时，评估无法支持其预期声明：

- 声明涉及已消费的 context 或已行使的能力，但处理组从未检索或调用；
- 在预声明干预之外出现 instruction、tool、authority、外部状态或写权限差异；
- 将单次随机 rollout 视为代表性样本；
- 评分器需要未公开的参考实现；
- 仅评估者检查被报告为 worker 产出的 proof；
- 复制的语料库让评估者与其影子权威一致；
- token 计数、行数或活动被用来替代被接受的结果；
- worker 在比较内发生变化；或者
- 公共报告命名了读者无法检查或复现的不透明目标和修订。

在其中一次失败后，内部试验仍可指导本地迭代。其公共声明应止于它实际保留的证据。可分享的报告需要可检查的目标、可运行的 fixture 生成器，或足够的 artifact 和方法供另一位读者审计结果。

## 将结果反馈回 harness

按缺失 context、不良路由、不可用能力、不清晰 authority、弱 proof、domain 建模错误或 worker 限制对失败进行聚类。改善最小的所有者 artifact，然后重新运行新比较。为重复的失败类别添加保留用例，并淘汰其效果或价值不再覆盖维护成本的控制。

[Turn Feedback Into Infrastructure] 描述了观察到的轨迹如何成为持久的改进，而不会将原始 session 档案变成永久 context。

[Turn Feedback Into Infrastructure]: ../docs/feedback/
