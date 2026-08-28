# Repository Review Playbook

参见 [About the playbooks] 了解此过程的起源和当前状态。

通过询问一个有能力的 coding agent 能否恢复真实工作、做出连贯变更、证明结果并在不将人作为中继的情况下安全操作来审查仓库。有代表性的完整旅程为声明的工作提供证据。

此 playbook 是一个广泛的诊断。使用 [Improve One Harnessed Job] 关闭单个授权干预循环，使用 [Evaluate the Harness] 进行比较或纵向声明。授权检查仅允许其范围内的读取；它本身不授权更广泛的访问、出口或对目标的更改。

[Improve One Harnessed Job]: improve-harness.md
[Evaluate the Harness]: ../evals/
[About the playbooks]: README.md

在审查期间保持选定的 model-and-coding-agent 配置不变。评估该 worker 周围的 context、tool、仓库结构、执行环境、权限和反馈循环。跟随有代表性的旅程，确定选定的 worker 能否通过这些 surface 完成工作。

## 从结果开始

审查应回答：

- agent 今天能完成哪些完整工作？
- 它在哪里需要隐藏的人工知识、tool 中继或 authority？
- 哪些检查建立了声明的行为，哪些仅建立内部一致性？
- 哪些防御性机制围绕缺失的 domain 模型积累？
- 哪些依赖和发布决策携带未拥有的风险？
- 什么是最小的变更，可以改善接下来的多个轨迹？

保持这些答案的独立性。拥有许多控制的仓库可能比拥有少数良好拥有的边界的仓库更难维护且更不安全。单个发布、凭据、兼容性或数据丢失缺陷可能主导原本强大的系统。

## 定义审查合约

记录目标修订和相关外部状态、固定的模型和 coding-agent 配置、声明的工作、其被接受的结果、每个边界所需的 proof、审查的 authority 和访问权限，以及其已知排除项。命名重要操作的停止条件。

审查可能诊断缺失的所有者或在其变更权限之外提出更改。保持该发现明确，而非将检查访问视为实施它的许可。

## 将工作作为轨迹检查

选择有代表性的任务并从请求到结果跟随它们：

1. agent 如何分类任务？
2. 哪些根指导将其路由到相关的 domain context？
3. 它能否找到需要更改的概念的现有所有者？
4. 它能否在没有人工中介的情况下重现和观察行为？
5. 实现是否保持一个 domain 模型和一个事实来源？
6. proof 是否在正确的边界行使声明？
7. agent 能否处理审查、CI、冲突和交付？
8. 它是否知道哪些操作需要人工判断或新权限？
9. 结果是否为下一次运行留下持久的改进？

静态检查是必要的但不充分的。当审查声称端到端可操作时，通过配置的 worker 运行至少一次真实旅程。[Give One Agent the Whole Job] 阐述了分解、执行、集成、proof、审查、交付和生命周期关闭的所有权。

[Give One Agent the Whole Job]: ../docs/whole-job/

## 从先前的协作中恢复证据

审查用户和 agent 一起工作的本地可用 session 日志。它们保留了仓库快照无法保留的证据：哪些请求需要反复引导、用户拒绝了哪些表述、agent 误解了什么、人工在哪里充当了工具、哪些 proof 改变了用户的信心，以及哪些经验从未到达持久的所有者。

将这些日志视为轨迹证据而非仓库策略。在得出结论之前，将每个观察与产生的 diff、审查、测试、部署或用户结果联系起来。寻找重复的修正和仓库仍然允许用户已经教 agent 避免的失败的情况。持久的修复将经验移入能够塑造未来工作的最小所有者：type、API、test、lint、runbook、架构规则、示例或路由文档。

本地日志可能包含私有源材料、凭据、个人数据、未发布的决策或仅用于一次协作的 context。仅阅读审查权限内的日志。将原始日志保存在其现有存储中，记录任何访问限制，并仅发布目标受众允许接收的有界综合和证据。不要将会话语料库嵌入被审查的仓库中。

## 审查所有权边界

### Context 和路由

寻找一个简短的根映射，说明仓库是什么、其兼容性 surface 为什么重要、适用哪个操作循环，以及每个任务类应在哪里检索更深层的指导。检查重要知识是否与工作一起版本控制，而非被困在聊天、私有记忆或一个自动化实例中。测试 [Route Context Just in Time] 中描述的检索层次结构：agent 能否在不预加载整个语料库的情况下找到相关的架构、依赖态势、生成合约或 runbook？

[Route Context Just in Time]: ../docs/just-in-time-context/

### 能力和 tool

跟随每个所需能力经过发现、选择、调用、解释、修复和真实系统验证。存在但难以找到的 tool 或其响应不指导下一步操作的 tool 会将人保留为中继。检查 agent 能否从格式错误的输入、部分结果、超时和不可用操作中恢复，而不猜测隐藏状态。

[Make Capabilities Legible and Operable] 阐述了这个完整的 tool 循环。

[Make Capabilities Legible and Operable]: ../docs/tool-legibility/

### Domain 和架构

