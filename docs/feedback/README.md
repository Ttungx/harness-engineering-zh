# 将反馈转化为基础设施

当团队将反复出现的纠正措施转化为塑造下一次运行的环境时，反馈就会产生复利效应。评审、干预、失败、事故、用户反应以及被接受或丢弃的工作成果，都能揭示 agent 在哪些方面存在困难，以及人类仍在哪些方面传递上下文、证据或权限。

在反复开展的工作中，后续轨迹会继承从早期结果中提炼出的约束和正面示例。经过数年积累，这种判断力可以使[连贯性具有累积效应]。

[make coherence cumulative]: ../durable-systems/#make-coherence-cumulative

## 收集可观察信号

有用的记录是可观察的轨迹及其结果。保留 prompt、工具调用及输出、差异、检查、评审、运行时证据、人类干预以及被接受或拒绝的结果。这些证据支持验证，无需依赖模型的隐式推理。

不同的传感器暴露系统的不同部分：

| 传感器                          | 能够揭示的内容                                            |
| ------------------------------- | ------------------------------------------------------------- |
| 人类引导或传递         | 缺失的上下文、工具、权限或对结果的理解   |
| 评审评论                 | 与团队判断的偏差或未表达的需求   |
| 测试、lint 和构建失败 | 对已可执行不变量的违反                  |
| 日志、追踪和事故     | 本地验证中缺失的端到端后果               |
| 用户反馈和分析     | 产品结果、需求或解释是否出错 |
| MLD 自报告                | worker 注意到的摩擦和缺失界面              |
| 已接受和失败的 artifact   | 正确决策和已知失败路径的密集示例      |

[feedback-capture inventory] 要求团队捕获评审评论、中断的运行、agent 干预、构建失败和生产异常，然后一起分析缺失的防护措施和更好的工作方式。OpenAI 的 [feedback encoded as documentation and tooling] 描述了相同的流程——从评审、重构和面向用户的 bug 转化为持久的环境变更。

[feedback-capture inventory]:
  https://tessl.io/registry/ainativedev/aidevcon-2026-ldn/0.100.8/files/talk-lopopolo-harness-engineering/transcript.md#:~:text=Every%20review%20comment,every%20night
[feedback encoded as documentation and tooling]:
  https://openai.com/index/harness-engineering/#:~:text=Review%20comments%2C%20refactoring%20pull%20requests%2C%20and%20user-facing%20bugs%20are%20captured%20as%20documentation%20updates

信号是线索而非诊断。外部失败、随机变异、错误的任务框架和过拟合控制仍然是可能的。在更改 harness 之前，将 worker 的解释与轨迹的其余部分进行比较。

许多被标记为"slop"的投诉指向缺失或不可访问的非功能性需求。[Slop 可能反映了缺失的需求上下文和合并门禁]：worker 缺乏本地质量标准，而测试和其他阻塞性检查并未约束被接受的结果。恢复治理需求可以将模糊的拒绝转化为对上下文、示例、评审策略或应塑造后续工作的可执行边界的搜索。

[Slop can reflect missing requirement context and merge gates]:
  https://x.com/_lopopolo/status/2036967120345743776

## 恢复治理失败类别

对某个指定行的修正可能会让所有同类缺陷保持不变。Ryan Lopopolo 的 [sentinel-error 示例]给 agent 了一个具体的 Go API 修正——返回一个 sentinel error 而非成功布尔值——但 agent 只更改了指定的函数。该指令包含了关于 API 设计的更广泛原则，但修复并未搜索其余同类问题。

[sentinel-error example]: https://x.com/_lopopolo/status/2054027190967349685

将反复出现的引导视为系统问题：

1. 重建承诺的结果和观察到的失败。
2. 定位本可以防止或暴露问题的最早边界。
3. 搜索受同一原则约束的其他实例。
4. 区分 harness 缺陷与 worker 变异、外部失败和错误前提。
5. 选择覆盖该类别但不禁止合法例外的干预措施。

当同样的纠正反复出现时，为其底层原则指定一个持久的负责人。[High-signal repeated steering] 值得一份文档、工具、检查或架构变更，而 [principle implicit in the prompt] 应指导搜索同类案例。这减少了反复引导，同时为新的和随机失败留出空间。

