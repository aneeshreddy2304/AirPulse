# Test suite

The existing suite contains 16 inference tests, 14 training-logic tests, and 22 API tests. It covers selected behavior; it does not cover every endpoint, Lambda workflow, cloud resource, deployment path or model-quality claim.

## Run focused checks

```bash
uv sync --frozen --group dev
uv run pytest tests/test_inference.py tests/test_training_logic.py -q
uv run ruff check app/ ml/ tests/
```

Run in an isolated development environment. The session fixture can create ignored synthetic model artifacts under `ml/model-registry/`. Never run it against production artifact files as an operational validation step.

## What is tested

| File | Scope |
|---|---|
| `test_inference.py` | Feature-vector shape/dtype/missing values, probability schema and batch alignment |
| `test_training_logic.py` | Synthetic per-city feature engineering and array preparation |
| `test_api.py` | Health/cities, selected prediction/history behavior, and model-metrics schemas with Athena mocked |

The inference feature vector has shape `(1, 19)`. Batch comparisons require five or more rows and produce `len(rows) - 4` entries: three lookback rows and one subsequent ground-truth row are consumed. Training feature preparation likewise removes four boundary rows per synthetic city.

## API lifecycle caveat

The API fixture mocks Athena, but application startup also starts a background cache warmer and optional configured telemetry/Redis integrations. The suite should not be described as unconditionally hermetic. Keep cloud/telemetry configuration unset and isolate side effects when running the full API suite.

In the portfolio audit at source commit `31db47e`, the background `_warm_loop` was patched to a no-op and MLflow tracking redirected to a temporary local directory. The full suite passed **52 tests** with synthetic model artifacts and mocked Athena. That audit used core versions aligned with `uv.lock`, including `mlflow-skinny` in a reduced environment, rather than a complete frozen dependency installation.

## Not established by these checks

Cloud integration correctness, buffer durability, every API route, real-model quality, infrastructure availability, and deployment security require their own evidence. Ruff's documented command covers `app/`, `ml/`, and `tests/`, not all pipeline/Lambda directories.
