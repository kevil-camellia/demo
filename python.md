#### 1.豆瓣

爬虫代码--豆瓣静态爬虫

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

#### 2.作业6

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

