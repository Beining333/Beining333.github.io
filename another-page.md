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
```Python
#导入必要的库
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore', category=FutureWarning)
```
```Python
#这里使用google drive
from google.colab import drive
drive.mount('/content/drive')
import os
path = '/content/drive/MyDrive/'
files = os.listdir(path)

loan_data = pd.read_csv('/content/drive/MyDrive/loan_data.xls')
loan_data.head()
```




[back](./)
