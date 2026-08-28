# 在结果边界度量有效性

通过有价值且经过验证的结果来度量有效性。将这些结果与产生和维护它们的全部成本和风险并列。人工注意力、经过的时间、计算、返工、后果和未来的持有成本描述了不同的约束。将它们作为单独的维度保持可见；一个混合分数会掩盖团队需要管理的权衡。

## 度量被接受的结果

从用户、操作者、依赖系统或决策者体验结果的地方开始。根据工作的不同，被接受的结果可能是交付的用户价值、经过验证的安全修正和公告、安全完成的升级、避免过早构建的经过验证的决策，或者是改变后续工作的持久能力。

[Prove a Security Claim] 跟踪了一份报告经过影响分析、修正、发布和公告的过程。[The robot-vacuum upgrade] 只有在升级后的机器保持可达并为下一次操作留下经过测试的路径后才关闭。[painted-door experiment] 开始于一位设计师探索了后来出现在 Codex 应用中的定时任务交互。第一个实现将新的 cron 后端耦合到 agent 核心，并一度将调度逻辑放在前端 JavaScript 中。团队回滚了该实现，保留了在 no-op 后端上的完整用户界面，并对表面进行了检测以了解用户是否理解并想要该功能。被接受的结果是关于是否优先处理后端的产品决策的证据。

[Prove a Security Claim]: ../proof/rustsec.md
[The robot-vacuum upgrade]:
  https://hyperbo.la/w/robot-vacuum-canary-tailscale/#:~:text=After%20those%20gates%2C%20Codex%20ran%20the%20runbook%20end%20to%20end
[painted-door experiment]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=ended%20up%20with%20what%20we%E2%80%99re%20calling%20a%20painted%20door%20style

一个 pull request、行数、计划、token 总量或生成的 artifact 可以贡献于一个结果。它们都不能单独确立价值。询问这项工作 [ladders up to] 什么、它服务于谁、失败的成本是什么，以及它如何改变团队未来的速度。[whole-job boundary] 及其证明应该在代理指标开始看起来像成功之前被声明。

[ladders up to]: https://x.com/_lopopolo/status/2055111894181335279
[whole-job boundary]: ../whole-job/

## 计算完全加载的人工注意力

实现成本可能下降，而方向、接力、审查、协调、验证、恢复和维护的成本仍然存在。计算提示和引导、复制粘贴接力、上下文切换、CI 和合并看护、冒烟测试、同步会议、升级和事件恢复——覆盖整个工作。从实现中移除的时间可能以审查负担或未来清理的形式重新出现。

[The Production Function Changed] 描述了更便宜的全仓库实现将人工注意力转向选择不变量和要求证明。[Software Work Is No Longer Scheduled] 将相同的经济学应用于具有明确期望状态、已理解和可验证结果的有界工作，同时为新产品、困难接口和其他未解决的形态保留注意力。

[The Production Function Changed]:
  https://hyperbo.la/w/production-function-changed/#:~:text=The%20scarce%20work%20is%20choosing%20the%20invariant%20and%20deciding%20whether%20the%20proof%20is%20sufficient.
[Software Work Is No Longer Scheduled]:
  https://hyperbo.la/w/software-work-not-scheduled/#:~:text=Once%20the%20decision%20has%20been%20made%20and%20success%20can%20be%20specified%2C%20I%20would%20rather%20hand%20execution%20to%20agents%20than%20make%20the%20change%20compete%20for%20a%20place%20on%20the%20roadmap.

Ryan Lopopolo 的 [2023 ChatGPT pairing account] 直接记录了早期的循环：代码在编辑器和浏览器之间复制，测试手动传回聊天，会话包含数百个提示，五分钟的编译修复，以及一小时后变得精神紧张的工作。在后来的 [60-hour refactor account] 中，他描述了职业生涯中最困难的重构之一，他估计手工完成需要三周时间。Codex 运行了 60 小时，跨越三到四天，产出了一个 pull request，只需要初始提示加上两次后续提示。[run summary] 和后来的采访将其成本定在大约 3.3-3.5 亿个 token 和 2,800 美元。挂钟时间增长了，而与人工的同步接口显著变小。