[High-signal repeated steering]:
  https://x.com/_lopopolo/status/2054011131572990266
[principle implicit in the prompt]:
  https://x.com/_lopopolo/status/2054021564862177517

AI Native DevCon 演讲使用 [React and Suspense] 作为说明性的入职场景。假设一个团队在特定系列的屏幕和组件中使用 Suspense 来实现其前端性能目标。人类可以在一次评审评论后记住这个惯例；而全新的 agent 运行则无法做到。团队必须决定缺失的上下文应该放在文档、lint、测试还是应用于每个 pull request 的评审者中。这种路由选择在最早可以塑造后续工作的边界处保留了反复出现的纠正。

[React and Suspense]:
  https://tessl.io/registry/ainativedev/aidevcon-2026-ldn/0.100.8/files/talk-lopopolo-harness-engineering/transcript.md#:~:text=make%20these%20mistakes%20statically%20impossible%20going%20forward

## 将经验提升为最小的持久负责人

将干预措施与经验的成熟度和后果相匹配：

| 干预措施                   | 适用场景                                                        |
| ------------------------------ | --------------------------------------------------------------- |
| prompt 调整和重新运行   | 任务框架或期望结果仍在探索中   |
| 路由文档、操作手册或 skill | 稳定知识必须出现在特定决策点     |
| 评审者或评估         | 判断是定性的、跨领域的，或仍在积累细微差别 |
| 类型、API 或领域工具      | 系统可以使正确使用变得自然，使误用变得困难    |
| lint、测试或策略检查    | 确定性不变量应阻止问题重现               |
| 架构或迁移      | 反复出现的缺陷暴露了错误的负责人或依赖方向 |

[steering-to-validator progression] 从引导开始，写下反复出现的指导，对仍然依赖上下文的判断使用评审，并将已确定的不变量移入可执行控制。稳定的教训可以继续向上游推进到类型和 API；反复出现的负责人缺陷可能需要架构迁移。[Nuance after overfit] 增加了相应的维护规则：当粗略指令过拟合时，添加细微差别。

[steering-to-validator progression]:
  https://tessl.io/podcast/109/#:~:text=the%20cheapest%20thing%20I%20can%20do%20is%20literally%20produce%20a%20change%20to%20my%20prompt,making%20deterministic%20tests%20that%20suss%20out%20this%20behavior
[Nuance after overfit]:
  https://hyperbo.la/w/what-does-it-mean-to-do-a-good-job/#:~:text=add%20nuance%20only%20when%20the%20coarse%20instruction%20starts%20to%20overfit

当更好的上游负责人使下游防御变得冗余时，就将其移除。在缺失的领域模型之上再叠加另一层验证器会保留不一致性并增加维护成本。

## 学习黄金原则并垃圾回收漂移

评审反馈、失败的发布、事故和反复出现的纠正可以揭示候选原则。恢复治理失败类别，并根据已接受的工作、合法例外和后续结果进行测试。为稳定的结果指定负责人和可评审的适用范围声明。当它可以表述为一个有主见的机械规则时，将其编码为有范围的 [golden principle]。

[golden principle]:
  https://openai.com/index/harness-engineering/#:~:text=Instead%2C%20we%20started%20encoding,open%20targeted%20refactoring%20pull%20requests

从评审后的结果中学习。[Accepted work]、[failed attempts]、评审者纠正和有记录的例外可以提供关于该原则及其实施的不同证据。[nightly feedback-distillation proposal] 收集评审评论、agent 干预、构建失败和生产异常，以便稳定的教训可以改善后续上下文和防护措施。在提升该教训之前，保留这些结果的理由和来源。

[Accepted work]: https://x.com/_lopopolo/status/2054037448297201696
[failed attempts]: https://x.com/_lopopolo/status/2063229530840690892
[nightly feedback-distillation proposal]:
  https://tessl.io/registry/ainativedev/aidevcon-2026-ldn/0.100.8/files/talk-lopopolo-harness-engineering/transcript.md#:~:text=Every%20review%20comment,every%20night

