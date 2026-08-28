# 让仓库教会 Agent

Agent 读取的每个文件都会影响它对下一个输出的预测。一个连贯的仓库为每个反复出现的概念只提供一个本地答案：一个权威的 owner、一个规范的模式，以及机械化检查的投影。Agent 可以从示例中推断出好的样子，在其他地方复用这一推断，并减少在判断哪个本地惯用法是最新时所需的注意力。

源代码在此循环中扮演两个角色。它是规格、边界和防护栏的下游实现输出。它也是下一条轨迹的输入。[Stop Treating Code as the Artifact] 描述了第一种关系；[code as prompts for future prompts] 命名了第二种关系。持久的 contract 塑造今天的代码，而今天的代码又成为明天的示例。

[Stop Treating Code as the Artifact]:
  https://hyperbo.la/w/code-is-not-the-artifact/#:~:text=The%20source%20tree%20still%20matters%20because%20future%20agents%20use%20it%20as%20a%20prompt%2C%20but%20it%20is%20one%20replaceable%20realization%20of%20the%20system.
[code as prompts for future prompts]:
  https://x.com/_lopopolo/status/2030460794765451300

## 代码是 prompt 的一部分

Agent 读取源代码以发现业务逻辑、adapter、状态管理选择、测试结构，以及团队表达熟悉操作的方式。在 [Aakash Gupta interview] 中，Ryan 直接描述了这一点："仓库中的代码就是 prompt。"统一的结构使在一个文件中学到的上下文可以跨 codebase 转移。Agent 可以更有效地模仿人类新人搜索先例、复制并修改的方式。

[Aakash Gupta interview]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=actually%20the%20code%20in%20the%20repository%20is%20prompts

一个具体的先例出现在 Ryan 2026 年 harness-engineering 文章之前的几年。在 [I Wrote 4,000 Lines of Code with ChatGPT in a Weekend] 中，当 Ryan 将一个合并的 `lib.rs`（Ruby 的 `String#unpack` 实现）输入到一个 prompt 中时，ChatGPT 继续混合无关的指令。他将 crate 拆分为具有高度规则结构和文档的小模块。这些重复的示例使模型在第一次尝试时就能正确实现额外的 state-machine 操作。同一篇文章设想了基于本地 codebase、commit 历史、issue 和 pull request 的 agent。后来 Ryan [revisited the article] 并表示，尽管模型已经改进，但上下文、约束、迭代和反馈的协作循环感觉仍然一样。

[I Wrote 4,000 Lines of Code with ChatGPT in a Weekend]:
  https://hyperbo.la/w/chatgpt-4000/#:~:text=implement%20these%20parts%20of%20the%20state%20machine%20without%20issue%20on%20the%20first%20attempt
[revisited the article]: https://x.com/_lopopolo/status/2032882451538977204

模型已经包含了许多构建软件的合理方式。仓库通过选择团队预期的选择来 [prunes that latent space]。

[prunes that latent space]: https://x.com/_lopopolo/status/2051411695873216842

## 使非功能需求可恢复

非功能需求是帮助判断一个结果是否属于系统的质量属性、约束和验收标准：兼容性、可维护性、可访问性、安全性、可靠性、性能和精细化。团队对如何解释、优先级排序、权衡和满足这些需求做出了通常未说明的选择，包括风险承受力、可接受的捷径，以及组织对"完成"的定义。历史上，它们通过组织结构、社会规范、招聘、入职以及与已经知道本地答案的同事反复接触来传递 [what doing a good job means]。

[what doing a good job means]:
  https://hyperbo.la/w/what-does-it-mean-to-do-a-good-job/#:~:text=tone%2C%20taste%2C%20risk%20tolerance%2C%20how%20much%20polish%20is%20enough%2C%20what%20shortcuts%20are%20acceptable

一个通用模型已经见过许多内部一致的答案来应对每个这样的选择。一个 [expert persona leaves the choice unresolved]，因为专家开发者可以对相同的非功能需求做出不同的、本地有效的决策。一个空白仓库为 [SDK design] 创造了同样的模糊性：没有本地先例，就没有东西可以修剪许多合理的语言、接口、兼容性和长期维护选择。模型已经知道许多好的选项；组织必须选择属于这个系统的那些。

[expert persona leaves the choice unresolved]:
  https://x.com/_lopopolo/status/2071843335749489068
[SDK design]: https://x.com/_lopopolo/status/2051411695873216842

Ryan 采用了 \[un\]prompted 会议的 [systems-level framing]，在其中 harness engineering 将整个非功能需求宇宙引入代码。[two-part context economy] 将这些需求放置在可以被检索到上下文中的地方，并减少 worker 产生高质量工作所需的上下文量。使它们可读和可检索成为 [a net-new function of the team]，相关需求以及时的方式提供。一个早期的调度器失败源于未指定的可扩展、可维护代码需求；[writing and injecting that missing specification] 正是 harness 的目的。

