## 适用场景
当你有多个列名一致的excel文件的时候，你想要把多个excel文件合并成一个excel文件

## Python代码实现
1. 首先导入需要的库

```python
import pandas as pd
import os
```

2. 将所有需要合并的excel放进一个单独的文件夹里
3. 定义一个函数

```python
def append(path): #path：所有需要合并的excel文件所在的文件夹
    filename_excel = [] # 建立一个空list,用于储存所有需要合并的excel名称
    frames = [] # 建立一个空list,用于储存dataframe
    for root, dirs, files in os.walk(path):
        for file in files:
            file_with_path = os.path.join(root, file)
        filename_excel.append(file_with_path)
        df = pd.read_excel(file_with_path, engine='openpyxl')
        frames.append(df)
df = pd.concat(frames, axis=0)
return df
```

### 一些说明
* 上面的代码中root是就是当前文件夹的所有路径 
* files是一个list, 包含文件夹中所有excel的名称 
* os.path.join(root, file)就是合并文件夹的路径和文件名称，这样后面的pd.read_excel()就能读取excel文件

### tips
也可以不定义函数直接用：
```python
filename_excel = []
frames = []
for root, dirs, files in os.walk(path):
    for file in files:
        file_with_path = os.path.join(root, file)
        filename_excel.append(file_with_path)
        df = pd.read_excel(file_with_path, engine='openpyxl')
        frames.append(df)
df = pd.concat(frames, axis=0)
df.to_excel("合并的excel.xlsx")
```

## 特殊情况
### 如果excel的文件名包括日期，且需要写到最后汇总的excel中
```python
def append(path):
    filename_excel = []
    frames = []
    for root, dirs, files in os.walk(path):
        for file in files:
            file_with_path = os.path.join(root, file)
        filename_excel.append(file_with_path)
        df = pd.read_excel(file_with_path, engine='openpyxl')
        # 将文件名中包含的日期信息写入dataframe
        df["日期"] = pd.to_datetime(file.strip('.xls')[-1:])#日期在什么位置需要自己调整
        frames.append(df)
df = pd.concat(frames, axis=0)
return df
```

### 如果将多个excel合并到一个excel中，sheet命名为excel的名字
```python
def combine(path):
    with pd.ExcelWriter("合并的excel.xlsx") as writer:
        for root, dirs, files in os.walk(path):
            for file in files:
                filename = os.path.join(root, file)
                df = pd.read_excel(filename, engine='openpyxl')
                df.to_excel(writer, sheet_name=file.strip('.xls')) #删除文件名的后缀，有时候是.csv/.xlsx
        return df
```