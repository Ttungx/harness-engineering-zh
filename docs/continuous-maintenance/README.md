# 将已知工作作为持续循环运行

一大类软件工作已经有明确的方向。为已知用户群本地化产品。填补监控空白。保持依赖项最新。移除已过时的兼容路径。修复偏离架构规则的漂移。刷新生成的数据和文档。一旦期望的结果、证据和权限变得清晰，agent 就可以让这些工作持续推进，而无需等待路线图排期。

[软件工作不再需要排期] 通过组织经常推迟的工作来阐述这一转变：为内部用户添加日语支持、监控、仪表盘、安全加固、性能优化和小型用户体验修复。即使每个人都同意这些变更应该发生，它们在季度规划能力中的竞争力也很差。当它们的期望状态、接口、防护栏和验收流程变得清晰时，agent 可以异步执行它们并返回证据供审查。

[软件工作不再需要排期]:
  https://hyperbo.la/w/software-work-not-scheduled/#:~:text=work%20does%20not%20need%20to%20wait%20for%20a%20sprint%20slot

持续维护始于一个已被接受的方向。一个可行的循环可以回答五个问题：

- 什么条件应该保持为真？
- 什么信号表明当前系统已经偏离了该条件？
- 什么证据证明提议的变更恢复了该条件？
- 哪些操作可以自主进行，哪些地方需要审批？
- 哪个持久状态为下一次迭代记录结果？

[Code Red 需要维护循环] 将紧急情况视为一个适应性场景，使循环的契约变得可发现。在响应过程中，人们会了解到哪些不变量重要、哪些信号预示漂移、哪些干预有效、哪些判断调用重要。绿色指标标记恢复。持久化完成还会记录目标、传感器、授权操作、升级路径以及将保持不变量健康的循环所有者。

[Code Red 需要维护循环]:
  https://hyperbo.la/w/code-reds-need-maintenance-loops/#:~:text=That%20understanding%20should%20become%20a%20maintenance%20loop%20the%20team%20leaves%20behind

该持久循环改变了响应被允许结束的方式。Agent 执行可以使持续的所有权成本足够低，以至于一些新发现的不变量可以得到维护，而无需无限期配置的团队。[一个目标、一个 harness 和一个审查循环] 在 tiger team 解散后继续承载运营模型。

[一个目标、一个 harness 和一个审查循环]:
  https://hyperbo.la/w/code-reds-need-maintenance-loops/#:~:text=The%20work%20that%20used%20to%20require%20an%20indefinitely%20staffed%20team%20can%20sometimes%20become%20a%20goal%2C%20a%20harness%2C%20and%20a%20review%20loop

该持续职责是一个目标而非任务。Code red 文章使用 [Codex 的 `/goal` 原语] 作为持久目标，在运行过程中发现并生成任务。"修复这个不稳定的测试"可以以一个补丁结束。"将 p95 CI 延迟保持在 N 分钟以下"则持续观察系统、生成和测试干预措施，并在必要时请求人类权限。持久状态连接这些任务。明确的退役条件在长期授权或维护的的能力过时后关闭循环。

[Codex 的 `/goal` 原语]:
  https://hyperbo.la/w/code-reds-need-maintenance-loops/#:~:text=A%20goal%20is%20a%20process%20for%20doing%20and%20discovering%20work%20which%20itself%20spawns%20tasks

缺失的答案会将即时结果变为调查、提案或升级。当用户需求、接口、架构或验收边界仍未确定时，产品发明仍然是前台工作。边界取决于期望结果的确定程度。六个月的迁移可以是已知工作；三行的产品变更仍然可能需要产品判断。

[OpenAI Symphony 文章] 在团队规模上描述了相同的划分：常规实现可以通过持续运行的 supervisor 进行，而模糊的工作和需要强判断的决策仍然是活跃的工程工作。[fixed-worker 论题] 阐述了模型或 agent 变更如何重新开启关于哪些结果可以安全进入循环的假设。

[OpenAI Symphony 文章]:
  https://openai.com/index/open-source-codex-orchestration-symphony/#:~:text=Symphony%20can%20handle%20the%20bulk%20of%20routine%20implementation%20work
[fixed-worker 论题]: ../fixed-worker/

## 为循环提供仓库拥有的契约

Cron、事件触发器和尾调用——一次完成的运行调用或调度其后续运行——唤醒 worker。一份签入的 runbook 提供任务：意图、当前真实来源、候选选择规则、范围、证据、权限、升级、回滚以及下次运行的状态。Ryan Lopopolo 将这些触发机制称为 [loop engineering]。调度器可以保持小巧，因为仓库承载了持久的运营知识。

[loop engineering]: https://x.com/_lopopolo/status/2068882715399926026