[2023 ChatGPT pairing account]:
  https://hyperbo.la/w/chatgpt-4000/#:~:text=I%20interacted%20with%20ChatGPT%20by%20copying%20code%20in%20vim
[60-hour refactor account]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=this%20is%20a%20PR%20that%20probably%20would%20have%20taken%20me%20three%20weeks
[run summary]: https://x.com/_lopopolo/status/2031207017919209690

这些运行在模型、任务、工具和环境上各不相同，因此不能确立趋势。它们确实展示了为什么挂钟时间和人工注意力需要分别的时钟。跟踪人们花费时间的地方，然后使用 worker 及其环境来 [remove recurring human work]，而不是将其转移到另一个阶段。

[remove recurring human work]:
  https://x.com/_lopopolo/status/2051445312049553642

## 保持四个时钟

"延迟"描述了几个不同的循环：

| 时钟                       | 它度量什么                                            | 为什么重要                                                |
| -------------------------- | ---------------------------------------------------- | -------------------------------------------------------- |
| worker 反馈延迟            | 从操作到有用的测试、构建、跟踪或审查信号              | 控制迭代速度                                              |
| worker 挂钟持续时间        | 从开始到完成的运行                                   | 占用执行容量并可能延迟依赖工作                            |
| 同步人工注意力             | 一个人必须指导、接力、审查或抢救的分钟数               | 决定组织可以监督多少工作                                  |
| 到被接受结果的时间          | 从请求到经过验证、接受或部署的结果                    | 一起捕获队列、重跑、审查和交付                            |

快速的本地反馈和长时间运行可以共存。在 GPT-5.2 下，Codex harness 没有后台 shell，因此团队可以依赖阻塞构建脚本。在 GPT-5.3 添加后台 shell 后，Ryan 发现 worker 不太愿意等待阻塞构建。在一周内，团队从定制的 Makefile 经过 Bazel 和 Turbo 转向 Nx，一旦 [build loop completed in under roughly one minute] 就停止了。那一分钟是一个大致的上限：更慢的构建触发了构建图的另一次分解。上面的 60 小时重构展示了为什么这个反馈循环和总 worker 持续时间需要分别的时钟。

[build loop completed in under roughly one minute]:
  https://www.latent.space/p/harness-eng#:~:text=over%20the%20course%20of%20a%20week%2C%20we%20went%20from%20a%20bespoke%20make%20file%20build%20to%20Basil%2C%20to%20turbo%20to%20nx

当一个内部应用程序的前十几位用户报告性能问题时，团队要求他们导出一个 trace tarball。一位值班工程师花了一个下午与 Codex 合作，为 trace 构建了一个精美的本地 Next.js 查看器。然后团队将 [the same tarball directly to give to Codex]，它在大约五分钟内返回了请求的分析。直接检查完成了这项工作；只有当 Codex 需要一个查看器来获取答案时，专门的查看器才值得拥有。

[the same tarball directly to Codex]:
  https://www.latent.space/p/harness-eng#:~:text=you%20could%20just%20spin%20up%20codex%20and%20give%20it%20the%20tar%20ball

小型 harness 更改可以独立改变一个时钟。当长时间运行的数据查询返回 pending 响应时，Codex 在每次轮询之前花时间进行推理。一个在 pending 响应上 sleep 的指令产生了报告的 [threefold wall-clock reduction]，而没有改变底层查询。

[threefold wall-clock reduction]:
  https://x.com/_lopopolo/status/2052066301821255929

## 区分可寻址性、活动性和生产利用率

Worker 只能通过其可以到达的工作部分来创造价值。仓库访问、本地应用程序、测试、浏览器、日志、跟踪、部署和证明增加了**可寻址性**。Token 和繁忙时间度量**活动性**。**生产利用率**是达到被接受结果或产生可重用证据的活动。可预防的丢弃、审查和修复降低了它。

