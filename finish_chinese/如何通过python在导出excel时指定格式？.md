# 如何通过python在保存excel的时候指定数据格式？

在工作中遇到了这样的需求：
1. 需要将python处理过的多个dataframe保存在一个excel文件中，保存为不同的sheet.
2. dataframe中的日期需要在excel中显示为特定日期格式
3. dataframe中的百分比需要以百分比形式显示
4. 表头要有背景颜色


通过一个例子来看如何使用python解决上述问题：

## 问题描述
假设你有下面两张dataframe，你需要把这两张dataframe存在一个名为new_excel的文件里，保存为两个sheet.
```python
# generate two dataframes 
import pandas as pd

df1 = pd.DataFrame(
    {'Date': ['2022/12/1', '2022/12/1', '2022/12/1', '2022/12/1'], 'Int': [116382, 227393, 3274984, 438164],
     'Int_with_seperator': [1845132, 298145, 336278, 443816], 'String': ['Tom', 'Grace', 'Luca', 'Tessa'],
     'Float': [98.45, 65.24, 30, 80.88], 'Percent': [0.8878, 0.9523, 0.4545, 0.9921]})
df2 = pd.DataFrame({'Date': ['2022/11/1', '2022/11/1', '2022/11/1', '2022/11/1'], 'Int': [233211, 24321, 35345, 23223],
                    'Int_with_seperator': [925478, 23484, 123249, 2345675],
                    'String': ['Apple', 'Huawei', 'Xiaomi', 'Oppo'], 'Float': [98.45, 65.24, 30, 80.88],
                    'Percent': [0.4234, 0.9434, 0.6512, 0.6133]})
print(df1)
print(df2)
```
###### 第一张表：df1 
| Date      |     Int |   Int_with_seperator | String   |   Float |   Percent |
|:----------|--------:|---------------------:|:---------|--------:|----------:|
| 2022/12/1 |  116382 |              1845132 | Tom      |   98.45 |    0.8878 |
| 2022/12/1 |  227393 |               298145 | Grace    |   65.24 |    0.9523 |
| 2022/12/1 | 3274984 |               336278 | Luca     |   30    |    0.4545 |
| 2022/12/1 |  438164 |               443816 | Tessa    |   80.88 |    0.9921 |

###### 第二张表：df2
| Date      |    Int |   Int_with_seperator | String   |   Float |   Percent |
|:----------|-------:|---------------------:|:---------|--------:|----------:|
| 2022/11/1 | 233211 |               925478 | Apple    |   98.45 |    0.4234 |
| 2022/11/1 |  24321 |                23484 | Huawei   |   65.24 |    0.9434 |
| 2022/11/1 |  35345 |               123249 | Xiaomi   |   30    |    0.6512 |
| 2022/11/1 |  23223 |              2345675 | Oppo     |   80.88 |    0.6133 |

## 解决方法：使用xlsxwriter保存excel

#### 1. 导入包和创建excel文件
```python
# import package
import xlsxwriter

# create excel file
workbook = xlsxwriter.Workbook("new_excel.xlsx")
```

#### 2. 创建两个sheet
```python
worksheet1 = workbook.add_worksheet('df1_sheet')
worksheet2 = workbook.add_worksheet('df2_sheet')
```

#### 3. 设置表头的格式 保存表头
.write_row有四个参数： worksheet.write_row(行, 列, 数据, 单元格格式)
```python
header_format = workbook.add_format({
    'valign': 'top',
    'fg_color': '#002060',
    'border': 1,
    'font_color': 'white'})

worksheet1.write_row(0, 0, df1.columns, header_format)
worksheet2.write_row(0, 0, df2.columns, header_format)
```
结果预览 保存之后的excel表头会像这样：
![header](.save_excel_images/4dfa410f.png)

#### 4. 创建一些格式
你可以在[xlsxwrite](https://xlsxwriter.readthedocs.io/format.html)查看到数据格式的index.
比如14代表'm/dd/yyyy'的数据格式
format.set_num_format(14) = format.set_num_format('m/dd/yyyy')

```python
# 设置日期格式 "m/d/yy"
format_datetime = workbook.add_format({'border': 1})
format_datetime.set_num_format(14) # based on above table, 14 means "m/d/yy"
format_datetime.set_font_size(12) # set font size

# 通用格式
format_general = workbook.add_format({'border': 1})
format_general.set_num_format(0) # 0 means general
format_general.set_font_size(12)

# 整数格式
format_integer = workbook.add_format({'border': 1})
format_integer.set_num_format(1)
format_integer.set_font_size(12)

# set float format "0.00"
format_float = workbook.add_format({'border':1})
format_float.set_num_format(2)
format_float.set_font_size(12)

# set integer format with thousands separators "#,##0"
format_integer_separator = workbook.add_format({'border': 1})
format_integer_separator.set_num_format(3)
format_integer_separator.set_font_size(12)

# set percent format "0.00%"
format_percent = workbook.add_format({'border':1})
format_percent.set_num_format(10)
format_percent.set_font_size(12)
```

#### 5. 保存数据
excel里的第一行已经写入表头数据了，所有这里保存数据是从第二行开始，也就是索引为1。
write_column的第一个参数1代表从第二行开始写入数据。
与write_row一样，write_column也有四个参数，它们分别是write_column(行，列，数据，单元格格式)
```python
worksheet1.write_column(1, 0, df1.iloc[:, 0], format_datetime)
worksheet1.write_column(1, 1, df1.iloc[:, 1], format_integer)
worksheet1.write_column(1, 2, df1.iloc[:, 2], format_integer_separator)
worksheet1.write_column(1, 3, df1.iloc[:, 3], format_general)
worksheet1.write_column(1, 4, df1.iloc[:, 4], format_float)
worksheet1.write_column(1, 5, df1.iloc[:, 5], format_percent)

worksheet2.write_column(1, 0, df2.iloc[:, 0], format_datetime)
worksheet2.write_column(1, 1, df2.iloc[:, 1], format_integer)
worksheet2.write_column(1, 2, df2.iloc[:, 2], format_integer_separator)
worksheet2.write_column(1, 3, df2.iloc[:, 3], format_general)
worksheet2.write_column(1, 4, df2.iloc[:, 4], format_float)
worksheet2.write_column(1, 5, df2.iloc[:, 5], format_percent)
```

#### 6. 设置列宽
默认的列宽不够宽，导致在excel中，部分数据显示不完全，这里将列宽设置为15
```python
worksheet1.set_column('A:A', 15)
worksheet1.set_column('B:B', 15)
worksheet1.set_column('C:C', 15)
worksheet1.set_column('D:D', 15)
worksheet1.set_column('E:E', 15)
worksheet1.set_column('F:F', 15)

worksheet2.set_column('A:A', 15)
worksheet2.set_column('B:B', 15)
worksheet2.set_column('C:C', 15)
worksheet2.set_column('D:D', 15)
worksheet2.set_column('E:E', 15)
worksheet2.set_column('F:F', 15)
```


#### 7. 结束
```python
workbook.close()
```

##
完整代码： [Github](https://github.com/luca1iu/save_excel_with_xlsxwriter.git)