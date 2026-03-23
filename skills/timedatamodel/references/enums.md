# TimeDataModel Enums

## Frequency

ISO 8601 duration strings. Calendar-based durations vary with the calendar.

| Value | Period |
|-------|--------|
| `Frequency.P1Y` | 1 year |
| `Frequency.P3M` | 3 months (quarter) |
| `Frequency.P1M` | 1 month |
| `Frequency.P1W` | 1 week |
| `Frequency.P1D` | 1 day |
| `Frequency.PT1H` | 1 hour |
| `Frequency.PT30M` | 30 minutes |
| `Frequency.PT15M` | 15 minutes |
| `Frequency.PT10M` | 10 minutes |
| `Frequency.PT5M` | 5 minutes |
| `Frequency.PT1M` | 1 minute |
| `Frequency.PT1S` | 1 second |
| `Frequency.NONE` | No fixed frequency |

## DataType Hierarchy

Two-rooted tree. Use **leaf nodes** in practice. Navigate with `.parent`, `.children`, `.is_leaf`, `.root`.

```
ACTUAL
├── OBSERVATION       ← directly measured (sensors, meters)
└── DERIVED           ← computed from observations

CALCULATED
├── ESTIMATION
│   ├── FORECAST      ← future prediction with uncertainty
│   ├── PREDICTION    ← deterministic forward projection
│   ├── SCENARIO      ← hypothetical/what-if
│   ├── SIMULATION    ← model-based simulation
│   └── RECONSTRUCTION ← past state estimated from indirect data
└── REFERENCE
    ├── BASELINE      ← reference level for comparison
    ├── BENCHMARK     ← performance target
    └── IDEAL         ← theoretical optimum
```

```python
from timedatamodel import DataType

DataType.FORECAST.parent      # → DataType.ESTIMATION
DataType.FORECAST.is_leaf     # → True
DataType.ACTUAL.children      # → [DataType.OBSERVATION, DataType.DERIVED]
DataType.FORECAST.root        # → DataType.CALCULATED
```

## TimeSeriesType

| Value | Meaning |
|-------|---------|
| `TimeSeriesType.FLAT` | Standard non-overlapping timestamps |
| `TimeSeriesType.OVERLAPPING` | Overlapping windows (e.g. rolling forecasts with multi-index) |

Overlapping series use `(issue_time, valid_time)` as a MultiIndex in pandas.
