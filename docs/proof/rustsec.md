# 证明安全声明

[`intaglio`] 是一个 Rust 符号驻留器：它存储每个不同的字符串一次，并返回一个紧凑、稳定的符号，调用者可以比较并稍后解析。它的两个索引表示同一关系的相反方向：一个向量将每个符号映射到其字符串，而一个哈希映射将每个字符串映射回其符号。一个对 crate 进行红队测试的一行请求产生了 RustSec（Rust 生态系统的漏洞数据库）中的一个公告，因为验收标准要求可观察的影响或可利用性结果。

[`intaglio`]: https://github.com/artichoke/intaglio

## 重现状态损坏

Codex 发现了一个 unwind-safety 排序缺陷。驻留器在哈希映射中插入相应的字符串到符号条目之前，将字符串附加到符号到字符串的向量中。自定义 hasher 可能在第二次变更期间 panic。如果调用者使用 `catch_unwind` 恢复并继续使用该表，向量将包含新字符串而映射不会。后续插入可能重用缺失的符号编号并返回一个向量查找解析到先前字符串的符号。

[public issue] 记录了此符号混乱的发布模式重现器。它还限制了声明：当表使用 `RandomState`（Rust `HashMap` 的普通默认 hasher 构建器）时，证据未建立内存不安全。演示的失败需要一个自定义的可 panic hasher、插入期间的 panic 以及恢复后的继续使用。

[public issue]: https://github.com/artichoke/intaglio/issues/359

## 将发现带过审查和发布

Codex 开发了报告和重现器，实现了回滚保护，为所有五个符号表变体添加了回归测试，并打开了 [fix pull request]。然后人类审查实质性地改变了实施。Ryan 请求一个专用模块、一个类型化的状态转换而非布尔值、一个消耗性终端操作、更清晰的名称、断言和专注的单元测试；Codex 修订了补丁并重新运行了 crate 的检查。

[fix pull request]: https://github.com/artichoke/intaglio/pull/360

Ryan 批准了实施并明确授权 Codex 准备版本发布、合并 pull request 并打开 RustSec 报告。仓库在 Ryan 的维护者身份下记录了合并和 [v1.13.3 release]，产生的 [RUSTSEC-2026-0078] 公告发布了有界的影响。这个分工很重要：Codex 执行了调查和委托的发布工作流；人类维护者提供了实施判断以及合并、发布和披露的权限。

[v1.13.3 release]: https://github.com/artichoke/intaglio/releases/tag/v1.13.3
[RUSTSEC-2026-0078]: https://rustsec.org/advisories/RUSTSEC-2026-0078.html

相同的 prompt 在 Ryan 尝试的每个活跃 Artichoke crate 中至少产生了一个有用的发现。代表性结果包括一个未安装依赖的 Prettier CI 作业、过度强调未种子构造函数的 `rand_mt` 文档，以及缺乏 `Send` 和 `Sync` 自动 trait 边界和生命周期扩展的编译失败检查的 `raw-parts` 覆盖率。这些是 CI 正确性、文档和测试覆盖率发现。它们仍然是维护工作，因为它们没有为安全影响或可利用性提供重现器。harness 保留了重现的安全影响和有用的维护发现之间的区别。

来源：Ryan Lopopolo，["A Lazy Prompt Turned Into a RustSec Advisory"]。
快照：[`sources/raw/hyperbola/lazy-prompt-rustsec.mdx`]。

["A Lazy Prompt Turned Into a RustSec Advisory"]:
  https://hyperbo.la/w/lazy-prompt-rustsec/
[`sources/raw/hyperbola/lazy-prompt-rustsec.mdx`]:
  ../../sources/raw/hyperbola/lazy-prompt-rustsec.mdx
