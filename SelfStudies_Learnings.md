# Complete Guide to Pandas Correlation Functions

## 1. Basic Correlation Functions

### `.corr()` - DataFrame/Series Correlation Matrix
```python
# Pairwise correlation of all columns
df.corr()
df.corr(method='pearson')  # Default
df.corr(method='spearman')
df.corr(method='kendall')

# Series correlation with another Series
series1.corr(series2)
```

### `.corrwith()` - Correlation Between DataFrames/Series
```python
# Correlate DataFrame columns with another DataFrame/Series
df1.corrwith(df2)           # Column-wise correlation
df1.corrwith(series)        # Each column vs series
df1.corrwith(df2, axis=1)   # Row-wise correlation
```

## 2. Time Series Correlation Functions

### `.autocorr()` - Series Autocorrelation
```python
# Correlation between series and its lagged version
series.autocorr()           # Default lag=1
series.autocorr(lag=5)      # 5-period lag autocorrelation
```

## 3. Window-based Correlation Functions

### Rolling Correlation - `.rolling().corr()`
```python
# Rolling correlation between two series
series1.rolling(window=30).corr(series2)

# Rolling pairwise correlations in DataFrame
df.rolling(window=30).corr()

# Rolling correlation with custom parameters
df['A'].rolling(window=20, min_periods=10).corr(df['B'])
```

### Expanding Correlation - `.expanding().corr()`
```python
# Expanding window correlation (from start to current point)
series1.expanding().corr(series2)

# Expanding pairwise correlations
df.expanding().corr()

# Expanding with minimum periods
df['A'].expanding(min_periods=10).corr(df['B'])
```

### Exponentially Weighted Correlation - `.ewm().corr()`
```python
# Exponentially weighted correlation
series1.ewm(span=30).corr(series2)

# EWM correlation with different parameters
df['A'].ewm(alpha=0.1).corr(df['B'])
df.ewm(halflife=10).corr()
```

## 4. Summary Table

| Function | Purpose | Input | Output | Use Case |
|----------|---------|-------|--------|----------|
| `.corr()` | Pairwise correlation matrix | DataFrame/Series | DataFrame/float | Static correlation analysis |
| `.corrwith()` | Cross-correlation between objects | DataFrame vs DataFrame/Series | Series | Compare datasets |
| `.autocorr()` | Self-correlation with lag | Series only | float | Time series patterns |
| `.rolling().corr()` | Moving window correlation | Series/DataFrame | Series/DataFrame | Dynamic relationships |
| `.expanding().corr()` | Cumulative correlation | Series/DataFrame | Series/DataFrame | Growing sample correlation |
| `.ewm().corr()` | Exponentially weighted correlation | Series/DataFrame | Series/DataFrame | Recent data emphasis |

## 5. Common Usage Examples

### Basic Correlation Analysis
```python
import pandas as pd
import numpy as np

# Sample data
df = pd.DataFrame({
    'A': np.random.randn(100),
    'B': np.random.randn(100),
    'C': np.random.randn(100)
})

# All pairwise correlations
correlation_matrix = df.corr()

# Correlation of one column with all others
correlations_with_A = df.corrwith(df['A'])
```

### Time Series Analysis
```python
# Time series with dates
dates = pd.date_range('2020-01-01', periods=365, freq='D')
ts = pd.Series(np.random.randn(365), index=dates)

# Check if series is autocorrelated
autocorr_lag1 = ts.autocorr(lag=1)
autocorr_lag7 = ts.autocorr(lag=7)  # Weekly pattern

# Rolling 30-day correlation between two series
rolling_corr = ts.rolling(30).corr(ts.shift(1))
```

### Advanced Window Analysis
```python
# Multiple window correlation analysis
stock_a = pd.Series(np.random.randn(252))  # Daily returns
stock_b = pd.Series(np.random.randn(252))

# Different correlation measures
rolling_30d = stock_a.rolling(30).corr(stock_b)
expanding = stock_a.expanding().corr(stock_b)
ewm_corr = stock_a.ewm(span=30).corr(stock_b)

# Compare results
results = pd.DataFrame({
    'rolling_30d': rolling_30d,
    'expanding': expanding,
    'ewm_30d': ewm_corr
})
```

## 6. Key Parameters Summary

### Common Parameters Across Functions:
- **`method`**: 'pearson', 'spearman', 'kendall' (where applicable)
- **`min_periods`**: Minimum observations required
- **`numeric_only`**: Include only numeric columns

### Window-specific Parameters:
- **`window`**: Size of moving window (rolling)
- **`span`**: Span for exponential weighting (ewm)
- **`alpha`**: Smoothing factor (ewm)
- **`halflife`**: Half-life for decay (ewm)
- **`lag`**: Number of periods to shift (autocorr)

## 7. Pro Tips

1. **Use `.corr()` for static analysis** - great for initial data exploration
2. **Use `.corrwith()` to compare datasets** - useful for feature selection
3. **Use `.autocorr()` for pattern detection** - identify seasonality/trends
4. **Use rolling correlation for dynamic analysis** - see how relationships change over time
5. **Use expanding correlation for stability analysis** - see if correlations stabilize
6. **Use EWM correlation for recent-weighted analysis** - emphasize recent observations

Remember: All correlation functions automatically handle NaN values and return results between -1 and 1 (except for custom callable methods).