[systems-level framing]: https://x.com/_lopopolo/status/2028982729145237775
[two-part context economy]: https://x.com/_lopopolo/status/2030356581792293212
[a net-new function of the team]:
  https://tessl.io/registry/ainativedev/aidevcon-2026-ldn/0.100.8/files/talk-lopololo-harness-engineering/transcript.md#:~:text=make%20all%20these%20nonfunctional%20requirements%20of%20writing%20good%20software%20legible%20to%20the%20agent
[writing and injecting that missing specification]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=engineering%20had%20not%20yet%20fully%20specified%20all%20these%20non-functional%20requirements

适当的 owner 取决于需求的成熟度和范围。一个被认可的实现承载着关于结构和品味的密集选择。一个不被喜欢的结果可以被 [translated into its governing nonfunctional requirement]。团队可以记录其选择的解释，通过 reviewer 策略应用上下文判断，并通过测试或 lint 强制执行已确定的不变量。架构、类型、API、工具和 merge gate 可以表示和强制执行稳定的需求。示例和 reviewer 策略可以表达定性判断。版本化的指导和运行手册记录不断变化的运营需求以及满足它们的当前选择。每个表示都应该指向该需求及其本地决策的一个语义 owner，以便 agent 可以检索相关选择而无需将整个质量标准加载到每个 prompt 中。

[translated into its governing nonfunctional requirement]:
  https://rewiz.app/channels/%40openai/build-hour-api-codex#:~:text=work%20toward%20encoding%20those%20non-functional%20requirements%20in%20your%20codebase

## 一致性压缩上下文

Ryan 关于 [make all the code the same] 的指令涵盖了结构、模式、语言、构建、文档和技能。一致性降低了上下文和指令需求。一个可观测性栈、命令形状和包模式使 agent 能够跨仓库转移上下文。每个表达相同概念的变体都会迫使另一轮发现：哪个示例是规范的、迁移是否完成，以及哪些周围的规则也必须进入注意力。

[make all the code the same]: https://x.com/_lopopolo/status/2030325081868837216

[AI Native DevCon transcript] 用一个具体的对比展开了注意力机制。一个可观测性模式使来自仓库某一部分的上下文可以应用于其他地方；六个栈则需要 agent 研究哪个属于每个调用点。[AI Engineer Europe talk] 将降低的注意力需求与已完成的迁移联系起来：agent 可以标准化整个仓库，而不是留下两个时代的代码在活跃使用中。

[AI Native DevCon transcript]:
  https://tessl.io/registry/ainativedev/aidevcon-2026-ldn/0.100.8/files/talk-lopololo-harness-engineering/transcript.md#:~:text=if%20I%20have%20six%20of%20observability%20stacks%20in%20the%20code%20base
[AI Engineer Europe talk]: https://www.youtube.com/watch?v=am_oeAoUhew&t=466s

这是高效的 cargo-culting。一个 [accepted implementation] 体现了成千上万个难以指定的小决策。复用其好的部分为新的推出提供了一个 [nucleation point]。规范的重复减少了 agent 必须检查和保持在注意力中的竞争指令、迁移历史和兼容性路径。人类获得同样的好处：仓库使其偏好的延续对新贡献者变得明显。

[nucleation point]: https://x.com/_lopopolo/status/2051416715406483526
[accepted implementation]: https://x.com/_lopopolo/status/2051410617937125508

示例可以像传播良好判断一样容易地传播缺陷。[OpenAI harness-engineering essay] 观察到 Codex 会复制不均匀或次优的本地模式。一个半完成的迁移因此会产生竞争的 prompt，而一个糟糕的 helper 可以传播到每个需要类似行为的包中。黄金原则、共享原语和反复清理使示例集再次变得可信。

[OpenAI harness-engineering essay]:
  https://openai.com/index/harness-engineering/#:~:text=Codex%20replicates%20patterns%20that%20already%20exist%20in%20the%20repository

## 一个概念，一个权威 owner

在创建解析器、manifest 类型、版本常量、状态表示、fixture 构建器、策略检查或命令之前，找到该概念的现有 owner。在必要时扩展该 capability seam。

一个概念可以有多个物理表示，同时保留一个语义 owner。一个版本可能出现在源代码、生成的文档、pull request 和自动化状态中。从 owner 生成这些投影，从规范源解析它们，或机械化检查它们的关系。稳定的策略——精确固定、冷却期或已批准的依赖类——即使当前版本在 manifest 中发生变化，仍然是代码。

仓库拥有期望的配置和持久策略。运行中的服务、外部控制平面、硬件设备或部署系统拥有其活动状态。工具和检查协调这些视图，而不会将易变状态复制到第二个所谓的真相来源中。

警示信号包括：

- 仓库已经建模的语言的第二个解析器；
- 精确的工具版本被复制到策略代码和 fixture 中；
- 从字符串片段组装的 JSON 或 YAML；
- 重新实现领域行为的命令；
- 无类型的外部数据流入内部代码；以及
- 保护相同不变量的多个防御性检查。

每个重复的 owner 都会创建另一个本地合理的延续。Agent 可以忠实地遵循每一个附近的先例，但仍然使仓库变得不够连贯。

## 完成迁移并安装棘轮