[Agent Utilization Is the New Performance Ceiling] 使用高利用率作为暴露生命周期中不可访问部分的强制函数。空闲时间可能揭示缺失的上下文、工具、权限、可观察性、审查或交付。繁忙的 worker 也可能以高速产生被拒绝的补丁或维护负担。在后来的 [Tessl interview] 中，十亿 token 的框架获得了一个结果门控：工作必须到达 main 并改善用户体验。

[Agent Utilization Is the New Performance Ceiling]:
  https://hyperbo.la/w/agents-agents-agents/#:~:text=A%20billion%20tokens%20per%20engineer%20per%20day%20is%20a%20utilization%20target
[Tessl interview]:
  https://tessl.io/podcast/109/#:~:text=that%20make%20it%20to%20main%20and%20improve%20the%20end%20experience

[Token leaderboards] 可能奖励一个在价值停滞时上升的信号。[Spend unanchored from value] 很少说明 ROI；被接受的结果使 [effectiveness the objective]。

[Token leaderboards]: https://x.com/_lopopolo/status/2035101303140167745
[Spend unanchored from value]:
  https://x.com/_lopopolo/status/2035102053979271654
[effectiveness the objective]:
  https://x.com/_lopopolo/status/2035132736621748332

[Please Go Brr, on Token Mandates] 将广泛的 token 预算视为一种组织搜索策略。在评估边界，token 量记录了搜索工作。记录搜索发现了哪些情境化的工作、从业者、缺失的能力和可重用的部署模式。被接受的结果和持久的部署知识决定了哪些发现值得持续投资。

[Please Go Brr, on Token Mandates]:
  https://hyperbo.la/w/token-mandates/#:~:text=organizational%20discovery%20mechanism%20for%20learning%20how%20to%20deploy%20reasoning%20compute

## 区分探索、返工和浪费

当一次尝试解决了对后续工作重要的不确定性时，丢弃可以是生产性的。上面的定时任务实现在团队有用户想要后端的证据之前暴露了一个架构故障。在保留已检测接口的同时回滚它，将产品发现与被丢弃的实现分离开来。其他失败的尝试可以比成功的补丁更密集地暴露缺失的约束、工具或评分器。

当一个高模糊度重构的最终接口形态仍然未知时，Ryan 描述了一种 [large disposable-probe pattern]。他让 agent 生成一个可能达到大约 50,000 行的探索性 diff，推送 pull request 以便工作可以被检查，然后丢弃它。该尝试揭示了 worker 可能失败的地方以及最终切换需要哪些接口接缝。他将该地图转化为大约十五个准备和暂存 pull request，然后返回到切换。探索性实现是临时的；失败地图以及准备和暂存更改是他带入后续切换的内容。

[large disposable-probe pattern]:
  https://tessl.io/podcast/109/#:~:text=50%2C000%20line%20of%20diff%20PR%20and%20throw%20it%20away

当探索性工作可能被丢弃时使用 [an explicit rollback point]。当团队保留它暴露的约束、接缝或评分器时，一次失败的尝试变得 [information dense]。当该尝试改变了决策、改善了环境或使下一次成功更容易时，价值就出现了。被丢弃的 diff 只有在后续工作可以恢复它揭示的内容时才会累积。

[an explicit rollback point]: https://x.com/_lopopolo/status/2029085149749756028
[information dense]: https://x.com/_lopopolo/status/2063229530840690892

计算尝试次数、CI 周期、审查者周期、回滚和重复的故障类别。在没有新证据的情况下重复已知故障是浪费。一个不断引发相同高信号指导的 harness 即使每次单独的修正成本很低，也存在纵向缺陷。

重复的故障类别属于环境。通过 [Turn Feedback Into Infrastructure] 将它们提升到上下文、工具、测试、审查者或架构中。[Evaluate the Harness] 对轨迹和被接受的结果都进行定价。

[Turn Feedback Into Infrastructure]: ../feedback/
[Evaluate the Harness]: ../../evals/

