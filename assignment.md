# Assignment

## Brief

Write the Python codes for the following questions.

## Instructions

Paste the answer as Python in the answer code section below each question.

### Question 1

Question: Calculate the correlation between 'MSFT' and 'IBM' returns from a DataFrame of stock returns.

```python
import pandas as pd

# Assuming 'returns' is a DataFrame containing stock returns
returns = pd.DataFrame({
    'MSFT': [0.05, 0.07, -0.01],
    'IBM': [0.04, 0.02, 0.03]
})
```

Answer:

```python
returns["MSFT"].corr(returns["IBM"])
```

### Question 2

Question: Convert the columns `B` and `C` from wide to long format.

```python
import pandas as pd

df = pd.DataFrame({
'A': {0: 'a', 1: 'b', 2: 'c'},
'B': {0: 1, 1: 3, 2: 5},
'C': {0: 2, 1: 4, 2: 6}
})
```

Answer:

```python
melted = pd.melt(df, id_vars=['A'], value_vars=['B', 'C'])
```

### Question 3

Question: Slice the Series to return data from 5th to 15th January.

```python
import pandas as pd
import numpy as np

dates = pd.date_range("2023-01-01", "2023-01-31")
data = pd.Series(np.random.rand(len(dates)), index=dates)
```

Answer:

```python
data["2023-01-05":"2023-01-15"]
```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
