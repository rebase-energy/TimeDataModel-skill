# TimeDataModel Workflows

## Import / Export

All formats work for both `TimeSeries` and `TimeSeriesTable`.

```python
# From pandas
ts = TimeSeries.from_pandas(df, frequency=Frequency.PT1H, name="x", unit="MW")

# To pandas — index depends on DataShape:
#   SIMPLE     → valid_time as index
#   VERSIONED  → (knowledge_time, valid_time) MultiIndex
#   CORRECTED  → (valid_time, change_time) MultiIndex
#   AUDIT      → (knowledge_time, change_time, valid_time) MultiIndex
df = ts.to_pandas()

# Polars (native backend)
df_pl = ts.to_polars()
ts = TimeSeries.from_polars(df_pl, frequency=Frequency.PT1H, name="x", unit="MW")

# Other formats
ts.to_list()      # dict of lists (column-oriented)
ts.to_numpy()     # dict of NumPy arrays
ts.to_pyarrow()   # PyArrow Table

TimeSeries.from_list(...)
TimeSeries.from_numpy(...)
TimeSeries.from_pyarrow(...)
```

## Inspecting a TimeSeries

```python
ts.shape           # DataShape enum (SIMPLE, VERSIONED, etc.)
ts.num_rows        # int
ts.has_missing     # bool — True if any null values present
ts.columns         # list of column names
ts.metadata_dict() # dict of all metadata fields
ts.coverage_bar()  # SVG in Jupyter, Unicode blocks in terminal
```

## Slicing

```python
ts.head(n)   # first n rows → TimeSeries
ts.tail(n)   # last n rows → TimeSeries
```

## Unit Conversion (requires `pint` extra)

```python
ts_kw = ts.convert_unit("kW")   # returns new TimeSeries with converted values
```

Install: `pip install timedatamodel[pint]`

## TimeSeriesTable Operations

```python
# Select one column as TimeSeries
ts = table.select_column("wind_power")

# Inspect columns
table.column_names        # list of signal names
table.metadata_dict()     # metadata per column
```

## Geospatial Filtering (requires `geo` extra)

```python
from timedatamodel import GeoLocation, GeoArea

center = GeoLocation(latitude=59.33, longitude=18.07)

# Filter columns within radius
nearby = table.filter_columns_by_location(center, radius_km=50)

# Get nearest column(s)
nearest = table.nearest_columns(center)
```

Install: `pip install timedatamodel[geo]`

## Validation

```python
# Check if series is valid for database insert (SIMPLE or VERSIONED only)
df, shape = ts.validate_for_insert()
```

## VERSIONED Series (Bi-temporal Forecasts)

```python
import pandas as pd
from timedatamodel import TimeSeries, Frequency, DataType

df = pd.DataFrame({
    "knowledge_time": pd.date_range("2024-01-01", periods=24, freq="h", tz="UTC"),
    "valid_time": pd.date_range("2024-01-02", periods=24, freq="h", tz="UTC"),
    "value": [float(i) for i in range(24)],
})

ts = TimeSeries.from_pandas(
    df,
    frequency=Frequency.PT1H,
    name="day_ahead_forecast",
    unit="MW",
    data_type=DataType.FORECAST,
)
# ts.shape → DataShape.VERSIONED (inferred automatically)
```

## Optional Extras Summary

| Extra | Enables |
|-------|---------|
| `timedatamodel[pandas]` | pandas + pyarrow interop |
| `timedatamodel[pint]` | unit conversion |
| `timedatamodel[geo]` | GeoLocation, GeoArea, spatial filtering |
| `timedatamodel[all]` | everything above |
