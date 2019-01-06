# Python

## List All Files in a folder

More in the Source: https://stackoverflow.com/questions/3207219/how-do-i-list-all-files-of-a-directory

### Using glob

I found glob easier to select file of the same type or with something in common. Look at the following example:

```python
import glob

txtfiles = []
for file in glob.glob("*.txt"):
    txtfiles.append(file)
```

Using list comprehension

```python
import glob

mylist = [f for f in glob.glob("*.txt")]
```

### Get the full path name of a type of file into all subdirectories with walk

I find this very useful to find stuff in many directories, and it helped me finding a file about which I didn't remember the name:

```python
import os

# Getting the current work directory (cwd)
thisdir = os.getcwd()

# r=root, d=directories, f = files
for r, d, f in os.walk(thisdir):
    for file in f:
        if ".docx" in file:
            print(os.path.join(r, file))
```


# Jupyter

### Keyboard Shortcut Cheatsheet

https://www.cheatography.com/weidadeyue/cheat-sheets/jupyter-notebook/

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
# General Python

## see properties of object
```
for property, value in vars(theObject).iteritems():
    print property, ": ", value
```
## See members of a class
```
import inspect
inspect.getmembers(TheClass, lambda a:not(inspect.isroutine(a)))
```
