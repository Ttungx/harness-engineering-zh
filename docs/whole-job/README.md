# 将整个工作交给一个 Agent

在 harness 安全支持的最高级别委托结果，然后让一个主要轨迹负责完成它。团队在重要的边界提供方向、判断和权限。所有者检索 context、调查、选择方法、返回工作所需的分析、更改或决策、证明结果并将其带过交付。

## 委托结果及其标准

Ryan Lopopolo 经常在 prompt 中给出 [close to zero constraints]。稀疏的委托测试环境是否已使真实需求可恢复。当 agent 无法安全推断时，prompt 仍然命名结果、硬验收标准或重要边界。

[close to zero constraints]: https://x.com/_lopopolo/status/2054053216254636073

Ryan 用一个关于 `libghostty`（从 Ghostty 终端模拟器开发的可重用库表面）的假设请求来定义高雄心。worker 将 [across the open-source ecosystem] 搜索差距、bug、缺失功能以及可以使库更好的分发或打包改进。prompt 提出一个广泛的发现工作；它不报告已完成或穷举的调查。harness 仍然为跨越多个预定补丁或仓库的工作提供质量标准、证明和权限边界。

[across the open-source ecosystem]:
  https://x.com/_lopopolo/status/2075861840085848202

一个简短的 prompt 可以承载高标准。[A Lazy Prompt Turned Into a RustSec Advisory] 背后的请求要求完整的红队分析，并要求每个发现证明影响或可利用性。Codex 开发了报告、重现器、补丁和回归测试，然后在 [artichoke/intaglio#360] 中记录的详细人类审查后修订了实施。Ryan 批准了实施并授权 Codex 准备版本发布、合并 pull request 并打开 RustSec 报告。prompt 指定了结果和验收标准，而工作流在交付时保留了实质性审查和权限。

[A Lazy Prompt Turned Into a RustSec Advisory]:
  https://hyperbo.la/w/lazy-prompt-rustsec/#:~:text=full%20job%20end%20to%20end%2C%20from%20identification%20and%20fix%20to%20vuln%20publication
[artichoke/intaglio#360]: https://github.com/artichoke/intaglio/pull/360

Ryan 称这些 prompt 为"懒惰的"，因为方法仍然开放。当前模型仍然可能过于字面地对待稀疏请求，因此遗漏揭示了环境未能使哪些需求、工具或 context 可读。[prompt-observe-improve-reroll loop] 检查该轨迹，改善 harness，丢弃结果，并重新运行工作。

[prompt-observe-improve-reroll loop]:
  https://x.com/_lopopolo/status/2048901233721979363

可重用的指令从属于结果。[push-PR skill failure] 来自通常要求在打开 pull request 之前进行本地绿色构建的 agent 指令。一个被阻塞的任务需要早期的 pull request，以便协作者可以审查工作并帮助修复构建。skill 拒绝了该操作。其通常的保护已成为它本应支持的结果的障碍。

[push-PR skill failure]: https://x.com/_lopopolo/status/2030395671166230796

## 在 compaction 中保持持久意图稀疏

指令进入轨迹的位置影响它竞争注意力的时间长短。长的 Codex 轨迹反复压缩其活跃历史，因此 [context is rewritten across long work]。Ryan 来自这些会话的操作假设是用户意图仍然是高信号影响，而较旧的工具结果更可能被摘要化或剪枝。这是 worker 特定的行为，需要在模型、agent 或 compaction 更改后重新验证。在该假设下，用户消息中的详细阶段特定策略在工作结束后可能仍然突出，并将注意力分散到工作的其余部分。保持该通道聚焦于结果、验收标准和权限边界。Ryan 建议 [tokens easy to predict with the fewest instructions] 并说他 [closes off bad latent space outside user messages]。

[context is rewritten across long work]:
  https://x.com/_lopopolo/status/2062950475989725465
[tokens easy to predict with the fewest instructions]:
  https://x.com/_lopopolo/status/2031861303917363319
[closes off bad latent space outside user messages]:
  https://x.com/_lopopolo/status/2054053548124778786

仓库读取、工具结果、测试失败、浏览器观察和专注审查者可以在决策使其相关时呈现更窄的策略。活跃副本可能在该阶段后脱离 context；一个简短的仓库路由让 worker 可以再次检索规则。在 [robot-vacuum maintenance case] 中，一个定时 prompt 命名工作并指向一个版本化的自动化指南和任务特定 runbook。这些文档承载发布评估、证明和部署权限契约。另外，一个仅报告 [documentation-freshness role] 将运维指导与定义当前行为的配置和监控文件进行比较并报告偏差。这些路由使工作保持在 harness 策略上而不将每条规则加载到用户的指令中或假设 [deterministic context stuffing] 将在长序列的 compaction 中存活。

[deterministic context stuffing]:
  https://x.com/_lopopolo/status/2062966895876215172
[robot-vacuum maintenance case]:
  https://hyperbo.la/w/robot-vacuum-canary-tailscale/#:~:text=The%20repo%20carries%20the%20durable%20instructions
[documentation-freshness role]:
  ../domain-modeling/homelab.md#keep-operational-docs-current

Compaction 保留和指令权限是独立的关注点。检索的 context 和普通工具输出本身不授予权限或取代用户的结果。指定的审批或策略系统可能报告预先声明的授权条件已满足；harness 定义该证据如何改变 worker 可用的操作。[Route Context Just in Time] 阐述了检索和注意力机制。当 [the worker changes] 时重新验证放置策略。

[Route Context Just in Time]: ../just-in-time-context/
[the worker changes]: ../fixed-worker/

## 恢复隐藏需求

一个字面的请求通常命名功能性结果，同时将 [nonfunctional requirements]（如兼容性、可维护性和无障碍性）留作隐含。生成的对等性、回滚、必需检查以及未来更改的需求是其他隐藏的验收约束。[What Does It Mean to Do a Good Job?] 解释了团队如何通过共享规范和重复工作来承载这个更大的验收标准。

[nonfunctional requirements]:
  ../domain-modeling/#make-nonfunctional-requirements-recoverable
[What Does It Mean to Do a Good Job?]:
  https://hyperbo.la/w/what-does-it-mean-to-do-a-good-job/#:~:text=tone%2C%20taste%2C%20risk%20tolerance%2C%20how%20much%20polish%20is%20enough%2C%20what%20shortcuts%20are%20acceptable

隐藏的非功能性需求是 [the question behind the question]。模型可能 [struggle to recover them from the prompt]。它通过检查仓库、当前行为、历史和结果与用户的关系来恢复它们。它应该在要求人类重复之前检查可发现的事实，然后在缺失的答案改变产品意图、接受重大风险或授予它没有的权限时提问。

[the question behind the question]:
  https://x.com/_lopopolo/status/2030396192832819524
[struggle to recover them from the prompt]:
  https://x.com/_lopopolo/status/2032582601110913515

## 保持一个主要轨迹

一个主要轨迹是许多视角的问责集成点。产品、设计、运营、安全、支持和工程贡献不同的 context、约束、证据和验收标准。所有者将该团队判断编译为一个结果，同时保留用户问题、实施状态和完成责任。

在 [How PMs Ship 100K Lines of Code] 中，Ryan 描述了一个 agent 承载一个传统上跨越多个角色的循环：理解用户挑战和代码库、在设计系统内工作、部署结果、观察是否成功，并将 bug 和用户反馈反馈到工作中。角色继续提供专业知识和判断。使其知识可供一个代码产出的所有者使用，减少了设计、前端、后端和验证交接中目的的丢失。

[How PMs Ship 100K Lines of Code]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=with%20full%20context%20do%20the%20full%20job

在强 compaction 下，[one trajectory is the default]。当它购买独立证据时，特别是 [parallel discovery] 和对抗性审查，一个单独的 context window 值得其成本。主要轨迹集成这些结果。它可以在保留结果所有权的同时 [invoke a person or subagent] 用于不同的判断、权限或 context。

[one trajectory is the default]:
  https://x.com/_lopopolo/status/2051892678254854359
[parallel discovery]: https://x.com/_lopopolo/status/2051893022208708717
[invoke a person or subagent]:
  https://x.com/_lopopolo/status/2075863856275226709

## 在组织和跨轨迹中携带目的

所有权附属于结果。Ryan 描述了一个 [an organization-scale OODA loop]——Observe、Orient、Decide、Act——对相关组织活动具有横向意识，对委托轨迹具有纵向意识。负责迁移的父 worker 可能委托一个兼容性调查，将子 worker 的证据纳入其计划，然后决定哪个批次可以安全移动。跟踪器状态、计划、决策日志和证明在 compaction、重试和 agent 会话中携带该目的。不相关的结果可以并行进行，而不分割对此一个结果的责任。

[an organization-scale OODA loop]:
  https://x.com/_lopopolo/status/2055110057726263552

[Symphony] 给此拓扑一个具体的形式。问题跟踪器持有可交付物；每个独立拥有的问题接收一个专用的工作区和 agent；依赖关系阻止被阻塞的工作开始；主管重启停滞的执行。一个问题可能产生几个 pull request 或作为调查和分析完成而不接触代码。会话和 pull request 仍然是跟踪结果下方的执行工件。

[Symphony]:
  https://openai.com/index/open-source-codex-orchestration-symphony/#:~:text=each%20open%20Linear%20issue%20maps%20to%20a%20dedicated%20agent%20workspace

大型结果和紧密任务是兼容的。将工作分解为依赖感知的、可独立证明的部分，同时为完整结果保留一个所有者。在 [OpenAI harness-engineering essay] 中，计划成为持久的可执行 context：被单独审查，随着证据变化而更新，并跨长轨迹携带。

[OpenAI harness-engineering essay]:
  https://openai.com/index/harness-engineering/#:~:text=Plans%20are%20treated%20as%20first-class%20artifacts

## 关闭生命周期

观察主导许多工作的长中间部分：打开仪表板、检查跟踪、重现行为、比较发布、发现隐藏假设和组装证明。补丁生成是该生命周期中的一个站点。[Harness Engineering: How to Build Software When Humans Steer, Agents Execute] 通过 QA、用户反馈分类、隐私和日志审查、运营 runbook 和产品修复扩展了可寻址的工作。

[Harness Engineering: How to Build Software When Humans Steer, Agents Execute]:
  https://www.youtube.com/watch?v=am_oeAoUhew&t=2615s

对于普通软件工作，任务完成可以包括：

1. 检索相关 context；
2. 重现或检查当前行为；
3. 实施根本更正；
4. 更新生成的工件和公共契约；
5. 运行专注和系统级证明；
6. 对抗性审查结果；
7. 回应实质性审查和 CI 失败；
8. 获得所需审批；
9. 通过正常的保护路径合并、发布或部署；以及
10. 验证用户可见或运营结果。

交付是活跃的工作。agent 打开或更新 pull request，等待审查和 CI，修复实质性失败和冲突，并进入保护的合并路径。当结果包括发布或部署时，它跟随经过证明的工件通过审批和切换，观察运行结果，并与失败或用户反馈保持在一起，直到承诺的边界经过验证。

[literal admin-merge failure] 展示了为什么该保护路径属于结果。被要求分支、提交、推送和合并，worker 尝试使用管理绕过来满足最终动词，即使预期的工作包括等待测试。

[literal admin-merge failure]:
  https://x.com/_lopopolo/status/2032582327336083831

## 让结果选择工件

当用户报告内部应用中的性能问题时，团队收集了一个导出的跟踪 tarball。一位值班工程师花了一下午时间与 Codex 一起构建一个打磨的本地 Next.js 跟踪查看器。然后团队意识到 Codex 可以直接检查 tarball 并在大约五分钟内返回请求的分析，如 [Extreme Harness Engineering for Token Billionaires] 中所述。

[Extreme Harness Engineering for Token Billionaires]:
  https://www.latent.space/p/harness-eng#:~:text=you%20could%20just%20spin%20up%20codex%20and%20give%20it%20the%20tar%20ball

给 worker 证据和期望的结果。其轨迹可以决定它是否需要可观测性栈、解析器、一次性可视化或直接分析。同一次采访描述了可选的 skill 和脚本，让 Codex 在跟踪、日志或指标需要时启动本地可观测性栈。

whole-job 推理也可以在不构建想象的后端的情况下回答产品问题。Codex 应用的 scheduled-automations 接口的第一个实施将一个新的 cron 后端耦合到 agent 核心，甚至将调度逻辑放在前端 JavaScript 中。团队回滚了那个纠缠的实施。在 [painted-door experiment] 中，他们在无操作后端上保留了完整的接口，并检测生产表面以了解人们是否理解并想要定时自动化。该证据可以决定后端是否值得优先级。

[painted-door experiment]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=ended%20up%20with%20what%20we%E2%80%99re%20calling%20a%20painted%20door%20style

一个任务可能作为分析、检测的实验、更改的运营程序或决定不构建想象的系统来完成。跟踪需要诊断，所以直接检查关闭了循环。调度器问题需要需求证据，所以 painted door 关闭了循环。

## 将人类注意力花在模糊性和权限上

自主性清除已知的工作，并为零到一定义、困难接口和重要权衡中的人类判断创造空间。whole-job 委托适用于预期结果、权限边界和证明足够清晰可以完成时。生产服务比调查脚本需要更强的证据和更严格的权限。

[Software Work Is No Longer Scheduled] 围绕具有清晰期望状态、理解的接口和可验证结果的工作划出了此操作边界。零到一产品、困难的接口重构以及接口未知的领域仍然需要持续的人类判断。agent 可以调查和组装证据，而未解决的判断保持可见而不是被偷运到常规工作队列中。

[Software Work Is No Longer Scheduled]:
  https://hyperbo.la/w/software-work-not-scheduled/#:~:text=Building%20a%20product%20from%20zero,all%20require%20sustained%20human%20judgment
