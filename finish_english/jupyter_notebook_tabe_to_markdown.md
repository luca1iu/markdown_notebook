<!-- TOC -->
* [How to convert](#how-to-convert)
      * [Example:](#example-)
<!-- TOC -->

# How to convert

I use Markdown to write my Blog, and I often show my dataframe in markdown file. Unfortunately, when I try to copy the
dataframe from jupyter notebook to markdown file, markdown does not recognize these copied dataframes very well.

and then I found a solution that can easily generate markdown friendly table from pandas dataframe.

that is:

```python 
df_markdown = df.to_markdown()
```

before using this function we need to install `tabulate`

```bash
pip install tabulate
```

#### Example:

```python
import pandas as pd
df = pd.DataFrame({'Date': ['2022/11/1', '2022/11/1', '2022/11/1', '2022/11/1'], 'Int': [233211, 24321, 35345, 23223],
                    'Int_with_seperator': [925478, 23484, 123249, 2345675],
                    'String': ['Apple', 'Huawei', 'Xiaomi', 'Oppo'], 'Float': [98.45, 65.24, 30, 80.88],
                    'Percent': [0.4234, 0.9434, 0.6512, 0.6133]})
print(df.to_markdown(index=False))
```

output:

| Date      |    Int |   Int_with_seperator | String   |   Float |   Percent |
|:----------|-------:|---------------------:|:---------|--------:|----------:|
| 2022/11/1 | 233211 |               925478 | Apple    |   98.45 |    0.4234 |
| 2022/11/1 |  24321 |                23484 | Huawei   |   65.24 |    0.9434 |
| 2022/11/1 |  35345 |               123249 | Xiaomi   |   30    |    0.6512 |
| 2022/11/1 |  23223 |              2345675 | Oppo     |   80.88 |    0.6133 |

