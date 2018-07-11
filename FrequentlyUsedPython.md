# Jupyter

## Convert To Script

```bash
jupyter nbconvert --to script yournotebook.ipynb
```

## MagicFunction

Time a cell:
` %%time`

Draw with Retina resolution:
`
%config InlineBackend.figure_format = 'retina'
`

# Pandas
## Create new pd and append new rows

```python
import pandas as pd
res = pd.DataFrame(columns=('Time', '25%MAPE'))
slice1 = [{'Time':"24H", '25%MAPE':percentile(MAPEs,25)}]
res = res.append( slice1, ignore_index= True )
```
