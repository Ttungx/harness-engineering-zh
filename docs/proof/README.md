# 在真实环境中证明结果

验证属于工作的一部分。当证据显示在用户、操作员或依赖系统将依赖的环境中承诺的结果时，更改就完成了。

## 在将要体验的地方定义成功

从命名产品功能和通过它们的旅程开始。这些旅程为实施、审查和发布提供了一个 agent 可以执行和观察的共享目标。

在 [Aakash Gupta interview] 中，一位之前担任产品经理的新工程师注意到仓库没有记录产品的功能。Codex 爬取了代码库并起草了功能清单；团队中的人员根据他们的产品知识进行了审查。经过审查的清单使关键用户旅程变得明确，QA agent 然后可以启动应用、执行这些旅程并断言它们仍然有效。部署需要更少的手动冒烟测试，因为一个队友的观察变成了一个可重复的验收套件。

[Aakash Gupta interview]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=we%20had%20no%20documentation%20in%20the%20codebase%20for%20what%20product%20features%20we%20had

在实施之前定义验收边界：

- 谁或什么体验结果；
- 哪些起始状态和输入重要；
- 哪些可见行为和副作用必须发生；
- 哪些不变量必须保持为真；
- 观察者将如何区分成功和看似合理的模仿；以及
- 哪些在声明之外。

这个规范可以保持小巧。它的工作是识别 worker 必须能够产生的证据。

## 将证据与声明匹配

将每个绿色勾号映射到它实际做出的断言。然后在受影响的边界收集证据。

| 声明                       | 声明边界的证据                                                             |
| -------------------------- | -------------------------------------------------------------------------- |
| 浏览器行为                 | 具有语义和渲染状态的真实浏览器旅程                                         |
| 生成的内容                 | 完整的源到输出比较和新鲜度                                                 |
| 兼容性迁移                 | 目标语料库对等性、边缘案例和显式接受的差异                                 |
| 安全影响                   | 重现器、有界可利用性和回归覆盖                                             |
| 部署                       | 经过验证的工件在部署后健康状态下运行                                       |
| 重要的远程更改             | 暂存金丝雀、切换、恢复和切换后检查                                         |
| 电子表格计算               | 公式、单位、源单元格和渲染的工作簿状态                                     |
| 数据集转换                 | schema、来源和源到输出的协调                                                |
| 文档或演示文稿             | 所需的结构和渲染的页面；承诺时的引用或无障碍性                             |
| 分析结论                   | 可重复的转换和由证据支持的结论                                             |
| 业务状态变更               | 所需的审批、收据和观察到的后条件                                           |

单元测试、类型检查、lint 和构建建立了重要的内部属性。标准兼容性需要一致性证据。语义完整性需要源到输出的比较。成功的上传需要在支持发布声明之前有一个健康的部署。

