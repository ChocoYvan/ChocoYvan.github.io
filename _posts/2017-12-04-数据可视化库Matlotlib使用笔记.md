---
layout:     post
title:      数据可视化库Matplotlib使用笔记
subtitle:   Matplotlib用法会持续更新
date:       2017-12-04
author:     巧不巧克力/ChocoYvan
header-img: img/host_bg_data.jpg
catalog: true
tags:
    - Python
    - 数据分析
    - 机器学习
---

# 折线图绘制

读入数据

```
import pandas as pd
unrate = pd.read_csv('unrate.csv')
unrate['DATE'] = pd.to_datetime(unrate['DATE'])
print(unrate.head(12))
```

```
DATE VALUE
0 1948-01-01 3.4
1 1948-02-01 3.8
2 1948-03-01 4.0
3 1948-04-01 3.9
4 1948-05-01 3.5
5 1948-06-01 3.6
6 1948-07-01 3.6
7 1948-08-01 3.9
8 1948-09-01 3.8
9 1948-10-01 3.7
10 1948-11-01 3.8
11 1948-12-01 4.0
```

绘制折线图

```
import matplotlib.pyplot as plt
first_twelve = unrate[0:12]
plt.plot(first_twelve['DATE'], first_twelve['VALUE'])
plt.show()
```

