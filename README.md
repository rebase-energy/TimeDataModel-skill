# TimeDataModel — Claude Code Plugin

A Claude Code skill that makes Claude an expert in the [TimeDataModel](https://github.com/rebase-energy/TimeDataModel) Python library — the standard for self-describing, bi-temporal time series data in energy and forecasting pipelines.

Once installed, Claude automatically applies this knowledge whenever you work with `TimeSeries` or `TimeSeriesTable` objects. No commands to remember — it just works.

## What Claude learns

- **DataShape selection** — when to use `SIMPLE`, `VERSIONED`, `CORRECTED`, or `AUDIT` based on your modeling needs
- **Creating time series** — from pandas, Polars, NumPy, and PyArrow, with the right `Frequency`, `DataType`, and metadata
- **Bi-temporal modeling** — structuring forecasts with `knowledge_time` + `valid_time` for full auditability
- **Enums** — complete `Frequency`, `DataType` hierarchy, and `TimeSeriesType` reference
- **Workflows** — import/export, slicing, unit conversion (pint), geospatial filtering, and validation

## Installation

Search for it in Claude Code:

```
/find-skills timedatamodel
```

Or install directly:

```
/plugin install timedatamodel
```

## Example

After installing, just describe what you want:

> *"Create a versioned TimeSeries of hourly wind power forecasts from this pandas DataFrame"*

Claude will produce correct, idiomatic TimeDataModel code — right DataShape, right enums, UTC-aware timestamps — without you needing to look anything up.

## About TimeDataModel

[TimeDataModel](https://github.com/rebase-energy/TimeDataModel) is an open-source Python library by [Rebase Energy](https://rebase.energy) for modeling time series data with rich, machine-readable metadata. It is designed for energy systems, forecasting, and any domain where data provenance and bi-temporality matter.

```
pip install timedatamodel
```