## 对终身成本定价

所有权账本从模型计算和人工注意力开始，然后通过证明、恢复、运营和维护继续。运营暴露和后果与产生的成本并列可见。廉价的生成降低了探索和实现成本，而仓库保留了由更改产生的义务。

Dependency 内部化使权衡变得具体。移除一个外部包可以减少供应链表面，同时将兼容性、安全、性能和标准维护转移到仓库中。[Ryan's homelab Markdown implementation] 用一个自有解析器、AST、渲染器和清理器以及语料库、golden、安全、取消、错误和覆盖证明接受了这种转移。当仓库无法维持这些义务时，高质量的解析器或编解码器可以是较低风险的所有者。

[Ryan's homelab Markdown implementation]: ../domain-modeling/homelab.md

跟踪 dependency 和工具数量、升级工作、策略复杂性、标准漂移、事件暴露和未来更改成本。计算属于相同的会计范围：token、推理成本、CPU 或 GPU 时间、CI 分钟和存储是真实的输入，即使模型预算刻意慷慨。

## 纵向度量复利

当可比工作的改进足以覆盖构建和维护它的成本时，harness 就赚取了其持有成本。记录 harness 版本，并在一个 worker 时代内比较类似工作的仪表板。

OpenAI 团队从一个空仓库开始 [an internal software product]，并将其开发了五个月作为公司内部使用的 beta。Codex 生成了其大约一百万行代码中的全部；团队报告了约 1,500 个合并的 pull request，没有手工编写的代码。[Early progress was slow]，因为仓库缺少 agent 需要的工具、抽象和结构。[Throughput rose as the team and harness grew]，因此后续工作继承了由早期工作编码的能力和判断。Robot-vacuum 案例为下一次升级留下了一条经过记录和测试的既定路径。这些纵向声称询问可比的后续工作是否继承了足够的杠杆来证明 harness 的持有成本。

[an internal software product]:
  https://openai.com/index/harness-engineering/#:~:text=Five%20months%20later%2C%20the%20repository%20contains%20on%20the%20order%20of%20a%20million%20lines%20of%20code
[Early progress was slow]:
  https://openai.com/index/harness-engineering/#:~:text=Early%20progress%20was%20slower%20than%20we%20expected
[Throughput rose as the team and harness grew]:
  https://openai.com/index/harness-engineering/#:~:text=surprisingly%20the%20throughput%20has%20increased%20as%20the%20team%20has%20grown

当模型或 coding agent 发生重大变化时，开始一个新的 worker 时代，并重新评估上下文、工具、雄心和运营先验。比较类似的工作。否则，更强的 worker 或更容易的任务组合可能伪装成 harness 改进。[Hold the Worker Constant] 定义了 worker 边界。

[Hold the Worker Constant]: ../fixed-worker/

## 保持各维度可见

对于每一个被接受的结果，记录：

| 维度       | 问题                                                       | 代表性证据                                                  |
| ---------- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| 结果       | 用户、运营、业务或决策发生了什么变化？                      | 任务特定的验收和已部署行为                                   |
| 注意力     | 整个工作消耗了多少人工时间？                                | 引导、接力、审查、QA、协调、恢复分钟数                       |
| 流程       | 哪个时钟限制了完成？                                        | 反馈、worker 持续时间、人工和验收百分位                      |
| 返工       | 什么被丢弃、重复或学习了？                                  | 尝试次数、回滚、CI 和审查者周期                             |
| 风险       | 什么可能失败，信心是如何建立的？                            | 与声称匹配的证明、事件、回滚、恢复                           |
| 终身       | 增加或移除了哪些未来义务？                                  | dependency、工具、策略、升级和更改负担                       |
| 计算       | 被接受的结果消耗了什么？                                    | token、推理成本、CPU/GPU 时间、CI 分钟、存储                 |
| 复利       | 可比工作在 worker 时代内是否有所改进？                      | 按 harness 版本和匹配任务类别的趋势                          |
