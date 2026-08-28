# Python Source 工具

这些脚本解析和验证 source manifest 和 Twitter corpus，并获取 OpenAI harness-engineering 文章。保持它们的小巧、类型化，并可通过 `uv` 独立运行。

## 在编写工作流之前先建模领域

- Parse JSON and other unknown input at the boundary. Business logic receives
  domain objects, never partially checked mappings.
- Represent domain state with standard-library dataclasses declared
  `@dataclass(frozen=True, slots=True, kw_only=True)`.
- Establish semantic invariants in `__post_init__`. Boundary decoders establish
  transport shape and primitive types, construct the domain object, and attach
  the input location to construction errors.
- Use closed tagged variants when records have different required fields. Avoid
  one dataclass with a collection of booleans and nullable fields that describe
  several implicit states.
- Prefer tuples, immutable values, and derived properties. Keep one owner for
  each fact instead of synchronizing parallel representations.
- Raw `dict[str, object]` values belong only at an input boundary or in JSON
  test fixtures. Do not return them as domain results or pass them through the
  workflow.
- Once construction proves an invariant, downstream code should rely on the
  type. Do not repeat shotgun validation in every caller.
- Build structured output from models and serializers. Repeated string joining
  and interpolation of structured records indicates a missing domain model.

## 保持单位完整到边界

- Represent durations as `datetime.timedelta`. Convert with `total_seconds()`
  only when calling an API that requires a numeric value.
- Construct an explicit `httpx.Timeout` at the HTTP client boundary. Keep
  connect and overall request policy visible as named durations.
- Accept clients or other consequential collaborators as arguments where that
  makes behavior testable. Keep network access out of model constructors.
- Retry only failures that can plausibly succeed unchanged. Keep retry budgets
  bounded and honor an actionable `Retry-After` response.

## 保持脚本边界

- Keep executable workflows as PEP 723 scripts with `uv` shebangs.
- Prefer the standard library. When a script needs an outside package, declare
  and pin it in that script's inline metadata and commit its `uv` lockfile.
- Keep shared models in `source_manifest.py` or `twitter_corpus.py`; keep the
  executable modules as thin orchestration layers.
- Do not edit immutable source snapshots while changing tooling.

## 测试与验证

Keep parser and model tests hermetic. Cover successful construction, missing and
unknown fields, tagged-variant boundaries, cross-record relationships, overflow
and path safety, and failures that previously escaped the model.

从仓库根目录运行聚焦的验证循环：

```sh
uvx ty check sources/scripts
uvx ruff check sources/scripts
uvx ruff format --check sources/scripts
uv run --script sources/scripts/test_manifest.py
uv run --script sources/scripts/validate_manifest.py
```

Use `uvx ruff format sources/scripts` to apply formatting. Run the locked fetch
as an integration check when changing retrieval behavior:

```sh
uv run --locked --script sources/scripts/fetch_openai.py
```
