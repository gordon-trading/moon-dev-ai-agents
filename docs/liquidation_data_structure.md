# Liquidation Data Structure

## Overview
The liquidation data from Moon Dev API contains historical liquidation events with detailed order information.

## Data Structure

The DataFrame contains the following columns (in order):

### Column Definitions

| Column Name | Type | Description | Example Values |
|------------|------|-------------|----------------|
| `symbol` | string | Trading pair symbol | `"MON"`, `"ASTER"`, `"SOL"`, `"PTB"`, `"AIOT"` |
| `side` | string | Order side (BUY or SELL) | `"BUY"`, `"SELL"` |
| `order_type` | string | Type of order | `"LIMIT"` |
| `time_in_force` | string | Time in force for the order | `"IOC"` (Immediate or Cancel) |
| `original_quantity` | float | Original order quantity | `12555.00`, `86.00`, `2.45` |
| `price` | float | Order price | `0.04276`, `1.09940`, `141.99000` |
| `average_price` | float | Average execution price | `0.04236`, `1.08870`, `141.30000` |
| `order_status` | string | Status of the order | `"FILLED"` |
| `order_last_filled_quantity` | float | Last filled quantity | `12555.00`, `86.00`, `2.45` |
| `order_filled_accumulated_quantity` | float | Total accumulated filled quantity | `12555.00`, `86.00`, `2.45` |
| `order_trade_time` | int | Timestamp in milliseconds (epoch) | `1764251872343`, `1764251872344` |
| `usd_size` | float | USD value of the liquidation | `536.85180`, `94.54840`, `347.87550` |
| `datetime` | datetime | Converted datetime from order_trade_time | `2025-11-27 13:57:52.343` |

## Example Record

```python
{
    'symbol': 'MON',
    'side': 'BUY',
    'order_type': 'LIMIT',
    'time_in_force': 'IOC',
    'original_quantity': 12555.00,
    'price': 0.04276,
    'average_price': 0.04236,
    'order_status': 'FILLED',
    'order_last_filled_quantity': 12555.00,
    'order_filled_accumulated_quantity': 12555.00,
    'order_trade_time': 1764251872343,
    'usd_size': 536.85180,
    'datetime': Timestamp('2025-11-27 13:57:52.343000')
}
```

## Data Characteristics

1. **Sorting**: Data is sorted chronologically (oldest → newest), similar to OHLCV data
2. **Timestamp**: The `order_trade_time` column contains Unix timestamps in milliseconds
3. **Derived Column**: The `datetime` column is automatically added by converting `order_trade_time` from milliseconds to a pandas datetime object
4. **Data Range**: Based on your output, the data spans from `2025-11-27 13:57:52` to `2025-11-28 15:20:27`

## Usage Example

```python
from agents.api import MoonDevAPI
import pandas as pd

# Initialize API
api = MoonDevAPI()

# Get liquidation data
liquidations = api.get_liquidation_data(limit=50000)

# Access columns
print(liquidations['symbol'].unique())  # List of all symbols
print(liquidations['side'].value_counts())  # Count of BUY vs SELL
print(liquidations['usd_size'].sum())  # Total USD value

# Filter by symbol
mon_liquidations = liquidations[liquidations['symbol'] == 'MON']

# Filter by time range
recent = liquidations[liquidations['datetime'] > '2025-11-28 00:00:00']

# Group by symbol
by_symbol = liquidations.groupby('symbol').agg({
    'usd_size': 'sum',
    'original_quantity': 'sum',
    'order_trade_time': 'count'  # Count of liquidations
})
```

## Notes

- All liquidations appear to have `order_status` = `"FILLED"` (since they're completed liquidations)
- `time_in_force` is typically `"IOC"` (Immediate or Cancel)
- `order_type` is typically `"LIMIT"`
- The `usd_size` column represents the USD value of the liquidation event
- Data is sorted chronologically for time-series analysis compatibility

