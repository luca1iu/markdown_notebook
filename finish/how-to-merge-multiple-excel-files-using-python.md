title: How to merge multiple excel files using python
<br>
date: 2022-5-26
<br>
abstract: When you have multiple similar excel files with the same column and you want to merge multiple excel files into one
excel file, when you want to iterate through files..

# How to merge multiple excel files using python

When you have multiple similar excel files with the same column and you want to merge multiple excel files into one
excel file, when you want to iterate through files..

## Python Method

1. put all the excel to be merged into a separate folder
2. define a function
3. replace the path parameter and use the function

```python
# import packages
import pandas as pd
import os

# define a funcion
def append(path):
    frames = []
    for root, dirs, files in os.walk(path):
        for file in files:
            file_with_path = os.path.join(root, file)
            df = pd.read_excel(file_with_path, engine='openpyxl')
            frames.append(df)
    df = pd.concat(frames, axis=0)
    return df

# use the function
df = append(path)  # path：The folder path where storage all the excel files
df.to_excel("merged_excel.xlsx")
```

### tips

It's also possible to use directly without defining the funtion

```python
import pandas as pd
import os 

frames = []
for root, dirs, files in os.walk(path):
    for file in files:
        file_with_path = os.path.join(root, file)
        df = pd.read_excel(file_with_path, engine='openpyxl')
        frames.append(df)
df = pd.concat(frames, axis=0)
df.to_excel("merged_excel.xlsx")
```

### notes:

1. “root” in the above code is the path to the folder that contains all the excel files to be merged
2. “files” is a list, containing the names of all the excel files in the folder
3. os.path.join(root, file) is to merge the paths and file names of the folders, so that pd.read_excel() can read the
   excel files later

## some special cases

### If the excel file name includes the date and needs to be written to the final summary excel

```python
import pandas as pd
import os

def append(path):
    frames = []
    for root, dirs, files in os.walk(path):
        for file in files:
            file_with_path = os.path.join(root, file)
            df = pd.read_excel(file_with_path, engine='openpyxl')
            df["date"] = pd.to_datetime(file.strip('.xls'))  # extract date string from the filename
            frames.append(df)
    df = pd.concat(frames, axis=0)
    return df
```

### If you need to merge multiple excel into one excel, and the sheet is named as the name of each excel name

```python
import pandas as pd
import os

def combine(path):
    with pd.ExcelWriter("merged_excel.xlsx") as writer:
        for root, dirs, files in os.walk(path):
            for file in files:
                filename = os.path.join(root, file)
                df = pd.read_excel(filename, engine='openpyxl')
                df.to_excel(writer, sheet_name=file.strip(
                    '.xls'))  # Delete the file name suffix, sometimes it could be .csv/.xlsx
        return df
```
