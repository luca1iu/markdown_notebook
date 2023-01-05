# Excel-Power Query: Year to Date comparison for current year vs. Prior year
# Excel-Power Query: 去年与今年的YTD对比

## question description
## 问题描述

We start with a very simple table of data with Sales in 2022 and 2023:  
![Row Data](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105110100.png)
![1](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105110100.png)

![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105110100.png)
## Step 1: Create able
select all the data and then click `Table` in `Insert`  
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105110411.png)

## Step 2: Start Power Query
select all the data and click `From Table/Range` in `Data` then you will entry into Power Query Editor  
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com//20230105110444.png)


## Step 3:  Change Date Type
in Power Query Editor, right click date column and click `Change Type` to `Date`  
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105110825.png)



## Step 4: add custom columns: extract Year and Month from Date
Year: Date.Year([DATE])
Month: Date.Month([DATE])
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105113109.png)


## Step 5: add custom column: current momth
Current Month: Date.Month(Date.From(DateTime.LocalNow()))
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143105.png)

## Step 6: select the data that month less than or equal to current month
1. add custom column Month_Bool: [Month] <=[Current Month]
   ![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143304.png)
2. filter rows:
   ![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143402.png)

## Step 7 : groupby Group and Year
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143559.png)

the result looks like this:
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105144121.png)


## Step 8 : transform the Year column
1. duplicate the current query and keep rows `Year` equals 2023
   ![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105143940.png)


2. in `Result`: keep rows `Year` equals 2022
3. Merge queries on `Group` and expend the Sales-Sum column
   ![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105144309.png)


## Step 9 : hide Year column and rename columns
![](https://lucablog-deutschland.oss-eu-central-1.aliyuncs.com/20230105144453.png)