使用 [Make the Repository Teach the Agent] 中的规范所有权和类型化边界来追踪出现在多个层次中的概念：版本、工作流清单、标识符、生成记录、UI 状态、发布 artifact 和依赖策略。每个都应有一个权威表示和一个清晰的所有者。

[Make the Repository Teach the Agent]: ../docs/domain-modeling/

将这些视为警告信号：

- 一个语言或概念的竞争性语义所有者；
- 一个事实复制到清单、策略、fixture 和测试中；
- fixture 耦合到附带表示而非 domain 条件；
- 不受信任的值流超出解码它们的边界；
- 状态转换由重叠的防御性检查表示；以及
- 适配器重新实现 domain 行为而非暴露其所有者。

重复的防御性检查看起来严谨，同时使系统不那么连贯。询问哪种 type、边界、状态机或事实来源会使无效条件不可表示。

### 执行和 proof

应用 [Prove the Outcome in the Real Environment] 中匹配声明的证据。验证 agent 能否启动和检查真实系统。根据声明，有用的证据可能包括浏览器状态、日志、指标、跟踪、截图、生成语料库 diff、fuzz 结果、兼容性向量、金丝雀健康或回滚验证。

[Prove the Outcome in the Real Environment]: ../docs/proof/

将每个绿色测试映射到其断言，然后在用户边界收集单独的证据。区分：

- 生成的新鲜性与源到输出对等；
- 单元行为与可用的浏览器旅程；
- 上传的 artifact 与健康的部署；
- 覆盖率百分比与标准兼容性；
- 漏洞形状模式与已证明的影响；以及
- 策略自我一致性与策略正确来源。

### Feedback 和护栏

使用 [Turn Feedback Into Infrastructure] 检查审查评论、失败轨迹、事件和构建失败是否改善了环境。稳定的经验可能成为 domain type、首选 API、lint、test、runbook 或路由文档。控制应解释被违反的原则并指向修复。

[Turn Feedback Into Infrastructure]: ../docs/feedback/

通过它预防的失败类别和它增加的承载成本来评估强制执行。寻找重复的验证、在品味稳定前编码品味的规则、没有收敛策略的审查者，以及重复可执行真相的散文。当更好的上游模型使其冗余时移除控制。

### 依赖和供应链

按照 [Own Dependency Risk] 中阐述的能力和生命周期所有权决策审查依赖。对于每个重要依赖或仓库拥有的替代品，询问：

[Own Dependency Risk]: ../docs/durable-systems/dependency-ownership.md

- 它提供什么能力？
- 为什么信任此来源？
- 它添加了哪些传递性和更新风险？
- 如果被移除，哪些兼容性和安全义务移入仓库？
- 什么语料库、fuzz、合规性、出处和维护计划重建信心？

根据目标仓库文档化的信任态势评估每个依赖。

### 交付和 authority

从验证到发布跟随确切的 artifact。优先部署 CI 已证明的不可变 artifact，而非在更特权的作业中重建它。检查工作流权限、凭据持久性、第三方 action 固定、网络执行、金丝雀边界、批准门控、回滚和部署后检查。

使用 [Maximize Autonomy Inside Explicit Authority] 将能力与 authority 分开。agent 可以完全准备重要操作，而人工保留对切换的批准。worker 不需要的 secret 不应进入其可读环境。

[Maximize Autonomy Inside Explicit Authority]: ../docs/authority/

### 比例性

使用 [Optimize for Measured Effectiveness] 计算每个 harness 组件的承载成本。询问它解决的观察到的失败或风险、另一个控制是否已拥有该不变量、正常循环需要多长时间，以及谁将维护该组件。优先选择覆盖仓库实际失败模式的最小系统。

[Optimize for Measured Effectiveness]: ../docs/effectiveness/

## 发现

以按后果排序的可操作发现开头。每个发现应命名：

- 风险中的行为或不变量；
- 具体证据和所有者边界；
- 为什么现有 proof 没有发现它；
- 在所有者边界的根本修正；以及
- 修正后应移除的任何冗余机制。

然后记录值得保留的强大基础、实际运行的旅程和审查的已知限制。

## 审查之后

当任务范围和目标权威合约授权更改时，选择一个按后果排序的发现并进入 [Improve One Harnessed Job]。保留其基线证据、预期行为效果、最小的所有者干预、proof 计划、承载成本和新重新运行标准。没有变更权限时，交付发现并在建议的干预处停止。

对一次旅程的成功修复在观察条件下建立操作结果。将一般效果的声明路由到 [Evaluate the Harness]。

## 就绪阻塞项

当证据显示以下情况时，将仓库视为尚未准备好承担声明的工作：

- 静默数据丢失或语义迁移回归；
- 可以绕过已验证 artifact 的发布路径；
- 具有实质意外权限的凭据或不受信任的执行；
- 没有适当门控或恢复路径的重要操作；
- 没有可信信心模型的拥有兼容性或安全 surface；
- 文档实质上夸大了强制保证；或者
- 无法证明被声明的用户可见或操作结果。

将 harness 扩展到声明的规模。每个阻塞项需要缩小声明或构建缺失的边界。
