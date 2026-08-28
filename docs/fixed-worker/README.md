# 保持 Worker 不变

Harness engineering 是通过塑造 agent 周围环境来改善 agent 输出的实践，它在更改上下文和工具的同时保持选定的模型和编码 agent 不变。这种不变性持续一个评估或部署周期。对模型、编码 agent 运行时或原生操作接口的更改会开启一个新周期，并需要重新验证环境。

[coding-agent boundary] 和 [game-engine distinction] 定义了黑盒。模型和编码 agent engineering 产出 worker。Harness engineering 将选定的 worker 部署到特定的工作主体中。

[coding-agent boundary]: https://x.com/_lopopolo/status/2051039234346438723
[game-engine distinction]: https://x.com/_lopopolo/status/2051058498235195856

在该边界内，上下文为 worker 提供意图、本地本体论、当前状态、约束、示例和良好工作的定义。工具使其能够在该上下文重要的系统中搜索、检查、转换、运行、观察和交付。环境通过仓库拓扑、架构、权限、测试、日志、评分器和金丝雀来策划这两者。[Route Context Just in Time] 和 [Make Capabilities Legible and Operable] 详细阐述了这两个杠杆。

[Route Context Just in Time]: ../just-in-time-context/
[Make Capabilities Legible and Operable]: ../tool-legibility/

## 在采纳周期中工作

保持 worker 不变使失败变得可操作。一个有能力但无法完成工作的 worker 可能缺少相关的机构上下文、可操作的能力、有用的反馈循环、足够的权限或结果边界处的证据。这些是团队可以检查和改进的部署环境属性。

记录每个周期的 worker 配置：模型、编码 agent 宿主和版本、原生工具以及运行时能力（如压缩、后台进程和计算机使用）。用代表性的完整工作旅程来验证它。反复运行很重要，因为固定的 worker 仍然是随机的；边界标识正在评估的条件，而反复试验则解释随机变异。

[OpenAI harness-engineering essay] 中描述的内部产品在五个月内从一个空仓库增长到大约一百万行代码；[Latent Space interview] 报告了约 1,500 个合并的 pull request。Codex 编写了其所有代码；人类没有手动贡献代码。工程师们改进了仓库、浏览器、可观测性、评审和交付界面，直到 Codex 能够启动应用程序、观察其行为、响应评审并交付经过验证的更改。该约束使缺失的环境能力变得具体。

[OpenAI harness-engineering essay]:
  https://openai.com/index/harness-engineering/#:~:text=Throughout%20the%20development%20process%2C%20humans%20never%20directly%20contributed%20any%20code
[Latent Space interview]:
  https://www.latent.space/p/harness-eng#:~:text=You%20kicked%20off%20as%20a%20team%20of%20three%20people.%20You%E2%80%99re%20putting%20out%20a%20million%20line%20of%20code%2C%20like%201500%20prs

## 每次 worker 升级都重新验证

人类先验是采纳状态的一部分。早期的 worker 教会团队任务应该多小、需要多少编排、内循环可以持续多久，以及人类必须在哪里监控。将这些习惯带入新周期可能会使 [capability overhang] 未被利用。假设每个点版本都在每个维度上改进会产生相反的错误。

[capability overhang]: https://x.com/_lopopolo/status/2031810853801247190

重新运行完整工作并[定期重置这些先验]。将 prompt、skill、文档、工具和证明调整为适应新 worker 的指令遵循、推理、压缩、工具可靠性和故障模式。Ryan Lopopolo 将此描述为针对模型系列中每个版本 [manicuring prompts, skills, and docs]。

[reset those priors regularly]:
  https://x.com/_lopopolo/status/2062192014561603850
[manicuring prompts, skills, and docs]:
  https://x.com/_lopopolo/status/2055157677496443281

[AI Native DevCon talk] 将点版本视为部署变更：可完成的工作可能会发生足够大的变化，以至于需要重新考虑周围的运营环境。[GPT-5.2 shipped while Ryan Lopopolo was on winter break]。当他回来时，团队每位工程师每天额外产出一到两个 pull request，而无需额外的 harness 投资。后来，一个不同的 Symphony harness 干预报告了每位工程师每周 pull request 十倍的收益。验证循环决定环境应该保持稳定、适应还是简化。

[AI Native DevCon talk]:
  https://tessl.io/registry/ainativedev/aidevcon-2026-ldn/0.100.8/files/talk-lopopolo-harness-engineering/transcript.md#:~:text=retooling%20your%20stack%20in%20the%20way%20you%20work%20with%20every%20point%20release
[GPT-5.2 shipped while Ryan Lopopolo was on winter break]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=when%20I%20came%20back%20without%20any%20additional%20investment

在同一周期中重新校准雄心。逐步探测更大的结果，直到证明开始失败。一个 [proposed high-ambition prompt] 要求 worker 搜索开源生态系统中可以改进 `libghostty` 的更改。该提案探测 worker 是否能够发现并完成跨仓库工作。在另一个操作者观察中，Ryan 报告 GPT-5.3 [less willing than GPT-5.2 to attempt difficult work]。评估应该发现新的结果边界，而不是通过习惯保留旧的边界。

[proposed high-ambition prompt]:
  https://x.com/_lopopolo/status/2075861840085848202
[less willing than GPT-5.2 to attempt difficult work]:
  https://x.com/_lopopolo/status/2030441273900097933

## 将内循环延迟视为验证的一部分

Worker 从其在轨迹失去焦点或耐心之前能获得的反馈中进行推理。因此，构建、测试、浏览器和可观测性延迟塑造了可用能力。GPT-5.2 Codex harness 缺少后台 shell，因此 worker 会等待阻塞性构建脚本。GPT-5.3 添加了后台 shell，且不太愿意在同一循环中阻塞。在一周内，团队将仓库从定制 Makefile 迁移到 Bazel、Turbo 和 Nx。当完整构建在一分钟内完成时，他们停了下来。团队采用 [sub-minute build loop] 作为回合上限。突破上限意味着停止当前工作并分解构建图，直到循环再次符合上限。

[sub-minute build loop]:
  https://www.latent.space/p/harness-eng#:~:text=retool%20the%20entire%20build%20system%20to%20complete%20in%20under%20a%20minute

为每个周期重新测试该阈值。快速聚焦的检查支持探索；更广泛的证明可以稍后运行，而不会阻塞 worker 的下一个有用操作。同时衡量接受的结果和速度，这样更短的循环不会仅仅更快地产出不正确的工作。

## 退役 worker 已吸收的脚手架

优先选择通过 worker 原生循环保持可见的干预：架构、类型、测试、工具契约、可观测状态和验收标准。定制的中介在一个周期内可能有价值，但在下一个周期内可能过时。在 [native computer-use example] 中，原生计算机使用使团队能够丢弃虚拟显示和视频捕获堆栈，同时保留端到端的应用验证。

[native computer-use example]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=get%20to%20throw%20away%20all%20that%20bespoke%20code

重新验证包括减法。移除不再改进完整旅程的脚手架，保留仍然表达工作的持久约束，并让新原生能力简化环境。编码 agent 的操作语言是 worker 配置的一部分；[Model-Native Semantics Across Agent Hosts] 详细阐述了采纳周期之间的迁移界面。

[Model-Native Semantics Across Agent Hosts]: model-native-semantics.md
