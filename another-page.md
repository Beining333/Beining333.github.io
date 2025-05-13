---
layout: default
title: 风控
description: 风控策略学习
---
风控策略可分为单维度及多维度策略，对应贷前、贷中、贷后的一些风控场景。

## 策略开发

### 单维度策略
单维度策略开发分为策略测算与泛化，策略测算包括变量EDA、变量最优分箱、规则测算效果分析，策略泛化包括规则泛化、待上线规则集合并泛化等步骤，主要是对单个变量进行分析和挖掘。策略测算是在Train Set上分析策略效果， 策略泛化是在Validation Set上分析策略效果。

1. 变量描述性统计分析

> 在此参考Kaggle文章(Loan EDA)，包括数据清洗与描述性统计。
```
https://www.kaggle.com/code/matinmahmoudi/loan-eda-project-quick-start-for-beginners/input
```

```python
#导入必要的库
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore', category=FutureWarning)
```

```python
#这里使用google drive
from google.colab import drive
drive.mount('/content/drive')
import os
path = '/content/drive/MyDrive/'
files = os.listdir(path)
loan_data = pd.read_csv('/content/drive/MyDrive/loan_data.xls')
loan_data.head()
```

|index|credit\.policy|purpose|int\.rate|installment|log\.annual\.inc|dti|fico|days\.with\.cr\.line|revol\.bal|revol\.util|inq\.last\.6mths|delinq\.2yrs|pub\.rec|not\.fully\.paid|
|:---|:-----|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
|0|1|debt\_consolidation|0\.1189|829\.1|11\.35040654|19\.48|737|5639\.958333|28854|52\.1|0|0|0|0|
|1|1|credit\_card|0\.1071|228\.22|11\.08214255|14\.29|707|2760\.0|33623|76\.7|0|0|0|0|
|2|1|debt\_consolidation|0\.1357|366\.86|10\.37349118|11\.63|682|4710\.0|3511|25\.6|1|0|0|0|
|3|1|debt\_consolidation|0\.1008|162\.34|11\.35040654|8\.1|712|2699\.958333|33667|73\.2|1|0|0|0|
|4|1|credit\_card|0\.1426|102\.92|11\.29973224|14\.97|667|4066\.0|4740|39\.5|0|1|0|0|

```python
numerical_summary = loan_data.describe().transpose()
palette = sns.color_palette("Pastel2", as_cmap=True)
numerical_summary.style.background_gradient(cmap=palette)
```
![Statistical Summary](/assets/Risk_pic_1.png)

```python
loan_data.info()
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 9578 entries, 0 to 9577
Data columns (total 14 columns):

|index|Column|Non-Null Count|Dtype|  
|:---|:---|:---|:----|  
|0|credit.policy|9578 non-null|int64| 
|1|purpose|9578 non-null|object|
|2|int.rate|9578 non-null|float64|
|3|installment|9578 non-null|float64|
|4|log.annual.inc|9578 non-null|float64|
|5|dti|9578 non-null|float64|
|6|fico|9578 non-null|int64| 
|7|days.with.cr.line|9578 non-null|float64|
|8|revol.bal|9578 non-null|int64|
|9|revol.util|9578 non-null|float64|
|10|inq.last.6mths|9578 non-null|int64|
|11|delinq.2yrs|9578 non-null|int64|
|12|pub.rec|9578 non-null|int64|  
|13|not.fully.paid|9578 non-null|int64|
dtypes: float64(6), int64(7), object(1)
memory usage: 1.0+ MB

```python
non_boolean_numerical_features = ['int.rate', 'installment', 'log.annual.inc', 'dti', 'fico', 'days.with.cr.line', 'revol.bal', 'revol.util', 'inq.last.6mths',  'delinq.2yrs', 'pub.rec']
boolean_numeric_features = ['credit.policy', 'not.fully.paid']

# Visualize the distributions and box plots for numerical features, including log-transformed versions for skewed data
for column in non_boolean_numerical_features:
    fig, (ax1, ax2, ax3) = plt.subplots(1, 3, figsize=(18, 4))

    # Histogram for the distribution
    sns.histplot(loan_data[column], kde=False, color='skyblue', ax=ax1)
    ax1.set_title(f'Distribution of {column}')
    ax1.set_ylabel('Frequency')

    # Boxplot for the variable
    sns.boxplot(x=loan_data[column], color='lightgreen', ax=ax2)
    ax2.set_title(f'Boxplot of {column}')

    # Log transformation and plot if the data is skewed
    if loan_data[column].skew() > 1:
        loan_data[column+'_log'] = np.log1p(loan_data[column])
        sns.histplot(loan_data[column+'_log'], kde=False, color='orange', ax=ax3)
        ax3.set_title(f'Log-transformed Distribution of {column}')
    else:
        ax3.set_title(f'Log-transformed plot not necessary for {column}')
        ax3.axis('off')

    plt.tight_layout()
    plt.show()
```

![Individual Feature Review](/assets/Risk_pic_2.png)


[back](./)
