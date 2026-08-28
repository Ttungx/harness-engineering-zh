# 教学的架构

## Ryan 的 homelab：一个语义 owner

Ryan 的 homelab 将当前工具版本保留在规范 manifest 中。边界解析器将这些文件转换为领域值，策略代码检查持久关系，如精确固定、冷却期和依赖信任类。常规版本更改更新拥有该值的 manifest。agent 可以从该 owner 推断期望状态，而无需协调复制的常量、策略分支和序列化的 fixture 文本。

相同的模式适用于命令和生成的配置：外部语法通过类型化边界进入，下游操作使用受信任的模型。[The homelab case study] 详细阐述了在该所有权模型之上构建的包边界、工具、证明和重要操作。

[The homelab case study]: homelab.md

## Artichoke：capability 作为上下文边界

Artichoke 是一个用 Rust 编写的 Ruby 编程语言的模块化实现。其原始执行后端嵌入了 mruby，一个 Ruby 虚拟机和解析器的 C 实现。Artichoke 的 Rust 前端依赖于以 trait 形式表达的解释器 capability；mruby 后端提供这些 capability，而独立的 crate 拥有 Ruby 类型和标准库组件。

调用者可以从稳定的 capability contract 进行推理，而无需将后端的 C 集成加载到活动上下文中。相同的边界让项目增量替换 mruby 行为：新的 Rust 实现可以一次接管一个 capability，而其余行为继续通过嵌入后端运行。

[Capability and migration seams] 跟随架构的起源和 Strangler Fig 迁移。[The state-model evaluation] 使用后来的重构来测试更困难的问题：当许多独立合并的变更将仓库推向 seam 时，harness 是否能帮助 agent 保留未来的目标。

[Capability and migration seams]: ../lineage/#capability-and-migration-seams
[The state-model evaluation]: ../../evals/artichoke-state-modeling.md

## `rand_mt`：仓库形状作为操作上下文

[`artichoke/rand_mt`] 是一个小型 Rust crate，实现了参考 MT19937 和 MT19937-64 Mersenne Twister 伪随机数生成器。其兼容性表面包括种子的确定性输出、与 Ruby 生成器的可重现性、稳定的 public API、无分配的 `no_std` 操作，以及委托给相同实现的可选 `rand_core` adapter。这些生成器不适用于加密用途。

仓库保持较短的根路由、故意扁平的架构、那些不变量、依赖策略、集中的防护栏和签入的自动化运行手册。Agent 可以发现哪个文档拥有一个决策，并在机械化检查的约束被违反时收到精确的失败信息。文档管理其余的判断。这种组合使仓库的首选延续更容易推断，而无需将每个规则放在初始 prompt 中。[source library] 索引了仓库文档、推出历史和后来的适配。

[`artichoke/rand_mt`]: https://github.com/artichoke/rand_mt
[source library]: ../../sources/#projects-and-cases
