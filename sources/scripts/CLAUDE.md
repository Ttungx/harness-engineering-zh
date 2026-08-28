@AGENTS.md

# Claude Code

- 以导入的指南作为权威的 Python 契约；不要从附近的 raw-source 文件推断出第二种风格。
- 优先用一个封闭的 parser 和更强的 domain object 替代防御性分支。在 JSON 边界保留有用的错误位置。
- 保持变更集中在 model、decoder、test 和可执行 adapter 中，然后运行导入指南中的完整验证循环。
- 将 `uv` lockfile 视为生成的产物。通过 `uv` 刷新它们；绝不手动编辑。
