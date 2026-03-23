# TimeDataModel Claude Code Plugin

A Claude Code plugin that teaches Claude how to work with the [TimeDataModel](https://github.com/davide/timedatamodel) Python library.

## What it does

Provides Claude with expert knowledge of `TimeSeries` and `TimeSeriesTable` objects, including:
- Choosing the right `DataShape` (SIMPLE, VERSIONED, CORRECTED, AUDIT)
- Creating and converting time series from pandas, Polars, NumPy, PyArrow
- Using `Frequency`, `DataType`, and `TimeSeriesType` enums correctly
- Geospatial filtering, unit conversion, and validation workflows

## Installation

```
/plugin install timedatamodel@<your-github-user>/TimeDataModel#claude-plugin
```

Or, if published to a marketplace:

```
/plugin install timedatamodel
```

## Structure

```
claude-plugin/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
└── skills/
    └── timedatamodel/
        ├── SKILL.md          # Main skill (auto-invoked by Claude)
        └── references/
            ├── enums.md      # Frequency, DataType, TimeSeriesType values
            └── workflows.md  # Import/export, slicing, geo filtering patterns
```
