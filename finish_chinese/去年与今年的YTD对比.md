# Excel-Power Query: 去年与今年的YTD对比

## 问题描述
我们从这张很简单的源数据开始
![Row Data](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105110100.png)

在我写这篇博客的现在是2023年1月，我们需要计算2023年YTD的销售总和，并计算出去年同一时期的销售总和进行对比。最终结果：
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105172542.png)

## 解决方案：

### 第一步：创建表格
选中所有数据然后点击`Insert`里的`Table`
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105110411.png)

### 第二步：开启Power Query
选中所有数据再点击`Data`中的`From Table/Range`进入Power Query
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com//20230105110444.png)

### 第三步：修改数据类型
将日期格式从`7/27/2023 12:00:00 AM` 改成 `7/27/2023`
在power query中右击日期列，选择修改类型成"日期"
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105110825.png)

### 第四步：新建列：从日期中摘取出年份和月份
Year: Date.Year([DATE])
Month: Date.Month([DATE])
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105113109.png)

### 第五步：新建列：当前月份
Current Month: Date.Month(Date.From(DateTime.LocalNow()))
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143105.png)

### 第六步：选择所有月份小于或等于当前月份的数据
1. 新建列：Month_Bool: [Month] <=[Current Month]
   ![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143304.png)
2. 添加过滤条件:只选择Month_Bool为True的数据
   ![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143402.png)

### 第七步: groupby Group和Year
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143559.png)

结果是这样：
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105144121.png)

### 第八步：处理年份列，让2022数据和2023数据分别在两列中
1. 复制当前结果表并筛选年份为2023
   ![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143940.png)
2. 筛选原表数据为2022
3. 基于`Group`进行合并查询然后展示Sales-Sum列
   ![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105144309.png)

### 第九步： 隐藏年份列并重命名列
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105144453.png)