为学到的指导提供检索路径：描述其何时重要，并链接有范围的支撑示例。保持目标语料库和底层轨迹的变化与其权威所有者一致。有范围的引用快照可以遵循来源策略，而后续工作可以检索相关片段，而不会为目标真相创建并行的语义负责人。[Route Context Just in Time] 开发了该检索边界。

[Route Context Just in Time]: ../just-in-time-context/

Agent 使用仓库的代码和文本作为先例。Ryan 的观察——[code is prompts for future prompts]——意味着留在代码树中的弱模式可以在其直接缺陷被修复后继续影响后续轨迹。因此，持久原则需要两条路径：一条前向路径将不变量置于其最早的语义负责人处，以及一条后向路径找到并迁移现有实例。Ryan 描述了 [the no-await-in-loop migration]：启用 ESLint 规则，修复大约 600 个现有违规，并在一个 pull request 中添加穷举覆盖。该文章单独陈述了 [the durable rule]：重要的不变量应属于仓库拥有的验证器，当模型产生后续违规时，它可以[返回描述性反馈]。

[code is prompts for future prompts]:
  https://x.com/_lopopolo/status/2030460794765451300
[the no-await-in-loop migration]:
  https://hyperbo.la/w/production-function-changed/#:~:text=I%20turned%20on%20no-await-in-loop%20from%20ESLint,exhaustively%20add%20test%20coverage
[the durable rule]:
  https://hyperbo.la/w/production-function-changed/#:~:text=If%20it%20matters%2C%20it%20belongs%20in%20a%20verifier%20owned%20by%20the%20repo.
[return descriptive feedback]:
  https://hyperbo.la/w/production-function-changed/#:~:text=surface%20a%20descriptive%20failure%20to%20the%20model%20every%20time%20it%20writes%20another%20fetch%20call

垃圾回收保持该关系的时效性。定期扫描发现漂移、更新质量评级并提出窄范围修复，缩短反例作为先例保持可用的时间。收集器还维护 harness：更强的类型、API 或架构可以使下游检查变得冗余，而变化的需求或 worker 行为可以使指令和豁免失效。Ryan 将自动重构和垃圾回收与 [automatic distillation] 结合；[continuous maintenance] 拥有该循环的节奏、权限、证明和退役契约。

[automatic distillation]: https://x.com/_lopopolo/status/2064346868398874848
[continuous maintenance]: ../continuous-maintenance/

收集器为判断提供候选。将疑似违规与其提议的修复安全且完整的声明分开。[authority boundary] 管辖谁可以应用修复，[proof boundary] 管辖接受修复所需的证据。不确定的、重大的和跨领域的变更需要适当的评审；具有结果匹配证明的窄范围修复可以自动进行。经过评审的结果可以在后续过程中发挥教学作用，而不会将收集器自身未验证的输出变成策略。

[authority boundary]: ../authority/
[proof boundary]: ../proof/

强制执行的知识应积累，而验证器数量应整合。保持定性的原则留在上下文、示例和评审中。已确定的不变量可以移入可执行负责人。完整的迁移消除矛盾的先例，每次收集轮次都可以整合或退役因更强负责人而变得不必要的机制。

Ryan 的指令——[replace "codebase" with "artifact"]——将该循环推广到软件仓库之外。将其应用于文档、数据集、报告、接口和运营工作流意味着定义领域适当的原则、验收证据和维护。

[replace "codebase" with "artifact"]:
  https://x.com/_lopopolo/status/2076878736507736390

## 通过工作积累领域专业知识

评审反馈通常揭示了在不完整的本地上下文中做出的合理决策。在 [Build Hour: API & Codex] 中，一个 OpenAI 内部产品团队曾与安全团队合作选择了一个经批准的密码学实现，但该决策存在于一个旧的 Slack 线程中而非仓库中。Codex 为一位新工程师工作时添加了一个不同的 npm 密码学依赖。团队恢复了该决策，编码了一条仓库规则要求使用经安全批准的实现并禁止替代的 npm 密码学包，然后在该约束下重新运行了更改。仓库现在拥有了有范围的决策及其执行边界。

