# Harness Engineering

> "大多数人并不知道，他们只需将自己的 agent 指向我的文章、推文、播客和演讲，
> 就能将 agent 的输出提升 100 倍。"
>
> — [Ryan Lopopolo]

[Ryan Lopopolo]: https://x.com/_lopopolo/status/2050698864542482709

Harness engineering 是通过塑造 agent 周围的环境来提升其输出质量的实践，它将所选模型和 coding agent 视为黑箱保持不变。它改善了两个外部杠杆——context 和 tools——并精心策划其周围的环境。Worker 应当能够恢复意图、操作真实系统、尊重权限、验证结果，并为下一次运行做好更充分的准备。

该环境的核心目的是承载组织的 nonfunctional requirements：即支配可靠性、安全性、兼容性、可维护性、性能、可操作性、风险态势和精良程度的质量属性和约束。Harness 还承载着关于如何优先排序、权衡和满足这些需求的本地决策。Ryan 采用了 2026 年 [\[un\]prompted conference] 的 [systems-level framing]，将其描述为将整个 nonfunctional requirements 的天地纳入代码。[Make the Repository Teach the Agent] 阐述了这些需求和决策如何成为可检索的 context、examples、tools 和可执行约束。

[systems-level framing]: https://x.com/_lopopolo/status/2028982729145237775
[\[un\]prompted conference]: https://www.youtube.com/watch?v=U2O14Jd3MBU
[Make the Repository Teach the Agent]:
  docs/domain-modeling/#make-nonfunctional-requirements-recoverable

由于 [work is an iterative game]，harness 可以使组织的判断力不断积累。来自已采纳的工作、修正、失败和用户反馈的经验教训，会转化为 context、boundaries、tools、examples 和 checks，塑造后续的轨迹。随着时间的推移，该反馈循环可以 [make coherence cumulative]，贯穿 agent 维护的 artifacts。

[work is an iterative game]: https://x.com/_lopopolo/status/2052858891835465813
[make coherence cumulative]: docs/durable-systems/#make-coherence-cumulative

[Code is how an agent uses a computer]。当 [last-mile deployment] 提供了组织的 context、capabilities、authority 和 proof 时，这种内部动作语言可以为那些从未审查过实现的人们产出可靠的领域结果。

[Code is how an agent uses a computer]:
  https://x.com/_lopopolo/status/2043495733375230026
[last-mile deployment]: docs/last-mile-deployment/

通用模型权重仅包含组织 process-data 冰山的可见尖端。水面之下是 worker 完成特定工作所需的当前运行状态、本地本体论、质量标准、流程、异常历史和权限关系。组织不能假定这些私有的、不断变化的 process data 会存在于通用模型权重中，也不能假定 agent 会可靠地推断出哪些 process data 对组织重要。Harness engineering 是将其作为 context 和 tools 提供给 capable worker 的最后一公里工作。

将 coding agent 指向此仓库及其应改进的系统。[`AGENTS.md`] 将任务路由到相关的论据、案例和证明。如需直接阅读，从 [thesis index] 开始。如需应用，从 [playbooks] 中选择。

[`AGENTS.md`]: AGENTS.md
[thesis index]: docs/
[playbooks]: playbooks/

## 来源与相关工作

- ["Harness engineering: leveraging Codex in an agent-first world"]（供被 canonical page 阻塞的 agent 使用的 [fetch helper]）
- [Source library]
- [Influences and alternate framings]

["Harness engineering: leveraging Codex in an agent-first world"]:
  https://openai.com/index/harness-engineering/
[fetch helper]: sources/scripts/fetch_openai.py
[Source library]: sources/
[Influences and alternate framings]: docs/lineage/

仓库创作的材料按 [CC BY 4.0] 授权。有关 source materials 的署名和权利，请参阅 [`COPYING.md`]。

[CC BY 4.0]: LICENSE
[`COPYING.md`]: COPYING.md
