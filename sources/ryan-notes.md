# Ryan Lopopolo 的实现观察

## Polytoken 和 Codex `apply_patch`

2026年7月17日，Ryan Lopopolo 基于他对 Polytoken 实现的了解提供了以下观察：

> Polytoken directly reuses Codex's open-source `apply_patch` implementation for
> OpenAI models.

Polytoken 的公开文档描述了模型选择的 `patch_edit` 接口，但并未独立确认实现复用关系。
