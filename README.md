<p align="center"><img src="docs/assets/banner.svg" alt="AirPulse — From environmental observations to an operational forecast." width="100%"></p>

<p align="center"><img src="docs/assets/badges/python.svg" alt="python" height="28"> <img src="docs/assets/badges/fastapi.svg" alt="fastapi" height="28"> <img src="docs/assets/badges/xgboost.svg" alt="xgboost" height="28"> <img src="docs/assets/badges/aws.svg" alt="aws" height="28"> <img src="docs/assets/badges/iceberg.svg" alt="iceberg" height="28"> <img src="docs/assets/badges/redis.svg" alt="redis" height="28"></p>

<p align="center"><strong>An end-to-end air-quality forecasting platform spanning ingestion, lakehouse storage, model training, and a FastAPI prediction service.</strong></p>

## Overview

An end-to-end air-quality forecasting platform spanning ingestion, lakehouse storage, model training, and a FastAPI prediction service.

### Why it matters

AirPulse treats forecasting as a data-system problem, keeping collection, historical storage, training artifacts, serving, and observability connected.

## Key features

- Hourly environmental-data ingestion
- S3, Athena, Glue, and Iceberg lakehouse path
- XGBoost training with MLflow tracking
- FastAPI inference with Redis-backed responses

## Architecture

```mermaid
flowchart LR
  A[Environmental data] --> B[S3 + Iceberg]
  B --> C[Feature engineering]
  C --> D[XGBoost training]
  D --> E[Model artifacts]
  E --> F[FastAPI service]
  F <--> G[Redis]
```

## Technology stack

**python** · **fastapi** · **xgboost** · **aws** · **iceberg** · **redis**

## Getting started

Use Python 3.12+ and `uv sync --frozen --group dev`, copy `.env.example`, and provide authorized cloud configuration plus compatible model artifacts. Start the API only after those prerequisites are in place.