当保留在 agent 轨迹内部时，证明包使用用户的领域语言。电子表格公式、渲染的幻灯片、引用的发现或完成的状态转换承载声明。对隐藏脚本通过的测试仅建立内部属性。结果在 [the user's domain boundary] 确立。

[the user's domain boundary]:
  ../last-mile-deployment/#serve-users-who-who-never-see-code

## 让 agent 访问真实系统

agent 应该能够启动应用、驱动浏览器或 UI、读取日志、查询指标和跟踪、检查持久副作用、比较生成的语料库，并观察 CI 和部署。人类不应该中继 agent 可以直接检查的证据。

[canonical harness-engineering essay] 描述了每个工作树的应用实例和临时可观测性栈，让 Codex 可以重现 bug、查询日志和指标、驱动 UI 并验证自己的更改。[The Production Function Changed] 将同样的要求带入审查：在请求审查之前附加测试结果、QA 计划、暂存日志、截图或视频来演示结果。

[canonical harness-engineering essay]:
  https://openai.com/index/harness-engineering/#:~:text=we%20made%20the%20app%20bootable%20per%20git%20worktree
[The Production Function Changed]:
  https://hyperbo.la/w/production-function-changed/#:~:text=The%20agent%20should%20attach%20those%20validation%20artifacts%20to%20the%20PR%20before%20asking%20for%20review.

一起使用语义和渲染证据。DOM 快照和结构化状态对角色、值和关系是高效的。截图、视频或计算机使用暴露了语义视图省略的布局、裁剪、焦点、渲染和交互缺陷。日志和跟踪建立了视觉演示无法建立的运行时行为。

Ryan Lopopolo 的 OpenAI 团队在其 Electron 应用内部暴露了这样一个表面。[Electron component workbench] 是一个可启动的开发窗口，包含应用的原生渲染画布和完整的设计系统组件库。Codex 可以从生产组件组合新屏幕，在团队将要发布的表面中渲染它们，并将截图返回给设计师审查。

[Electron component workbench]:
  https://tessl.io/podcast/109/#:~:text=One%20neat%20way%20that%20we%20made%20that%20idea%20real,give%20them%20to%20our%20designer

团队还让 Codex 启动一个 [production-like observability stack] 用于本地指标和日志。其 [local browser shell] 挂载了已连接 Chrome DevTools 的 UI，为 worker 提供盒模型、控制台和其他浏览器原生观察。Ryan 将内置计算机使用描述为他今天会用来检查应用、点击旅程并观察预期副作用是否发生的方法。

[production-like observability stack]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=What%20we%E2%80%99ve%20done%20instead%20is%20permit%20codeex%20to%20spin%20up,diagnose%20issues%20in%20local%20dev
[local browser shell]:
  https://www.aakashg.com/how-pms-ship-100k-lines-of-code/#:~:text=We%20mount%20the%20UI%20into%20a%20local%20browser%20shell,that%20it%20works%20end%20to%20end

另一个互补技术将 [semantic and rasterized views] 配对。语义表示帮助模型识别接口对象及其角色。光栅化接口暴露空间关系，如布局、裁剪和占用空间。两者结合给 worker 命名的对象和它正在操作的渲染几何体。

[semantic and rasterized views]:
  https://www.latent.space/p/harness-eng#:~:text=We%20want%20the%20agent%20to%20be%20able%20to%20see%20the%20UI,object%20it%E2%80%99s%20manipulating

直接访问也让 agent 选择最小的有用检查工具。当面向人类的仪表板只会增加一个中间工件和另一个中继时，agent 可以直接查询跟踪归档。[Latent Space interview] 讲述了一位队友花了一下午时间打磨一个跟踪查看器，然后发现 Codex 可以在大约五分钟内从跟踪 tarball 本身回答问题。

[Latent Space interview]:
  https://www.latent.space/p/harness-eng#:~:text=you%20could%20just%20spin%20up%20codex%20and%20give%20it%20the%20tar%20ball

## 为审查压缩轨迹

审查者需要足够的证据来评估结果而无需重放整个会话。同一次采访将 PR 视频描述为工作的压缩：一个队友呈现做出合并决策所需的证据，而不是要求审查者旁观实施。

一个有用的证明包包含：

- 预期的结果和受影响的边界；
- 重要的设计和风险决策；
- 运行的确切测试和旅程；
- 承载声明的截图、视频、日志、跟踪、diff 或重现器；
- 已知的限制和未证明的行为；以及
- 提议交付的工件身份。

计划可以协调轨迹。因为 [plans are not shipped]，证明包覆盖向用户承诺的行为、副作用和确切工件。

[Plans are not shipped]: https://x.com/_lopopolo/status/2048945121815867857

包应该是简洁的，因为审查者的注意力是稀缺的。更多证据仅在改变对声明的信心时才有帮助。

## 通过交付保留工件身份

发布证明从验证到切换跟随一个不可变的工件。在更特权的作业中重建会中断该链：部署的字节不再是之前证据覆盖的字节。

[Release Integrity] 阐述了完整路径：构建一次，记录来源，提升相同的工件，验证运行的系统，并保留回滚。部署后检查属于证明，因为成功的交付是关于运行边界的运营声明。

[Release Integrity]: release-integrity.md

[homelab canary] 将此原则应用于远程访问升级。成功的构建不能证明升级的机器将保持可达。证明通过一个隔离的金丝雀身份、一个独立验证的访问路径、一个有备份的生产切换以及切换后访问和日志检查来进行。

[homelab canary]: ../domain-modeling/homelab.md

## 给 agent 它可以测试的基准真相

当领域提供它们时，客观评分器使迭代变得廉价。[Miri] 是 Rust 中级中间表示的解释器，可以运行二进制文件或测试套件并报告未定义行为的类别，包括越界访问、释放后使用、未初始化数据的无效使用以及一些别名违规。在 Ryan 的 [Codex-and-Miri work] 中，他使用 Miri 作为可执行的基准真相，同时 Codex 在流行的 Rust crate 中搜索不健全的执行。干净的 Miri 运行仅限于它探索的执行；报告的违规给 worker 一个具体的失败来重现和修复。

[Miri]: https://github.com/rust-lang/miri
[Codex-and-Miri work]: https://x.com/_lopopolo/status/2047102222530728035

[Accessibility standards and assistive technology] 提供具体的质量检查。在一个 [CTF-style UI exercise] 中——"CTF" 意为"夺旗"——团队在真实应用工作流后面放置标记。一个 skill 通过驱动接口并捕获标记来成功，给重复的改进运行一个可执行的胜利条件。兼容性向量、金丝雀健康和完整语料库比较同样可以将模糊的目标转变为可观察的结果。

[Accessibility standards and assistive technology]:
  https://x.com/_lopopolo/status/2054987836731150606
[CTF-style UI exercise]: https://x.com/_lopopolo/status/2047052450604212728

困难的部分是构建一个奖励真实结果并抵抗博弈的验证器。Ryan 将此描述为需要一个 [unhackable grader]。从业者也带着 [hard cases from their own work]。将这些案例编码为可重复的本地试验，用自己的工件和风险补充广泛的公共基准。

[unhackable grader]: https://x.com/_lopopolo/status/2056991142093508831
[hard cases from their own work]:
  https://x.com/_lopopolo/status/2059378830939402614

非功能性需求在它们管辖的边界处成为证明义务。代表性证据可以包括与相关语料库、版本矩阵或规范的兼容性；针对命名工作负载和阈值的性能；无障碍性一致性检查加上辅助技术或用户旅程；以及针对威胁模型评估的安全控制。定性需求如基调、品味、风险容忍度、打磨和可接受的捷径在团队命名其判断并给审查者一个收敛策略时变得更加可靠。[What Does It Mean to Do a Good Job?] 阐述了验证的这一部分。

[What Does It Mean to Do a Good Job?]:
  https://hyperbo.la/w/what-does-it-mean-to-do-a-good-job/#:~:text=tone%2C%20taste%2C%20risk%20tolerance%2C%20how%20much%20polish%20is%20enough%2C%20what%20shortcuts%20are%20acceptable

[Evaluate the Harness] 将结果、证明、架构和轨迹成本分离，使方便的代理不会静默地替换正在衡量的工作。

[Evaluate the Harness]: ../../evals/

## 说明证据未确立的内容

安全证据应说明重现器工作的条件和支持的结论。在 [Prove a Security Claim] 中，损坏需要自定义 hasher 和 panic 后的恢复；重现器未在默认路径上建立内存不安全。Codex 开发了报告、重现器、补丁、测试和对实质性实施审查的回应。Ryan 审查了实施、批准了它，并分别授权了合并、版本发布和 RustSec 报告。操作指令是 [prove impact or exploitability] 每个报告的问题；推测性的安全形状输出不满足工作。

[Prove a Security Claim]: rustsec.md
[prove impact or exploitability]:
  https://x.com/_lopopolo/status/2038739679542215136

证明负担随后果缩放。一个错别字、一个解析器迁移、一个远程升级和一个生产发布需要不同的证据和权限。[Repository Review Playbook] 跟踪这些声明通过代表性的仓库旅程，并记录可用证据未达到的任何边界。

[Repository Review Playbook]: ../../playbooks/repository-review.md