一份有用的维护 runbook 告诉 agent：

- 检查哪些外部和仓库状态；
- 如何判断是否需要工作；
- 哪些变更属于长期授权范围；
- 哪些检查建立安全的候选版本；
- 哪些变更需要人类审批；
- 如何证明实际结果；
- 当运行未改变任何内容时记录什么；
- 部分进展后如何恢复；以及
- 何时 runbook 或循环本身应该退役。

这种安排使维护策略变得可审查、可版本控制，并对每个未来的轨迹可用。Ryan 的 [仓库维护布局] 将面向路由的 `AGENTS.md` 与人类入职、固定工具链、依赖更新 runbook 和从仓库历史中提炼的防护栏配对。自动化指向该知识库并执行当前契约。

[仓库维护布局]:
  https://x.com/_lopopolo/status/2060767456302428343

[让 Codex 为我的扫地机器人升级] 展示了在真实运营风险下的该安排。Loki 是一台运行开源 Valetudo 固件的扫地机器人。自定义 Tailscale daemon 是其远程访问路径。每周自动化检查 Tailscale 发行版和上游构建标签行为，比较当前和候选功能集，构建 `linux/arm64` 候选版本，并准备 pull request。其 prompt 是指向签入文档的简短指针。仓库拥有完整的评估契约。

[让 Codex 为我的扫地机器人升级]:
  https://hyperbo.la/w/robot-vacuum-canary-tailscale/#:~:text=The%20repo%20carries%20the%20durable%20instructions

同一契约也缩小了自动化的权限。计划运行可以研究、构建和提议。单独的部署 runbook 将候选版本暂存在同一台扫地机器人的第二个 Tailscale 身份和状态目录下。该 canary 必须启动、加入 tailnet 并接受 SSH，然后才能进行生产变更。两个人类审批守卫关键边界：认证新身份，以及在全新备份后授权替换生产访问路径。

## 在运行间携带最新的世界模型

持续循环跨越多个 agent 轨迹运行。它需要持久状态来记录上次运行观察到了什么、拒绝了哪些候选版本及原因、哪个变更处于活跃状态、完成了哪些证明，以及哪些决策仍需要人类参与。该状态可以存在于仓库历史、问题跟踪器、执行计划、自动化内存或拥有实时条件的外部系统中。每个所有者应该保持清晰。

[openai/symphony:SPEC.md] 展示了协调层。Symphony 持续从问题跟踪器读取符合条件的工作，为每个 issue 创建一个工作区，遵守依赖关系，重试中断的工作，协调跟踪器变更，并暴露操作者可见的状态。仓库拥有的工作流策略告诉 coding agent 如何执行工作；跟踪器记录哪些结果可用、被阻塞、审查中或已完成。

[openai/symphony:SPEC.md]: https://github.com/openai/symphony/blob/main/SPEC.md

世界模型还必须注意到仓库之外的变化。一个请求询问 [Canonical Multipass]——一个在 macOS、Windows 和 Linux 上启动本地 Ubuntu 虚拟机的工具——是否可以成为 agent 编排器的后端，为 coding agent 提供隔离的机器来工作。Ryan 建议将这单一建议扩展为 [对可信 VM 运行器整个类别的研究]：让 Codex 找到高质量、维护良好、受信任的 VM 运行器，这些运行器提供相同的能力，并为合适的候选者添加后端。

[Canonical Multipass]: https://x.com/_lopopolo/status/2061613371883954430
[对可信 VM 运行器整个类别的研究]:
  https://x.com/_lopopolo/status/2062220470057898417

然后他提议了一个 [持久 VM-runner agent]，它可以在成员变化时保持该类别的最新状态：搜索新的候选者、添加有用的后端，以及移除已过时或不安全的运行器。在这种运营模型下，期望的能力保持稳定而实现发生变化，因此没有最终的运行器列表。持久状态记录最新的分类和证据；退役跟随能力或长期授权过时后进行。

[持久 VM-runner agent]: https://x.com/_lopopolo/status/2062222490483474772

一个有限的 campaign 在多次运行中执行已知的转换。Ryan 设置了一个 [每日重新许可循环]，将一个 Rust crate 从 MIT 许可证迁移到 MIT OR Apache-2.0 双许可证，并持续运行直到仓库范围完成。API 所有者同样可以 [迁移依赖方] 到已确定的替换契约，包括跨服务和组织边界的消费者。campaign 需要依赖感知的批处理、每次运行产生独立有用结果、防回归的棘轮机制、可衡量的完成条件，以及临时迁移机制的退役。

[每日重新许可循环]: https://x.com/_lopopolo/status/2056958332464984429
[迁移依赖方]: https://x.com/_lopopolo/status/2049591048759116267

