# Source 工具

在仓库根目录下运行以下命令：

- `uv run --script sources/scripts/validate_manifest.py` validates source
  relationships, snapshot custody, hashes, and Twitter-corpus reconciliation.
- `uv run --script sources/scripts/test_manifest.py` runs the source-model and
  repository-contract tests.
- `uv run --locked --script sources/scripts/fetch_openai.py` retrieves readable
  text for the canonical OpenAI harness-engineering essay, falling back to the
  archived URL recorded in the manifest.

`source_manifest.py` and `twitter_corpus.py` own the typed domain models and
JSON boundaries used by the commands. They are library modules, not root-invoked
workflows.
