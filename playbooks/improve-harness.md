# Improve One Harnessed Job

参见 [About the playbooks] 了解此过程的起源和当前状态。

使用此 playbook 在固定 worker 下改善一个有界、代表性工作。它关闭一个操作循环：

> 基线 → 最早的缺口 → 最小的所有者干预 → 原生验证 → 新的重新运行 → 保留、修改或移除

此 playbook 将 [Harness Engineering anthology] 转换为一个有界操作循环。一次遍历可以显示工作在观察条件下完成，并可以揭示下一个缺失的边界。使用 [Repository Review Playbook] 进行广泛评估，使用 [Evaluate the Harness] 进行比较、因果或纵向声明。

[Harness Engineering anthology]: ../README.md
[Repository Review Playbook]: repository-review.md
[Evaluate the Harness]: ../evals/
[About the playbooks]: README.md

## 建立范围和 authority

当应用于另一个目标时，context bundle 是补充性和只读的。用户请求的结果和授予的 authority 支配；目标的 instructions、所有者、合约和当前状态提供本地真相。

授权检查仅允许其范围内的读取。它不允许更广泛的数据访问、出口或修改。仅当任务和目标权威合约都授予该操作时才修改目标。

不要扩大权限、重新定义验收、削弱评分器以使运行通过，或将未经证实的自我报告提升为策略。当实现在任务权限之外时，在记录证据、建议所有者、干预、proof 计划和预期效果后停止。

选择一个足够小以重新运行、足够重要以测试疑似缺口的真实工作。广泛的仓库审计、多季度架构计划和"让 agent 更好"的无限制授权需要更窄的声明，然后此循环才能开始。

## 记录工作合约

在更改 harness 之前写下条件：

```text
Target and revision:
Relevant external state:
Fixed model and coding-agent configuration:
Representative job:
Accepted outcome:
Evidence that proves the outcome:
Authority envelope and approval boundary:
Budget and stop conditions:
Suspected harness gap:
```

将目标真实性与其权威所有者保持在一起。playbook 可以记录如何到达该真实性；它不应创建可以在目标漂移时与自身一致的并行生产语料库或私有 fixture。

## 1. 观察基线

当安全且授权时，通过新轨迹运行工作。当新运行将产生重要影响时，检查最近的一次协作，其目标状态、worker 和结果已知。记录可观测证据而非仅摘要：

- 请求的结果是否被接受；
- worker 产出了哪些 proof；
- 哪些 context 可用、被检索且相关；
- 哪些能力被发现、调用并正确解释；
- 哪些地方有人中继了事实、tool 输出、分解或恢复；
- 重试、延迟、放弃的路径和可避免的审查周期；
- authority 摩擦或意外访问；以及
- 已存在于 harness 中的风险和承载成本。

将缺失的能力与不良发现分开，将缺失的事实与不良路由分开，将 worker 限制与环境缺口分开。基线是通过特定系统的轨迹，而非对模型的一般判定。

## 2. 定位最早的失败交接

找到轨迹缺少工作所需内容的第一个点。向上游追踪症状，直到一个权威所有者可以塑造未来的运行。分类缺口以选择相关路由：

- **Context：** 信息缺失、过时、过载或从未被检索。
- **能力：** 操作不可用，或在发现、选择、调用、解释、修复或真实系统验证中失败。
- **Domain 所有权：** 多个合理表示竞争，或没有 type、API、状态机或规范来源拥有不变量。
- **Authority：** 能力和权限混淆、过度或缺少批准、审计或恢复边界。
- **Proof：** 检查建立了内部代理，而用户或操作声明未被测试。
- **Feedback 或 delivery：** 接受的经验、artifact 身份、发布步骤或维护条件未能在轨迹中存活。
- **Worker 限制：** 即使环境使工作清晰可操作，必要的行为仍然不可靠。

一次失败的轨迹无法建立 worker 限制。将其保留为开放的候选项，直到可比的重复证据将其与随机方差和未解决的环境缺口分开。

使用 [`AGENTS.md`] 仅在目标本地证据使所有者决策未解决时检索一个 thesis。不要导入另一个仓库的布局、fixture、策略或版本固定作为本地所有权的替代品。

[`AGENTS.md`]: ../AGENTS.md

## 3. 陈述一个干预假设

命名最早的最小可逆变更所有者。在实现之前陈述其预期行为：

```text
If <intervention> is added at <authoritative owner>, then the fixed worker will
<observable behavioral change> on <representative job>, because <mechanism>.

Evidence that would support this:
Evidence that would weaken this:
Expected carrying cost and owner:
```

优先选择移除人工中继或在源头使不变量清晰的干预。示例包括更短的根路由、规范示例、类型化边界、可操作诊断、domain tool、authority 门控、真实系统测试或具有明确状态和恢复的 runbook。新机制应通过观察到的失败来证明其维护成本的合理性。

## 4. 在声明边界实现和验证

确认变更权限，然后通过目标的正常工作流进行干预。保持 diff 足够窄，使行为假设保持可理解。验证两个层面：

1. 运行保护内部合约的原生目标检查。
2. 行使建立被接受结果的用户或操作旅程。

收集 worker 在正常操作期间应产出的证据。仅评估者检查可以诊断结果，但它不能替代 worker 预期附加的 proof。仅当新的上游所有者使其冗余且目标证据支持移除时，才移除下游控制。

## 5. 运行新轨迹

使用相同的模型、coding-agent 配置、authority envelope 和实质等效的外部状态重新运行同一类工作。使用新的 session 和隔离的、等效的起始状态，保留无关工作，这样重新运行不会从实现对话中继承隐藏帮助。

确认干预可用、被检索或调用，且与重新运行相关。成功的结果无法提供轨迹从未使用的 instruction 或 tool 的证据。

跨独立维度比较重新运行与基线：

- 接受结果和匹配声明的 proof；
- 人工中继、引导和审查收敛；
- 经过时间和重试；
- authority 和恢复行为；
- 新的失败模式或转移的复杂性；以及
- 持续维护和延迟成本。

## 6. 保留、修改或移除

当新运行关闭了有界工作、干预实际被检索或调用、证据与假设机制一致且明显收益证明其承载成本合理时，临时保留干预。当缺口正确定位但接口仍然难以检索或使用时修改它。当目标本地证据可能已经提供该行为时，暂时搁置干预并在决定前重新运行。当它增加噪音、重复更好的所有者或反复未能改善工作时移除它。

在目标保存架构或操作历史的地方记录决策。命名所有者、证据、任何后续案例以及应重新考虑或退役干预的条件。持久的结果是改善的环境及其证据，而非另一个 harness 组件的存在。

## 保留紧凑的结果记录

```text
Job and accepted outcome:
Target revision and external state:
Fixed worker and authority envelope:

Baseline evidence:
Earliest failed handoff and owner:
Intervention and expected mechanism:
Verification performed:
Fresh-rerun evidence:

Outcome comparison:
Proof comparison:
Human-relay and latency comparison:
Risk and carrying-cost comparison:
Test-without evidence, if run:

Decision: retain | revise | remove
Durable owner and recorded lesson:
Follow-up or retirement condition:
Known limits:
```

此记录是一个操作循环的证据。它支持关于所观察工作和条件的有界声明。一次前后运行无法隔离干预的机制或估计一般处理效果或随机方差。

当多个旅程或所有权边界需要诊断时使用 [Repository Review Playbook]。当决策需要重复条件、环境对等、条件盲评分或干预泛化的证据时使用 [Evaluate the Harness]。
