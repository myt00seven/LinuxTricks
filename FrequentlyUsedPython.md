# Pandas
## Create new pd and append new rows

```python
res = pd.DataFrame(
    columns=('Time', '25%MAPE', '50%MAPE', "75%MAPE", '25%MSE', '50%MSE', "75%MSE", '25%MAE', '50%MAE', "75%MAE"))
slice1 = [{'Time':"24H", '25%MAPE':percentile(MAPEs,25), 
           '50%MAPE':percentile(MAPEs,50), "75%MAPE":percentile(MAPEs,75),
            '25%MSE':percentile(MSEs,25), 
            '50%MSE':percentile(MSEs,50), "75%MSE":percentile(MSEs,75),
            '25%MAE':percentile(MAEs,25), 
            '50%MAE':percentile(MAEs,50), "75%MAE":percentile(MAEs,75)
          }]
res = res.append( slice1, ignore_index= True )
```
