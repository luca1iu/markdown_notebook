# Excel-Power Query: Year to Date comparison for current year vs. Prior year
# Excel-Power Query: 去年与今年的YTD对比

## question description
## 问题描述

We start with a very simple table of data with Sales in 2022 and 2023:
![row data](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230103162512.png)


## step1: Create able
select all the data and then click `Table` in `Insert`
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/Capture.PNG)

## step2: start power query
click `From Table/Range` in `Data` then you will entry into Power Query Editor
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230103161302.png)

## Step3: change date type
in Power Query Editor, right click date column and click `Change Type` to `Date`

## Step4: duplicate the query and rename them
1. duplicate the query and rename them as `Prior Year` and `Current Year`
2. in `Prior Year` set a Filter of `Date`, in `Date Filters` set Year Equals `Last Year`
   ![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230103173156.png)
3. in `Current Year` set a Filter of `Date`, in `Date Filters` set Year Equals `This Year`


![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230103172900.png)