# How to update Oracle table using Python with Parameters?

for example, I want to update the DATE column in SALES table to the first day of the current month.

```python
# get the first day of the current month
import datetime

update_date_str = datetime.datetime(datetime.datetime.now().year, datetime.datetime.now().month, 1).strftime('%Y-%m-%d')

# connect oracle database
import cx_Oracle

conn = cx_Oracle.connect("account/password@host:port/servicename")
cur = conn.cursor()


# define the function
def update_SALES_date(date):
    try:
        update_sql = """UPDATE SALES t
    SET t.date = TO_DATE(:first_day_of_the_month, 'YYYY-MM-DD HH24:MI:SS')
    WHERE t.date IS NOT NULL"""
        cur.execute(update_sql, {'first_day_of_the_month': (date)})
        conn.commit()
        print('update and commit successfully ')
    except Exception as e:
        print("something is wrong", e)
        conn.rollback()


update_SALES_date(update_date_str)
```

after updating the table, we can also write a function to test if the update successfully is.

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine(
    "oracle+cx_oracle://account:password@host:port/servicename")


def test_update_SALES_date():
    df = pd.read_sql('''"SELECT * FROM SALES"''', con=engine)
    if all(df.date == update_date_str):
        print("update successful")
    else:
        print("update failed")
```

## How to update oracle table based on dataframe or local Excel?

in the example above: All values in the date column of the SALES table are updated to the 1st of the current month. what
if we want to update values row by row and based on some specific conditions?

##### for example: your local excel or dataframe looks like this:

| value  | condition1 | condition2|
|--------|------------|----------|
| value-new0 | con1-0     | con2-0 |
| value-new1 | con1-1     | con2-1 |
| value-new2 | con1-2     | con2-2 |

###### and the table in oracle database looks like this:

| value  | condition1 | condition2|
|---|-----------|-----|
| value-old0 | con1-0     | con2-0 |
| value-old1 | con1-1     | con2-1 |
| value-old2 | con1-2     | con2-2 |

in "value" column you want to update "value-old0"/"value-old1"/"value-old2" to "value-new0"/"value-new1"/"value-new2"
based on condition1 and condition2.

### Method:

#### 1. convert dataframe to tuples
this tuples will be the parameters
```python
df_tuple = []
for i in range(len(df)):
    item = tuple(df.iloc[i].values)
    df_tuple.append(item)

# df_tuple: [('value-new0', 'con1-0', 'con2-0'),
# ('value-new1', 'con1-1', 'con2-1'),
# ('value-new2', 'con1-2', 'con2-2')]
```

#### 2. connect the database

```python
import cx_Oracle
conn = cx_Oracle.connect("account/password6@host:port/servicename")
cur = conn.cursor()
```
#### 3. execute the sql command with parameters
```python
for i in range(len(df_tuple)):
    param = df_tuple[i]
    sql = """UPDATE TABLE t SET t.value1 = :1 WHERE t.condition1 LIKE :2 ESCAPE '#' AND t.condition2 LIKE :3 ESCAPE '#'"""
    sql_res = cur.execute(sql, param)
    print(i, param, ":", "update successful")
    
#if all df_tuple updates successfully, commit update
conn.commit()

#if updates fails, rollback the update
conn.rollback()
```
#### 5. close the connection
```python
cur.close()
conn.close()
```

### optimize the code
we can use try, except statements to handle the exceptions, and all the code will look like this:
```python
df_tuple = []
for i in range(len(df)):
    item = tuple(df.iloc[i].values)
    df_tuple.append(item)

import cx_Oracle
conn = cx_Oracle.connect("account/password6@host:port/servicename")
cur = conn.cursor()

try:
    for i in range(len(df_tuple)):
        param = df_tuple[i]
        sql = """UPDATE TABLE t SET t.value1 = :1 WHERE t.condition1 LIKE :2 ESCAPE '#' AND t.condition2 LIKE :3 ESCAPE '#'"""
        sql_res = cur.execute(sql, param)
        print(i, param, ":", "update successful")
    conn.commit()
except Exception as e:
    print("update failed", e)
    conn.rollback()

cur.close()
conn.close()
```