![](https://ws4.sinaimg.cn/large/006tNc79gy1fm4wx76wpqj30ah070t8r.jpg)

横坐标输出变换

```
plt.plot(first_twelve['DATE'], first_twelve['VALUE'])
plt.xticks(rotation=45)
#print help(plt.xticks)
plt.show()
```

![](https://ws1.sinaimg.cn/large/006tNc79gy1fm4wyetskoj30af07rq30.jpg)

添加坐标

```
plt.plot(first_twelve['DATE'], first_twelve['VALUE'])
plt.xticks(rotation=90)
plt.xlabel('Month')
plt.ylabel('Unemployment Rate')
plt.title('Monthly Unemployment Trends, 1948')
plt.show()
```

![](https://ws3.sinaimg.cn/large/006tNc79gy1fm4wzbhb52j30at08mmxb.jpg)

# 子图操作

子图位置，几行几列第几个图

```
import matplotlib.pyplot as plt
fig = plt.figure()
ax1 = fig.add_subplot(3,2,1)
ax2 = fig.add_subplot(3,2,2)
ax2 = fig.add_subplot(3,2,6)
plt.show()
```

![](https://ws4.sinaimg.cn/large/006tNc79gy1fm4x05krsmj30ak070dfn.jpg)

子图大小设置和颜色设置

```
fig = plt.figure(figsize=(10,6))#大小
colors = ['red', 'blue', 'green', 'orange', 'black']#颜色
for i in range(5):
    start_index = i*12
    end_index = (i+1)*12
    subset = unrate[start_index:end_index]
    label = str(1948 + i)#标签设置
    plt.plot(subset['MONTH'], subset['VALUE'], c=colors[i], label=label)
plt.legend(loc='upper left')#标签提示位置
plt.xlabel('Month, Integer')
plt.ylabel('Unemployment Rate, Percent')
plt.title('Monthly Unemployment Trends, 1948-1952')

plt.show()
```

![](https://ws4.sinaimg.cn/large/006tNc79gy1fm4x0wttdwj30gr0araal.jpg)

# 条形图与散点图

读入所需数据

```
import pandas as pd
reviews = pd.read_csv('fandango_scores.csv')
cols = ['FILM', 'RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']
norm_reviews = reviews[cols]
print(norm_reviews[:1])
```

绘制条形图

```
import matplotlib.pyplot as plt
from numpy import arange

num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue', 'Fandango_Stars']
bar_heights = norm_reviews.ix[0, num_cols].values
print(bar_heights)
bar_positions = arange(5) + 0.75#柱形距离
tick_positions = range(1,6)
fig, ax = plt.subplots()

ax.bar(bar_positions, bar_heights, 0.5)#柱形宽度,barh为横式图
ax.set_xticks(tick_positions)
ax.set_xticklabels(num_cols, rotation=45)

ax.set_xlabel('Rating Source')
ax.set_ylabel('Average Rating')
ax.set_title('Average User Rating For Avengers: Age of Ultron (2015)')
plt.show()
```

```
[4.2999999999999998 3.5499999999999998 3.8999999999999999 4.5 5.0]

```

![](https://ws4.sinaimg.cn/large/006tNc79gy1fm4x2djcnjj30ax09x0st.jpg)

绘制散点图

```
fig = plt.figure(figsize=(3,6))
ax1 = fig.add_subplot(2,1,1)
ax2 = fig.add_subplot(2,1,2)
ax1.scatter(norm_reviews['Fandango_Ratingvalue'], norm_reviews['RT_user_norm'])
ax1.set_xlabel('Fandango')
ax1.set_ylabel('Rotten Tomatoes')
ax2.scatter(norm_reviews['RT_user_norm'], norm_reviews['Fandango_Ratingvalue'])
ax2.set_xlabel('Rotten Tomatoes')
ax2.set_ylabel('Fandango')
plt.show()
```

![](https://ws4.sinaimg.cn/large/006tNc79gy1fm4x33dtrtj30660aft8r.jpg)

# 柱形图与盒图

绘制柱形图

```
fandango_distribution = norm_reviews['Fandango_Ratingvalue'].value_counts()
fandango_distribution = fandango_distribution.sort_index()

imdb_distribution = norm_reviews['IMDB_norm'].value_counts()
imdb_distribution = imdb_distribution.sort_index()

print(fandango_distribution)
```

```
2.7 2
2.8 2
2.9 5
3.0 4
3.1 3
3.2 5
3.3 4
3.4 9
3.5 9
3.6 8
3.7 9
3.8 5
3.9 12
4.0 7
4.1 16
4.2 12
4.3 11
4.4 7
4.5 9
4.6 4
4.8 3
```

```
fig, ax = plt.subplots()
ax.hist(norm_reviews['Fandango_Ratingvalue'])
ax.hist(norm_reviews['Fandango_Ratingvalue'],bins=20)
ax.hist(norm_reviews['Fandango_Ratingvalue'], range=(4, 5),bins=20)
plt.show()
```

![](https://ws3.sinaimg.cn/large/006tNc79gy1fm4x4flarpj30ac070q2q.jpg)

```
fig = plt.figure(figsize=(8,8))
ax1 = fig.add_subplot(2,2,1)
ax2 = fig.add_subplot(2,2,2)
ax3 = fig.add_subplot(2,2,3)
ax4 = fig.add_subplot(2,2,4)
ax1.hist(norm_reviews['Fandango_Ratingvalue'], bins=20, range=(0, 5))
ax1.set_title('Distribution of Fandango Ratings')
ax1.set_ylim(0, 50)#y轴区间

ax2.hist(norm_reviews['RT_user_norm'], 20, range=(0, 5))
ax2.set_title('Distribution of Rotten Tomatoes Ratings')
ax2.set_ylim(0, 50)

ax3.hist(norm_reviews['Metacritic_user_nom'], 20, range=(0, 5))
ax3.set_title('Distribution of Metacritic Ratings')
ax3.set_ylim(0, 50)

ax4.hist(norm_reviews['IMDB_norm'], 20, range=(0, 5))
ax4.set_title('Distribution of IMDB Ratings')
ax4.set_ylim(0, 50)

plt.show()
```

![](https://ws3.sinaimg.cn/large/006tNc79gy1fm4x4yjas6j30dw0dejrf.jpg)

绘制盒图

```
num_cols = ['RT_user_norm', 'Metacritic_user_nom', 'IMDB_norm', 'Fandango_Ratingvalue']
fig, ax = plt.subplots()
ax.boxplot(norm_reviews[num_cols].values)
ax.set_xticklabels(num_cols, rotation=45)
ax.set_ylim(0,5)
plt.show()
```

![](https://ws2.sinaimg.cn/large/006tNc79gy1fm4x5k42o9j30a6097glk.jpg)

# 细节设置

颜色设置

```
import pandas as pd
import matplotlib.pyplot as plt

women_degrees = pd.read_csv('percent-bachelors-degrees-women-usa.csv')
major_cats = ['Biology', 'Computer Science', 'Engineering', 'Math and Statistics']


cb_dark_blue = (0/255, 107/255, 164/255)
cb_orange = (255/255, 128/255, 14/255)

fig = plt.figure(figsize=(12, 12))

for sp in range(0,4):
    ax = fig.add_subplot(2,2,sp+1)
    # The color for each line is assigned here.
    ax.plot(women_degrees['Year'], women_degrees[major_cats[sp]], c=cb_dark_blue, label='Women')
    ax.plot(women_degrees['Year'], 100-women_degrees[major_cats[sp]], c=cb_orange, label='Men')
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(major_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")

plt.legend(loc='upper right')
plt.show()
```

![](https://ws2.sinaimg.cn/large/006tNc79gy1fm4x6hclewj30jv0jfgmd.jpg)

```
fig = plt.figure(figsize=(18, 3))

for sp in range(0,6):
    ax = fig.add_subplot(1,6,sp+1)
    ax.plot(women_degrees['Year'], women_degrees[stem_cats[sp]], c=cb_dark_blue, label='Women', linewidth=3)
    ax.plot(women_degrees['Year'], 100-women_degrees[stem_cats[sp]], c=cb_orange, label='Men', linewidth=3)
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(stem_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")
plt.legend(loc='upper right')
plt.show()
fig = plt.figure(figsize=(18, 3))

for sp in range(0,6):
    ax = fig.add_subplot(1,6,sp+1)
    ax.plot(women_degrees['Year'], women_degrees[stem_cats[sp]], c=cb_dark_blue, label='Women', linewidth=3)
    ax.plot(women_degrees['Year'], 100-women_degrees[stem_cats[sp]], c=cb_orange, label='Men', linewidth=3)
    for key,spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xlim(1968, 2011)
    ax.set_ylim(0,100)
    ax.set_title(stem_cats[sp])
    ax.tick_params(bottom="off", top="off", left="off", right="off")
    
    if sp == 0:
        ax.text(2005, 87, 'Men')
        ax.text(2002, 8, 'Women')
    elif sp == 5:
        ax.text(2005, 62, 'Men')
        ax.text(2001, 35, 'Women')
plt.show()
```

![](https://ws4.sinaimg.cn/large/006tNc79gy1fm4x7burjgj30t405uq3i.jpg)

![](https://ws3.sinaimg.cn/large/006tNc79gy1fm4x7kn41dj30t505ugm7.jpg)

图像外部边缘的调整可以使用plt.tight_layout()进行自动控制，此方法不能够很好的控制图像间的间隔。

如果想同时控制图像外侧边缘以及图像间的空白区域，使用命令：

```
plt.subplots_adjust(left=0.2, bottom=0.2, right=0.8, top=0.8，hspace=0.2, wspace=0.3)
```