当目标接口或架构仍未确定时，循环的交付物是证据、提案或为选择下一步的人提供的升级。

一个已确定的发现契约可以管理重要性仍未确定的发现。潜在的 code red 是一种严重的、正在恶化的条件，一旦被识别就值得紧急关注，但尚未被识别或宣布。一个 [发现潜在 code red 的流程] 可以检查指定的指标、数据源和弱信号，组装一个可供决策的候选者，并记录哪些误报浪费了注意力。这减少了对恰好发现问题、能令人信服地描述问题、有权升级并能触达正确管理层的人的依赖。一个人仍然决定该候选者是否值得维护目标，以及由此产生的循环应拥有哪个不变量。

[发现潜在 code red 的流程]:
  https://hyperbo.la/w/code-reds-need-maintenance-loops/#:~:text=An%20agentic%20system%20reduces%20dependence%20on%20executive-attention%20topology%20from%20the%20discovery%20process

## 让每次迭代都证明并记录其结果

维护循环应以少量持久状态之一结束：无需操作、候选版本已提议、变更已证明、已请求审批、需要恢复或策略已过时。安静的空操作运行是健康的。重复重新发现相同事实表明状态缺失。

证据必须与维护的条件匹配。成功的编译建立了候选二进制文件。Canary 证明了该二进制文件可以启动、加入其 Tailscale 私有网络并接受 SSH。生产日志和 SSH 检查证明切换保留了访问路径。扫地机器人 runbook 分别记录这些不同的断言，并保留旧二进制文件以供恢复。

同样的纪律适用于低后果的维护。Runner 镜像循环必须在其策略中同时保持两个属性：镜像保持 [固定且最新]。文档循环检查文本是否仍然描述真实行为。重构循环扫描指定类别的偏差，迁移每个实例，并留下一个棘轮机制防止旧形式回归。

[固定且最新]: https://x.com/_lopopolo/status/2077904710003228694

重试、有界并发、去重、停止条件和审计跟踪使持久性变得可操作。升级应携带一个可供决策的数据包：当前状态、已尝试的工作、证据、风险以及最小的未解决选择。人类提供判断而无需重建轨迹。

## 用同样的循环维护 harness

agent 周围的环境也会积累漂移。文档变得过时，代码产生竞争模式，审查评论暴露出缺失的防护栏，一个曾经有用的工具在模型或 agent 升级后可能变得不必要。

OpenAI 的 [定期后台维护任务] 替代了每周的人工清理会话。这些任务扫描偏离黄金原则的情况、更新质量评级、打开专注的重构 pull request，以及维护过时文档。小修复在不良模式成为未来变更的 prompt 素材之前就已落地。

[定期后台维护任务]:
  https://openai.com/index/harness-engineering/#:~:text=On%20a%20regular%20cadence%2C%20we%20have%20a%20set%20of%20background%20Codex%20tasks

反馈收集也可以持续运行。[AI Native DevCon 演讲] 提出了一个每夜蒸馏循环，处理审查评论、人类干预、失败构建和生产异常。Agent 检查这些证据以发现缺失的上下文、工具和防护栏，然后提出对环境的持久改进。

[AI Native DevCon 演讲]:
  https://tessl.io/registry/ainativedev/aidevcon-2026-ldn/0.100.8/files/talk-lopopolo-harness-engineering/transcript.md#:~:text=Every%20review%20comment,every%20night

[每日会话日志反思] 在团队规模上描述了这一点：agent 会话日志每天被收集，检查团队可以改进工作方式的地方，并反思回仓库中，使一个人的经验惠及后续运行。原始轨迹仍然是遥测数据。经过审查的蒸馏将稳定的教训提升到最小的持久所有者：一个 runbook、类型、工具、测试、lint、架构规则或路由文档。

[每日会话日志反思]:
  https://www.latent.space/p/harness-eng#:~:text=Running%20agent%20loops%20over%20them%20every%20day

自动重构、垃圾回收和 [蒸馏] 构成了对 harness 本身的维护循环。接受的变更优化了未来的运行；过时的指令和冗余的控制被移除。该循环应减少已知故障，并退役因更强大的所有者而变得冗余的重复验证。

[蒸馏]: https://x.com/_lopopolo/status/2064346868398874848

## 将发明保持在前台

持续维护保持了策略与变化世界之间已被选择的关系。人类仍然决定值得构建什么样的世界。他们设定产品方向、解决不兼容的用户需求、在权衡未知的架构之间做出选择，并定义新的验收边界。

一个成熟的循环完成已确定的工作，围绕不确定的工作收集证据，并升级需要判断的选择。人类注意力更快到达发明环节，因为本地化、依赖更新、监控、漂移修复、文档维护和长期机械迁移不再与它在同一个队列中等待。
