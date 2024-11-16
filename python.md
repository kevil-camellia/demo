#### 1-豆瓣爬虫

：爬虫代码--豆瓣静态爬虫

```python
# encoding=utf-8
import requests
from bs4  import BeautifulSoup
import pandas as pd
headers =  {

    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.0.0 Safari/537.36"


}
response = requests.get("https://movie.douban.com/top250",headers=headers)
print(response.status_code)

html = response.text
soup =  BeautifulSoup(html,"html.parser")
all_titles = soup.findAll("span",attrs = {"class":"title"})
title_list=[]
for title in all_titles:
    title_string = title.string
    if "/" not in title_string:
        title_list.append(title_string)
        # print(title_string)
df = pd.DataFrame(title_list,columns=['Title'])

df.to_csv('douban.csv',index=False)
print("结束")
```

#### 2-作业6

```python

import random
import datetime
import time

def fun_ch():
    string = ''
    for i in range(10):
        string = string + str(i)
    for k in range(ord('A'),ord('Z')+1):
        string = string + chr(k)
    for t in range(ord('a'),ord('z')+1):
        string = string + chr(t)
    return string

a = fun_ch()
print(a)

def get_code():
    yzm = fun_ch()+ "@#$%&*+?"
    yzm_code = ''

    for i in range(6):
        yzm_code =  yzm_code + ''.join(random.choice(yzm))
    return yzm_code
b = get_code()
print(b)
def display():
    num = random.randint(5,10)
    for i in range(num):
        code = get_code()
        now = datetime.datetime.now() 
        print(f"Code: {code}, time: {now.strftime('%Y-%m-%d %H:%M:%S')}")
        time.sleep(5)

c = display()
print(c)
print("结束")
```

#### 3-情感分析

```python
from snownlp import SnowNLP
import pandas as pd

def sentiment_analysis(text):
    """
    使用 SnowNLP 进行中文文本情感分析。
    :param text: 中文文本字符串。
    :return: 情感分析结果，包括情感倾向（正面情感、负面情感）和情感值。
    """
    # 创建 SnowNLP 对象
    snlp = SnowNLP(text)
    # 获取情感值（范围 0~1，1 越接近正面情感越强）
    sentiment = snlp.sentiments

    # 根据情感值判断情感倾向
    if sentiment > 0.5:
        sentiment_polarity = '正面情感'
    else:
        sentiment_polarity = '负面情感'

    return sentiment_polarity, sentiment

# 读取 CSV 文件
df = pd.read_csv('4.csv',encoding='utf-8' )  # 读取 CSV 文件
comments = df['评论'].values  # 提取“评论”列的数据为 NumPy 数组
inclinations = []   # 情感倾向列表
values = []  # 情感值列表

# 对每条评论进行情感分析
for comment in comments:
    sentiment_polarity, sentiment_score = sentiment_analysis(comment)  # 调用情感分析函数
    inclinations.append(sentiment_polarity)  # 存储情感倾向
    values.append(sentiment_score)  # 存储情感值
    print(f"情感倾向: {sentiment_polarity}, 情感值: {sentiment_score}")

# 将情感分析结果添加到 DataFrame 中
df['情感倾向'] = inclinations  # 新增列“情感倾向”
df['情感值'] = values  # 新增列“情感值”

# 保存分析结果到新 CSV 文件
df.to_csv('3_分析结果.csv', index=False, encoding='utf-8-sig')  # 保存为 CSV 文件，适配中文字符

```

#### 4-词云分析

```python
import pandas as pd
import jieba
import wordcloud
import matplotlib.pyplot as plt
import numpy as np
import PIL

# 读取 CSV 文件
df = pd.read_csv("4.csv", encoding='utf-8')  # 确保文件路径正确

# 将 'content' 替换为需要分析的列名
title = ';'.join([str(c) for c in df['评论'].tolist()])

# 定义停用词列表
stop_words = {'一个', '这个', '可以', '27', '36','你们', '然后', '建议','真的','这泼天','真的','这么','就是','直接'}  # 添加需要去掉的词

# 分词并过滤停用词
gen = jieba.lcut(title)
filtered_words = [word for word in gen if word not in stop_words and len(word) > 1]  # 去掉停用词和单字词

# 统计词频
data = {}
for word in filtered_words:
    data[word] = data.get(word, 0) + 1

# 将词频转换为 DataFrame
hlist = list(data.items())
hlist.sort(key=lambda x: x[1], reverse=True)
dd = pd.DataFrame(hlist)
dd = dd.iloc[:30, :]  # 只保留前 30 个高频词

# 保存词频数据
dd.to_csv('6_词云分析文件.txt', encoding='utf-8', index=False, header=False, sep=':')


```

4-3333333