当实现充足时，一致性变得更容易实现。[The Production Function Changed] 描述了启用 ESLint 的 `no-await-in-loop` 规则、修复大约 600 个违规，并在一个 pull request 中添加穷尽测试覆盖。在一个连贯的变更中完成迁移消除了矛盾的先例。

[The Production Function Changed]:
  https://hyperbo.la/w/production-function-changed/#:~:text=I%20turned%20on%20no-await-in-loop%20from%20ESLint,exhaustively%20add%20test%20coverage

完成迁移与选择目标同样重要。将新旧模式并列会使其都可用作 prompt 材料。完成的迁移消除了这种模糊性，而棘轮将新模式转变为持久的上下文。Agent 也使维护变得更便宜：反复的园艺可以发现偏差、迁移它们，并在漂移积累之前将已接受的修正反馈到仓库中。

绿地迁移可以故意阻止旧系统提供最简单的延续。Ryan 描述了一种探索性模式，将 Python 包分解为 PyO3 边界后面更小的 Rust 包。语言和包边界阻止了对现有"大泥球"的随意依赖，而 Rust 类型系统使更多的目标结构可强制执行。Ryan 将 [the brownfield pattern] 作为关于故意不连续性的探索性证据；其价值取决于迁移及其创建的边界。

[the brownfield pattern]: https://x.com/_lopopolo/status/2054955223609680303

[hyperbo.la build case] 在一个真实的仓库中应用了这一操作：一个工具链和 workspace 图、显式的包 owner、语义化的 URL 和 frontmatter 类型、一个渲染路径、自定义结构检查，以及一次性的内容迁移。仓库变得更容易让下一个 agent 推断，因为迁移没有留下竞争的构建或内容路径。

[hyperbo.la build case]: hyperbola.md

## 在边界处解析不确定性

在接受从真实外部系统进入的数据时接受不确定性。将其解析为受信任的领域类型一次，然后让内部代码依赖规范状态。重复的记录检查、每个函数中的规范化、每个事件处理器中的就绪检查，以及多个过期响应守卫通常意味着没有边界拥有有效性。

[Parse, don't validate] 提供了早期的类型设计原则：解析应该保留值满足程序需求的证据。Ryan 后来的 harness 工作将该原则转化为面向模型的结构，使用 [branded identifiers]、[unit-bearing duration types]、[boundary-only decoding]，以及用于类型系统无法防止的逃生舱口的 AST 感知 lint。

[Parse, don't validate]:
  https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/#:~:text=discharging%20checks%20on%20input%20up-front%2C%20right%20on%20the%20boundary
[branded identifiers]: https://x.com/_lopopolo/status/2076191023500611843
[unit-bearing duration types]:
  https://x.com/_lopopolo/status/2076192816477425981
[boundary-only decoding]: https://x.com/_lopopolo/status/2076200856681357613

## 机械化编码所有权

命令将参数、环境和退出行为适配到拥有该操作的类型化 API。架构文档命名包所有权和依赖方向。结构测试强制执行这些关系。自定义诊断解释控制模型并将 agent 引导至适当的修复，以便失败的检查在有用时提供缺失的上下文。

架构还限制了变更所需的上下文。当一个子系统具有文档化的、可靠的接口时，agent 可以从该 contract 推理，并将大部分实现细节留在活动注意力之外。[Capability-shaped boundaries] 使相同的操作可执行：调用者依赖 capability，而其背后的实现可以在不让每个消费者将迁移加载到上下文中的情况下进行更改。

[Capability-shaped boundaries]: ../lineage/#capability-and-migration-seams

Fixture 应通过类型化构建器表达领域条件，并仅在格式边界处序列化。生成的 artifact 应该有一个显式的 source-to-output 模型。它们的证明可能比较字节、语义或完整语料库，具体取决于 contract；[proof thesis] 展开了这一区别。

[proof thesis]: ../proof/

[architecture cases] 在 Ryan 的 homelab、Artichoke 和 `rand_mt` 中展开了这些机制。

[architecture cases]: implementations.md

## 保留有意义的差异

一致性适用于多个地方表达相同反复出现的概念时。原型和生产工作具有 [different nonfunctional requirements]，因此它们的模式无需匹配。它们不是单个操作 contract 内的竞争答案。规范性局部于相关领域和范围。在该范围内，一个语义 owner 可以有意地产生多个表示。

[different nonfunctional requirements]:
  https://tessl.io/registry/ainativedev/aidevcon-2026-ldn/0.100.8/files/talk-lopololo-harness-engineering/transcript.md#:~:text=what%20it%20means%20for%20something%20to%20be%20a%20prototype%20versus%20production%20feature

仓库特定的规则应该通过具体的失败类别或有价值的不变量来证明其承载成本。广义架构建立稳定的 seam；本地实现对 agent 的推理保持开放。这使示例集保持连贯，而不会将每个设计选择都变成集中的策略。

结果是一个增强循环。规范示例帮助 agent 以更少的上下文做出对齐的变更。Agent 使完整的迁移和持续的园艺变得可负担。这些改进提高了每个未来轨迹可用的示例质量。
