# 影响与替代框架

贯穿始终的主线是对使系统可读、在边界保留知识、暴露有用的反馈以及增量改善工作环境的兴趣。Ryan Lopopolo 的应用 AI 框架将模型和编码 agent 保持不变，然后使用 context 和工具将该黑箱连接到私有来源、运营历史、产品判断、当前状态、访问边界和隐含流程。组织不能假设这些私有的、变化的材料会存在于通用模型权重中，也不能假设 agent 会可靠地推断出哪些部分管辖它们的工作。

## 简短、稳定的代码地图

[matklad's `ARCHITECTURE.md` essay] 主张使用紧凑的仓库物理地图而非全面的手册。其代码地图应回答"做 X 的东西在哪里？"同时命名稳定的模块、边界、依赖方向、不变量和横切关注点。

[matklad's `ARCHITECTURE.md` essay]:
  https://matklad.github.io/2021/02/06/ARCHITECTURE.md.html#:~:text=Describe%20coarse-grained%20modules%20and%20how%20they%20relate%20to%20each%20other

在仓库 harness 中，该代码地图可以充当 context 路由层。根指南可以保持简短，因为架构和领域文档为 agent 提供心理地图，然后仅在任务需要时才将其路由到细节。稳定性约束与地图同样重要。架构指导应描述持久的所有权和关系。规范清单应拥有版本固定和瞬态实现事实。Ryan 的 [homelab canonical-manifest enforcement] 使每个消费者将规范文件解析为领域值，而仓库测试和 lint 拒绝并行的字面量和跨文件矛盾。解析和拒绝使清单成为运行中的事实来源。

[homelab canonical-manifest enforcement]:
  ../domain-modeling/homelab.md#model-facts-and-parse-canonical-files

## 能力和迁移接缝

Artichoke Ruby 是用 Rust 编写的 Ruby 编程语言的模块化实现。其命令行前端运行在解释器后端之上，该后端当时嵌入了 mruby——一个用 C 编写的小型 Ruby 实现，提供了虚拟机和解析器。Rust trait 独立于 mruby 命名解释器能力。后端实现了这些 trait 并将调用本地化到 mruby 的 C API，而独立拥有的 Rust crate 实现了 Ruby 数据结构和标准库 API。前端调用者可以依赖稳定的能力，而后端实现在变化。

2021-02-07，Ryan 在 [artichoke/artichoke@fbb43b6] 中添加了 Artichoke 的第一个架构文档和图表。该文档从前端映射到后端、核心解释器 trait、数据结构 crate、工具和发布表面。其提交消息明确引用了 matklad 的文章。

[artichoke/artichoke@fbb43b6]:
  https://github.com/artichoke/artichoke/commit/fbb43b6ce18556412a489a88e5cc24dea9b587a8

能力边界提供了增量迁移的提议接缝。替换实现可以在不要求稳定的调用者直接调用 mruby 的情况下实现一个 trait 支持的行为。2021-02-08，[artichoke/artichoke@1dd8978] 使 Strangler Fig 策略变得明确：mruby 函数将被禁用并逐步替换，同时兼容的边界保持其余部分工作。2022-08-24，[artichoke/artichoke@fa5b2eb] 移除了一个字面的 mruby 版本，以便架构文档在依赖更新中保持真实。这些提交共同记录了一个预期的迁移接缝：Artichoke 在兼容行为可以被增量替换的同时继续使用 mruby，而架构描述在后端版本变化中保持稳定。

[artichoke/artichoke@1dd8978]:
  https://github.com/artichoke/artichoke/commit/1dd8978fe4edf1971928fd00e30edbd124fd0aaa
[artichoke/artichoke@fa5b2eb]:
  https://github.com/artichoke/artichoke/commit/fa5b2eb907d8746efd7cf51f07cd84e7e1f9b49f

该序列说明了三个与 harness 相关的习惯：

- 架构映射持久的所有权和关系；
- 行为组织在能力形状的边界后面；以及
- 大型迁移通过保留工作系统的显式接缝进行。

## 在边界解析

Alexis King 的 ["Parse, don't validate"] 提供了类型化的边界规范。解析器将不太结构化的输入转换为更精确的表示，保留学到的内容。一旦边界产生了有效的领域值，下游代码就不需要重复临时检查或保留不可能的状态。

["Parse, don't validate"]:
  https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/#:~:text=consumes%20less-structured%20input%20and%20produces%20more-structured%20output

在仓库 harness 中，清单、CLI 参数、工作流文件、远程目标和生成的配置是外部语法，应该被一次性解析为语义值。然后策略对这些值提出领域问题：存在哪个能力、哪个版本是规范的、目标是本地还是远程、授权哪个操作，或者两个清单是否一致。这防止版本策略退化为重复的常量和序列化的固定文本。

类型化的边界解析也改善了反馈。一个理解领域的传感器可以报告违反的关系和预期的修复。字符串比较通常只能报告字节不同。

## 围绕判断关闭反馈循环

Ryan 2026-02-11 的 [seminal harness-engineering essay] 描述了该实践，并将跨年的架构一致性作为一个开放问题。Birgitta Böckeler 2026-02-17 的 [initial memo] 通过 context、确定性约束、LLM 审查和反复反馈进行了回应。

[seminal harness-engineering essay]:
  https://openai.com/index/harness-engineering/#:~:text=What%20we%20don%E2%80%99t%20yet%20know%20is%20how%20architectural%20coherence%20evolves%20over%20years
[initial memo]:
  https://martinfowler.com/articles/exploring-gen-ai/harness-engineering-memo.html#:~:text=mix%20deterministic%20and%20LLM-based%20approaches%20across%203%20categories

George Zhang 2026-03-07 的 ["Harness Engineering Is Cybernetics"] 将 OpenAI 实验解读为反馈循环可以关闭位置的改变。编译器、测试和 linter 已经检测到机械可观察的偏差。一个有能力的编码 agent 也可以检查和修复架构和设计。仓库特定的 context 和控制校准该传感器-执行器循环与系统期望状态之间的偏差。

["Harness Engineering Is Cybernetics"]:
  https://x.com/i/article/2030414577213820928

Böckeler 2026-04-02 的文章阐述了互补的操作词汇。[Guides provide feedforward context] 在 agent 行动之前；[sensors observe results] 并在之后实现自我纠正。两者都可以是计算的，如类型检查器或结构测试，也可以是推断的，如专注的审查 agent。该文章将 harness 描述为 [cybernetic governor]，并使用 Ashby 的必要多样性法则来解释为什么 [constrained service topologies] 可能更容易调节。该法则说，调节器需要足够的可能响应来匹配被管辖系统可以进入的状态多样性。标准化更少的服务拓扑减少该状态空间，允许有限的指南和传感器集合覆盖更多可能发生的行为。

[Guides provide feedforward context]:
  https://martinfowler.com/articles/harness-engineering.html#:~:text=Guides%20(feedforward%20controls)%20-%20anticipate%20the%20agent%27s%20behaviour
[sensors observe results]:
  https://martinfowler.com/articles/harness-engineering.html#:~:text=Sensors%20(feedback%20controls)%20-%20observe%20after%20the%20agent%20acts%20and%20help%20it%20self-correct
[cybernetic governor]:
  https://martinfowler.com/articles/harness-engineering.html#:~:text=The%20agent%20harness%20acts%20like%20a%20cybernetic%20governor
[constrained service topologies]:
  https://martinfowler.com/articles/harness-engineering.html#:~:text=committing%20to%20a%20topology%20narrows%20that%20space%2C%20making%20a%20comprehensive%20harness%20more%20achievable

两者都是对 Ryan 文章的后续解读。Zhang 使高级控制循环及其校准变得明确；Böckeler 按方向、执行类型、生命周期时序和它们调节的质量来区分控制。

该词汇使闭环变得具体。一个架构规则进行指导；一个拒绝违规的 linter 进行感知。一个 runbook 指导一个重要操作；一个金丝雀、访问检查和回滚验证感知它是否保持安全。预期的约束塑造尝试，观察到的结果纠正它，反复的失败改善下一个指南或传感器。

为 harness 干预保持不变的 worker 是选定的模型加上运行其工具循环的编码 agent。模型原生语义包括模型看到的工具名称和参数、返回给它的结果和错误、审批行为和会话生命周期。harness 干预更改仓库 context、工具、访问、验证器和周围环境，同时保持该 worker 配置稳定足够长的时间以归因结果。

[Agent Client Protocol] (ACP) 解决了客户端-agent 边界。它是一个 JSON-RPC 协议，编辑器可以通过它连接到编码 agent、接收会话和工具调用更新，并在 agent 保留对其模型和内部工具循环的责任的同时回应权限请求。ACP 可以将 worker 承载通过不同的客户端表面；适配器仍然必须保留对该 worker 重要的任何模型原生语义。

[Agent Client Protocol]:
  https://agentclientprotocol.com/get-started/architecture#:~:text=ACP%20makes%20heavy%20use%20of%20JSON%2DRPC%20notifications

## 增量采用

Martin Fowler 的 ["Strangler Fig"] 描述了"渐进的现代化过程"：引入接缝，以可管理的块移动行为，并在新旧系统共存时持续交付。

["Strangler Fig"]:
  https://martinfowler.com/bliki/StranglerFigApplication.html#:~:text=The%20alternative%20that%20my%20colleagues%20and%20I%20prefer%2C%20is%20to%20do%20a%20gradual%20process%20of%20modernization

harness engineering 可以遵循相同的采用模式。团队可以从最昂贵的人工中继或最重复的失败开始：添加代码地图，暴露一个领域命令，解析一个规范清单，编码一个审查规则，使一个运行时表面可观察，或将一个危险的切换从自主准备中分离出来。每一步都应使仓库比之前更可读，正常路径比之前更受约束。

Ryan 的 [private homelab infrastructure repository] 在运营控制中记录了这个序列。在远程访问升级工作暴露了丢失机器人吸尘器唯一修复路径的方法后，其 runbook 获得了一个隔离的金丝雀：第二个 Tailscale 身份和独立状态证明替换守护进程在替换生产环境之前启动、加入网络并允许 SSH。一个单独的仅报告文档新鲜度角色将运维页面与定义描述行为的配置和监控源进行比较。它指出矛盾的证据并提出编辑，然后等待人类授权再改变仓库。

[private homelab infrastructure repository]: ../domain-modeling/homelab.md

Artichoke 的能力接缝和 homelab 的 runbook 产生于具体的迁移和操作。审查和事件教训然后转移到类型、文档、测试、lint 和状态机中。每次添加都保留了一个工作系统，同时使下一个更改更容易路由和验证。

## 在 Ryan 的工作中演进

实施策略随着 worker 能力和部署表面的改善而改变。底层问题——将通用模型定位于本地工作中——保持稳定。

### 人工配对变为 whole-job 自主性

在 ["I Wrote 4,000 Lines of Code with ChatGPT in a Weekend"] 中，Ryan 在编辑器和 ChatGPT 之间复制代码和测试输出，并亲自中继每个操作。到 2026 年，[RustSec case] 和 [robot-vacuum case] 给予 agent 直接访问以重现行为、实施更改、运行测试、准备交付和组装证明。[artichoke/intaglio#360 review record] 描述了实质性的实施审查：审查者请求一个专用模块、显式的状态转换、一个消耗性终端方法、更清晰的名称和专注的测试，Codex 修订了补丁。Ryan 的 [artichoke/intaglio#360 human approval] 然后授权了合并、版本发布和 RustSec 报告。该演进减少了人工中继，同时保留了实施判断和发布权限。

["I Wrote 4,000 Lines of Code with ChatGPT in a Weekend"]:
  https://hyperbo.la/w/chatgpt-4000/#:~:text=I%20interacted%20with%20ChatGPT%20by%20copying%20code%20in%20vim
[RustSec case]: ../proof/rustsec.md
[robot-vacuum case]: ../domain-modeling/homelab.md
[artichoke/intaglio#360 review record]:
  https://github.com/artichoke/intaglio/pull/360#issuecomment-4158222124
[artichoke/intaglio#360 human approval]:
  https://github.com/artichoke/intaglio/pull/360#issuecomment-4158273357

### 提议的专家变为固定的 worker 加检索

在 2023 年的文章 "I Wrote 4,000 Lines of Code with ChatGPT in a Weekend" 中，Ryan [proposed off-the-shelf specialists]：一个在 Rust 文档和发布材料上训练的，一个用于每个依赖 crate 的，一个用于 Ruby Core 和 Standard Library API 的，一个用于 Ruby Spec 的，一个用于本地代码库及其历史的。后来的 harness 实践选择一个有能力的通用模型-agent worker，并按需提供相关的代码、历史、流程数据和工具。有位置感的专业知识的目标仍然存在。2023 年的提议将该专业知识放在单独训练的 agent 中；后来的路线围绕固定的 worker 策划环境。

[proposed off-the-shelf specialists]:
  https://hyperbo.la/w/chatgpt-4000/#:~:text=Dreaming%20big%2C%20I%E2%80%99d%20love%20to%20see%20a%20workflow

### 工具发现获得渐进式披露

2025 年的文章 ["MCP Solves Tool Discovery for LLMs"] 认为 Model Context Protocol (MCP) 通过将工具名称、描述、输入 schema 和示例放入模型 context 中使能力可见。[Ryan's 2026 note on progressive disclosure] 重新审视了该机制的 context 成本：加载整个工具目录类似于加载每个手册页面，而熟悉的命令行工具可以通过 `--help` 仅在 worker 请求时渐进地暴露细节。两种机制解决相同的需求：worker 必须接收足够的信息来发现操作并正确调用它，同时不消耗每个不可用或无关操作的注意力。

["MCP Solves Tool Discovery for LLMs"]:
  https://hyperbo.la/w/tool-discovery/#:~:text=names%2C%20descriptions%2C%20input%20schemas%2C%20and%20example%20calls
[Ryan's 2026 note on progressive disclosure]:
  https://x.com/_lopopolo/status/2026798214297571352

### "Code is free" 获得所有权和相称性

2026 年初的写作，包括 [hyperbo.la build]，使用廉价的实施来证明穷举迁移、本地 linter 和 100% 覆盖率的合理性。后来的工作在丰裕改变的周围划出了更坚定的边界。[Software Work Is No Longer Scheduled] 将具有清晰期望状态、理解的接口和可验证结果的工作分配给异步 agent 执行，同时为零到一产品、困难的接口重构以及接口未知的领域保留持续的人类判断。[Latent Space interview] 同样将困难的深度重构描述为一个开放问题。产品适配、兼容性、安全、证明和维护即使在另一个实施尝试很廉价时仍然昂贵。

[hyperbo.la build]:
  https://hyperbo.la/w/harness-engineering-the-blog-build/#:~:text=codify%20repo%20contracts%20as%20tools%20and%20lints%20instead%20of%20tribal%20memory
[Software Work Is No Longer Scheduled]:
  https://hyperbo.la/w/software-work-not-scheduled/#:~:text=Building%20a%20product%20from%20zero,all%20require%20sustained%20human%20judgment
[Latent Space interview]:
  https://www.latent.space/p/harness-eng#:~:text=where%20you%20don%E2%80%99t%20know%20what%20the%20proper%20shape%20of%20the%20interfaces%20are

运营环境也承载成本。Ryan 的 [ablation framing] 移除 context 或工具并衡量性能是否退化。这使每条指令和控制证明其消耗的注意力和维护是合理的，而不是将积累视为进步。

[ablation framing]: https://x.com/_lopopolo/status/2049145174790725654

### 利用率变为有效性内的诊断

[Agents, Agents, Agents] 将每天每个工程师十亿 token 定义为利用率目标：一个探针，衡量软件生命周期中有多少允许被 agent 观察和操作。这度量访问、可寻址性和 agent 活动。后来的帖子使结果度量变得明确：[token spend is unanchored in business value or return on investment]，以及 [effectiveness is what matters]。利用率可以揭示缺失的工具、不可访问的系统或使可寻址工作闲置的人工中继。有效性询问已完成的工作是否为其成本创造了足够的价值。

[Agents, Agents, Agents]:
  https://hyperbo.la/w/agents-agents-agents/#:~:text=A%20billion%20tokens%20per%20engineer%20per%20day%20is%20a%20utilization%20target
[token spend is unanchored in business value or return on investment]:
  https://x.com/_lopopolo/status/2035102053979271654
[effectiveness is what matters]:
  https://x.com/_lopopolo/status/2035132736621748332