[Build Hour: API & Codex]:
  https://rewiz.app/channels/%40openai/build-hour-api-codex#:~:text=I%20partnered%20with%20our%20security%20engineering%20team%20to%20upgrade%20our%20app%20to%20the%20blessed%20cryptography%20library

这一步在评审之前改变了下一个 worker 的决策。它还为团队提供了一个版本化的位置，以便在批准的库或风险态势发生变化时修改选择。

在 2026 年 [un]prompted 演讲 [Code Is Free] 中，安全工程师 Paul McMillan 和 Ryan Lopopolo 跟踪了从精确问题到持久安全判断的更长循环。当代码变更使威胁模型中的风险接受假设失效时，worker 识别负责的专家，解释为什么需要他们的判断，并呈现仓库无法解决的模糊性。该对话保留了来源和范围，同时将稀缺的专家注意力集中在需要它的决策上。

[Code Is Free]: https://www.youtube.com/watch?v=U2O14Jd3MBU&t=395s

供应链案例始于对公共 `openai/codex` 仓库的一个 pull request，该请求[更新 pnpm 以修复漏洞并禁止 post-install 脚本]。Ryan 恢复了补丁背后的原则：包管理器是高风险边界，依赖应保持最新，安装不应在 CI 中执行任意代码，例外需要人工评审。

演讲随后展示了一个经过脱敏的假设性 Electron 产品，其 lockfile 中约有 1,500 个包。在十五分钟的对话中，安全工程师 Paul McMillan 要求 Ryan [risk-profile the direct and transitive dependencies]，收集关于上游活动和作者声誉的证据，并识别需要升级、移除或内部替换的包。四个小时的 agent 工作将时间点分析转化为由十六个 agent 运行的每日扫描器。它检查依赖图和上游仓库，然后通过 GitHub Advanced Security 以静态分析结果交换格式（SARIF）发布机器可读的安全发现。Ryan 将结果描述为 [making the harness an expert security engineer]。这些报告还可以馈送给下游 agent，由它们执行这些操作或从携带问题依赖的框架迁移出去。

[updates pnpm to repair vulnerabilities and forbids post-install scripts]:
  https://www.youtube.com/watch?v=U2O14Jd3MBU&t=912s
[risk-profile the direct and transitive dependencies]:
  https://www.youtube.com/watch?v=U2O14Jd3MBU&t=1030s
[making the harness an expert security engineer]:
  https://x.com/_lopopolo/status/2022853771517100166

后来的 Slack 反馈表明，该扫描器比 Paul 的 [general reference frame] 更容易捕获点状实例。团队将缺失的防护措施和调查信号添加到 `SECURITY.md` 中；这些原则随后馈送每日扫描器和新的依赖深度及许可证扫描器。专业知识就是这样积累的：保留来源和接受的范围，恢复治理原则，在可以塑造未来工作的最早负责人中编码稳定判断，并将实质变化的假设路由回专家。后续扫描随后在依赖图上应用 Paul 修订后的原则，而不是逐包重新发现它们。

[general reference frame]: https://www.youtube.com/watch?v=U2O14Jd3MBU&t=1176s

## 保持评审收敛

反馈系统需要验收策略。Ryan 团队中的早期评审 agent 可能会反复迫使实现 agent 扩大变更范围。团队指示评审者偏向于合并并呈现重要发现，而作者可以修复、附理由推迟或反驳。[reviewer convergence account] 详细描述了这一演进。

[reviewer convergence account]:
  https://www.latent.space/p/harness-eng#:~:text=gave%20it%20the%20flexibility%20to%20either%20defer%20or%20push%20back%20against%20review%20feedback

将评审输出视为证据。严重性阈值、明确的响应选择和停止规则使评审循环保持收敛，同时约束其在 agent 和人类注意力方面的成本。

## 让产品反馈改变工作

