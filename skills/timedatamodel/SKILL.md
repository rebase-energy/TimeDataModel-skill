---
name: timedatamodel
description: Guides creation and manipulation of TimeDataModel objects (TimeSeries, TimeSeriesTable). Use when working with the timedatamodel Python library, time series with rich metadata, DataShape, Frequency, DataType, GeoLocation, or energy/forecasting data pipelines.
version: 1.0.0
metadata:
  pattern: tool-wrapper
  domain: timedatamodel
---

# TimeDataModel

TimeDataModel is a Python library for self-describing time series data with rich metadata. Core classes: `TimeSeries` (univariate) and `TimeSeriesTable` (multivariate). Backed by Polars internally.

```python
from timedatamodel import TimeSeries, TimeSeriesTable, Frequency, DataType, DataShape
```

## Four DataShapes

Shape is **auto-inferred** from column names present in the DataFrame.

| Shape | Required Columns | Use Case |
|-------|-----------------|----------|
| `SIMPLE` | `valid_time`, `value` | Observations, sensor readings |
| `VERSIONED` | `knowledge_time`, `valid_time`, `value` | Forecasts (knowledge_time = issue time) |
| `CORRECTED` | `valid_time`, `change_time`, `value` | Revised measurements |
| `AUDIT` | `knowledge_time`, `change_time`, `valid_time`, `value` | Full audit trail |

All timestamps stored as `pl.Datetime("us", time_zone="UTC")`. The `timezone` metadata field is display-only (IANA string).

## Creating a TimeSeries

```python
ts = TimeSeries.from_pandas(
    df,                           # DataFrame with timestamp + value columns
    frequency=Frequency.PT1H,
    name="wind_power",
    unit="MW",                    # canonical physical unit string
    data_type=DataType.OBSERVATION,
)
```

For `VERSIONED`, include `knowledge_time` + `valid_time` columns — shape inferred automatically.

## Creating a TimeSeriesTable

```python
table = TimeSeriesTable.from_pandas(
    df,
    frequency=Frequency.PT1H,
    units=["MW", "MW"],
    data_types=[DataType.OBSERVATION, DataType.FORECAST],
)
ts = table.select_column("col_name")   # returns TimeSeries
```

## Key Conventions

- All timestamps must be UTC-aware: `pd.date_range(..., tz="UTC")`
- `unit`: canonical physical unit string — `"MW"`, `"m/s"`, `"degC"`, `"dimensionless"`
- `data_type`: use leaf nodes of the DataType hierarchy (e.g. `OBSERVATION`, `FORECAST`)
- `timezone` metadata is a hint for display only, not for storage

## Choosing the Right Shape

Ask these questions before modeling:
1. Do you need to track **when** a value was produced? → `VERSIONED`
2. Do you need to track **when** a value was revised? → `CORRECTED`
3. Do you need both? → `AUDIT`
4. Neither? → `SIMPLE`

## References

- `references/enums.md` — Frequency, DataType, TimeSeriesType full value lists
- `references/workflows.md` — Import/export, slicing, unit conversion, geo filtering patterns
