# Pandas
## Create new pd and append new rows

```python
res = pd.DataFrame(columns=('Time', '25%MAPE'))
slice1 = [{'Time':"24H", '25%MAPE':percentile(MAPEs,25)}]
res = res.append( slice1, ignore_index= True )
```