产品学习可以改变工作应产出的 artifact。当团队首次构建 Codex 应用的定时自动化 UI 时，实现将接口耦合到 cron 后端和 agent 核心；调度器的一部分甚至最终出现在前端 JavaScript 中。团队回退了那个纠缠的实现。[painted-door experiment] 将完整的调度接口置于空操作后端之上，并对产品界面进行了仪表化。该 artifact 可以在团队决定后端是否值得优先处理之前，测试人们是否理解并需要定时自动化。

[painted-door experiment]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=ended%20up%20with%20what%20we%E2%80%99re%20calling%20a%20painted%20door%20style

重大操作可以在原始工作继续进行的同时改进 harness。Loki 是一个运行开源 Valetudo 固件的扫地机器人。自定义的 Tailscale 守护进程是其唯一的远程访问路径。将该守护进程从 v1.90.8 升级到 v1.96.4 因此可能破坏恢复失败部署所需的路由。[robot-vacuum upgrade] 在同一台机器上添加了第二个 Tailscale 身份和独立状态；候选版本必须启动、加入 tailnet 并接受 SSH 才能替换生产版本。

第一次金丝雀运行教会了部署工具真实的边界。OpenSSH `scp` 尝试了 SFTP 但无法干净完成，因此工具学会了通过 SSH 流式传输二进制文件。它添加了 Tailscale 的 `--accept-risk=lose-ssh` 标志并隔离了金丝雀身份的已知主机状态。后来，`prod backup` 暴露了将复合远程脚本作为 `ssh ... sh -c <script>` 发送会破坏 `if ... then ... fi` 等 shell 结构的问题。工具切换为通过标准输入将脚本发送到 `sh -s`，添加了有针对性的回归测试，重新运行了验证，并继续了部署。

[robot-vacuum upgrade]:
  https://hyperbo.la/w/robot-vacuum-canary-tailscale/#:~:text=The%20deploy%20also%20improved%20the%20tool

## 保留成功和失败的工作作为证据

已接受和失败的工作可以保存简短指令无法枚举的决策和边界条件。

[Symphony distillation loop] 从成功的实现中提取规范，通过独立的重新实现来测试该规范，并利用差异来改进它。这之所以有效，是因为 [blessed work is information dense]。它包含许多简短指令无法枚举的决策。当与解释失败原因的轨迹和结果一起保留时，[Failed work as evidence] 变得有用。

[Symphony distillation loop]: https://x.com/_lopopolo/status/2048906262109413801
[blessed work is information dense]:
  https://x.com/_lopopolo/status/2054037448297201696
[Failed work as evidence]: https://x.com/_lopopolo/status/2063229530840690892

将这些 artifact 与其结果和来源一起保留。将反复出现的原则提升为普通上下文或工具；不要将原始归档转化为永久 prompt。

## 将 MLD 视为一个传感器

[MLD instruction] 要求 agent 在工作时记录错误、学习和愿望。Ryan 后来将其命名为 MLD 框架，并说这些文件是[为 agent 构建者准备的]。将报告视为遥测数据：harness 构建者根据轨迹验证它，并仅提升反复出现的、有充分支持的缺失项。

[MLD instruction]: https://x.com/_lopopolo/status/2037307669464363433
[for the agent builder]: https://x.com/_lopopolo/status/2052095157336703410

Agent 可能会错过其最重要的错误、错误识别原因或请求错误的工具。原始 MLD 仍然附属于产生它的运行。[MLD: Telemetry for the Harness Builder] 详细描述了收集、验证、提升、过期和隐私边界。

[MLD: Telemetry for the Harness Builder]: mld.md

## 在后续运行中闭环

当后续结果改善时——更少的人类传递、更少的反复引导、更短的修复循环、更强的证明、更低的运营风险或在相同质量标准下完成更广泛的工作——干预措施就值回了其维护成本。在保持 worker 条件不变的情况下重新运行可比任务，并移除效果在消融实验中未存活的上下文或工具。

[Evaluate the Harness] 在固定 worker 条件下隔离一个干预措施，将真相保留在目标处，并在结果边界处评估契约。[Optimize for Measured Effectiveness] 考虑了由此改进产生的人类注意力、延迟、风险、计算和维护成本。

[Evaluate the Harness]: ../../evals/
[Optimize for Measured Effectiveness]: ../effectiveness/
