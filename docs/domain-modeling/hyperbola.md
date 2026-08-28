# Harness Engineering hyperbo.la 构建

添加 RSS feed 变成了围绕一个工具链、一个 workspace 图、显式的包所有权和可执行仓库 contract 重构 Ryan 的静态博客。每个机制为一个领域概念提供一个显式的 owner。

## 策略拥有一个包

`hld/` 下的仓库本地工具强制执行源结构和依赖方向。自定义 ESLint 规则要求有理由的抑制并约束文件系统导入。结构检查禁止旧版文件形式，将内容保留在规范位置，并将重复的依赖提升到 workspace 目录中。

诊断信息是 agent 接口的一部分。它们说明哪个不变量被违反并引导修复。仅仅拒绝输出的检查将保留人类解释架构的需要。

## 领域类型替代宽泛原语

`HyperUrl` 表示不可变的 URL 意图，而不是在包之间传递可变的 `URL` 对象或原始字符串。frontmatter 包接受不确定的 Markdown/YAML 边界，应用博客的受限 schema，并返回带有路径感知错误的类型化值。

这些类型减少了其他地方的重复验证。应用代码不需要重新发现哪些 URL 形状或 frontmatter 键是合法的。

## 包拓扑承载意图

博客领域拥有 manifest、帖子、路由访问器、feed 渲染和类型化模板上下文。应用 shell 拥有共享的组合和资产，而不导入博客规则。一个 Vite 集成包将领域数据连接到开发和构建渲染。

内容及其资产是共置的。开发和预渲染使用相同的 Vite 原生 SSR 路径，因此仓库不维护一个用于本地工作的实现和另一个用于生产的实现。

## 文档是架构 contract

前端架构文档命名了北极星、包职责、禁止的变通方法，以及变更预期的证据。它将 agent 引导到架构修复。文档是持久上下文；结构测试和测试使其稳定的部分可执行。

领域所有权、路径结构、可操作的诊断和单一执行路径使系统更容易延续。每个控制的承载成本仍然必须匹配仓库的风险。

来源：Ryan Lopopolo，["Harness Engineering the Blog Build (Again)"]。快照：
[`sources/raw/hyperbola/harness-engineering-the-blog-build.mdx`]。

["Harness Engineering the Blog Build (Again)"]:
  https://hyperbo.la/w/harness-engineering-the-blog-build/
[`sources/raw/hyperbola/harness-engineering-the-blog-build.mdx`]:
  ../../sources/raw/hyperbola/harness-engineering-the-blog-build.mdx
