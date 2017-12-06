---
layout:     post
title:      Python可视化库Seaborn使用笔记
subtitle:   Seaborn用法会持续更新
date:       2017-12-07
author:     巧不巧克力/ChocoYvan
header-img: img/host_bg_data.jpg
catalog: true
tags:
    - Python
    - 数据分析
    - 机器学习
---

# 整体布局风格设置

```
import seaborn as sns
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
%matplotlib inline#展示图
```

matplotlib绘制

```
def sinplot(flip=1):
    x = np.linspace(0, 14, 100)
    for i in range(1, 7):
        plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)

sinplot()
```

![](https://ws1.sinaimg.cn/large/006tNc79gy1fm4xg3b3flj30ae070js9.jpg)

seaborn默认绘制

```
sns.set()
sinplot()
```

![](https://ws4.sinaimg.cn/large/006tNc79gy1fm4xgst50vj30a706vaaz.jpg)

5种主题风格
darkgrid
whitegrid
dark
white
ticks

```
sns.set_style("whitegrid")#指定风格
sns.despine()#去掉上方和下方的轴
```

# 风格细节设置

```
sns.violinplot(data)
sns.despine(offset=10)#离轴线的距离
sns.despine(left=True)#隐藏左侧轴
```

![](https://ws4.sinaimg.cn/large/006tNc79gy1fm4xhugcw3j30a507hgln.jpg)

```
with sns.axes_style("darkgrid"):#with下风格不变
    plt.subplot(211)
    sinplot()
plt.subplot(212)
sinplot(-1)
```

![](https://ws3.sinaimg.cn/large/006tNc79gy1fm4xiemwxlj30a706v0u3.jpg)

```
sns.set_context("notebook", font_scale=1.5, rc={"lines.linewidth": 2.5})#指定内容风格（poster，talk，paper），字体大小，线宽
sinplot()
```

# 调色板

调色板
颜色很重要

color_palette()能传入任何Matplotlib所支持的颜色

color_palette()不写参数则默认颜色

set_palette()设置所有图的颜色

分类色板

```
current_palette = sns.color_palette()
sns.palplot(current_palette)
```

![](https://ws2.sinaimg.cn/large/006tNc79gy1fm4xjnzyrfj309p01w0c2.jpg)

6个默认的颜色循环主题： deep, muted, pastel, bright, dark, colorblind

圆形画板

当你有六个以上的分类要区分时，最简单的方法就是在一个圆形的颜色空间中画出均匀间隔的颜色(这样的色调会保持亮度和饱和度不变)。这是大多数的当他们需要使用比当前默认颜色循环中设置的颜色更多时的默认方案。

最常用的方法是使用hls的颜色空间，这是RGB值的一个简单转换。

```
sns.palplot(sns.color_palette("hls", 8))
```

![](https://ws1.sinaimg.cn/large/006tNc79gy1fm4xkj15gmj30cs01w0cr.jpg)

```
data = np.random.normal(size=(20, 8)) + np.arange(8) / 2
sns.boxplot(data=data,palette=sns.color_palette("hls", 8))#传入颜色
```

![](https://ws1.sinaimg.cn/large/006tNc79gy1fm4xl0c934j30a709wa9y.jpg)

hls_palette()函数来控制颜色的亮度和饱和

l-亮度 lightness

s-饱和 saturation

```
sns.palplot(sns.hls_palette(8, l=.7, s=.9))
```

![](https://ws2.sinaimg.cn/large/006tNc79gy1fm4xlqu4ccj30cs01w0cu.jpg)

```
sns.palplot(sns.color_palette("Paired",8))#颜色标记成对
```

![](https://ws3.sinaimg.cn/large/006tNc79gy1fm4xm818a8j30cs01w0dk.jpg)

# 调色板颜色设置

使用xkcd颜色来命名颜色

xkcd包含了一套众包努力的针对随机RGB色的命名。产生了954个可以随时通过xdcd_rgb字典中调用的命名颜色。

```
plt.plot([0, 1], [0, 1], sns.xkcd_rgb["pale red"], lw=3)
plt.plot([0, 1], [0, 2], sns.xkcd_rgb["medium green"], lw=3)
plt.plot([0, 1], [0, 3], sns.xkcd_rgb["denim blue"], lw=3)
```

连续色板

色彩随数据变换，比如数据越来越重要则颜色越来越深

```
sns.palplot(sns.color_palette("Blues"))
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm4xp8awbxj309p01w0c4.jpg)

如果想要翻转渐变，可以在面板名称中添加一个_r后缀

```
sns.palplot(sns.color_palette("BuGn_r"))
```

cubehelix_palette()调色板

色调线性变换

```
sns.palplot(sns.cubehelix_palette(8, start=.5, rot=-.75))
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm4xoq44rjj30cs01w0ds.jpg)

light_palette() 和dark_palette()调用定制连续调色板

```
sns.palplot(sns.light_palette("green"))
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm766ykz09j309p01w0bp.jpg)

```
x, y = np.random.multivariate_normal([0, 0], [[1, -.5], [-.5, 1]], size=300).T
pal = sns.dark_palette("green", as_cmap=True)
sns.kdeplot(x, y, cmap=pal);
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm767rsxu2j30a709w753.jpg)

# 单变量分析绘图

```
x = np.random.gamma(6, size=200)
sns.distplot(x, kde=False, fit=stats.gamma)
```

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fm768hmr8aj30al06v3ym.jpg)

根据均值和协方差生成数据

```
mean, cov = [0, 1], [(1, .5), (.5, 1)]
data = np.random.multivariate_normal(mean, cov, 200)
df = pd.DataFrame(data, columns=["x", "y"])
```

观测两个变量之间的分布关系最好用散点图

```
sns.jointplot(x="x", y="y", data=df);
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm769a6bxpj30bl0bojrk.jpg)

```
x, y = np.random.multivariate_normal(mean, cov, 1000).T
with sns.axes_style("white"):
    sns.jointplot(x=x, y=y, kind="hex", color="k")
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm769rud02j30bl0bo3z1.jpg)

```
iris = sns.load_dataset("iris")
sns.pairplot(iris)
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76aa3sdfj30je0jktat.jpg)

# 回归分析绘图

```
tips = sns.load_dataset("tips")
print(tips.head())
```

```
total_bill tip sex smoker day time size
0 16.99 1.01 Female No Sun Dinner 2
1 10.34 1.66 Male No Sun Dinner 3
2 21.01 3.50 Male No Sun Dinner 3
3 23.68 3.31 Male No Sun Dinner 2
4 24.59 3.61 Female No Sun Dinner 4
```

```
sns.regplot(x="total_bill", y="tip", data=tips)
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76bm25l1j30al079aa9.jpg)

regplot()和lmplot()都可以绘制回归关系,推荐regplot()

```
sns.regplot(x="size", y="tip", data=tips, x_jitter=.05)#添加浮动
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76c81gbij30al079aa5.jpg)

# 多变量分析绘图

```
sns.stripplot(x="day", y="total_bill", data=tips, jitter=True)
```

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fm76d60bcuj30al079q36.jpg)

```
sns.swarmplot(x="day", y="total_bill", data=tips)
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm76dj4447j30al07974q.jpg)

```
sns.swarmplot(x="day", y="total_bill", hue="sex",data=tips)
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76dwh2izj30al079mxn.jpg)

盒图

IQR即统计学概念四分位距，第一/四分位与第三/四分位之间的距离

N = 1.5IQR 如果一个值>Q3+N或　<　Ｑ1-N,则为离群点

```
sns.boxplot(x="day", y="total_bill", hue="time", data=tips);
```

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fm76ek810zj30al0793yf.jpg)

```
sns.violinplot(x="day", y="total_bill", hue="sex", data=tips, split=True);
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm76exh1e2j30al079wes.jpg)

# 分类属性绘图

显示值的集中趋势可以用条形图

```
sns.barplot(x="sex", y="survived", hue="class", data=titanic);
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm76fnonbuj30ao079jrb.jpg)

点图可以更好的描述变化差异

```
sns.pointplot(x="sex", y="survived", hue="class", data=titanic);
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm76g2q40rj30ao079aa9.jpg)

```
sns.pointplot(x="class", y="survived", hue="sex", data=titanic,
              palette={"male": "g", "female": "m"},
              markers=["^", "o"], linestyles=["-", "--"]);
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm76gf31t3j30ao079aa5.jpg)

宽形数据

```
sns.boxplot(data=iris,orient="h");
```

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fm76gx5085j30bh06vglj.jpg)

多层面板分类图（整合）

```
sns.factorplot(x="day", y="total_bill", hue="smoker", data=tips)
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm76hesq2wj309507oaa5.jpg)

```
sns.factorplot(x="time", y="total_bill", hue="smoker",
               col="day", data=tips, kind="box", size=4, aspect=.5)
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76hq9sn6j30h907o74a.jpg)

seaborn.factorplot(x=None, y=None, hue=None, data=None, row=None, col=None, col_wrap=None, estimator=, ci=95, n_boot=1000, units=None, order=None, hue_order=None, row_order=None, col_order=None, kind='point', size=4, aspect=1, orient=None, color=None, palette=None, legend=True, legend_out=True, sharex=True, sharey=True, margin_titles=False, facet_kws=None, **kwargs)

参数（Parameters）：

x,y,hue 数据集变量 变量名

date 数据集 数据集名

row,col 更多分类变量进行平铺显示 变量名

col_wrap 每行的最高平铺数 整数

estimator 在每个分类中进行矢量到标量的映射 矢量

ci 置信区间 浮点数或None

n_boot 计算置信区间时使用的引导迭代次数 整数

units 采样单元的标识符，用于执行多级引导和重复测量设计 数据变量或向量数据

order, hue_order 对应排序列表 字符串列表

row_order, col_order 对应排序列表 字符串列表

kind : 可选：point 默认, bar 柱形图, count 频次, box 箱体, violin 提琴, strip 散点，
swarm 分散点 

size 每个面的高度（英寸） 标量 

aspect 纵横比 标量 

orient 方向 "v"/"h" 

color 颜色 matplotlib颜色 

palette 调色板 seaborn颜色色板或字典 

legend hue的信息面板 True/False 

legend_out 是否扩展图形，并将信息框绘制在中心右边 True/False 

share{x,y} 共享轴线 True/False

# Facetgrid使用方法

```
%matplotlib inline
import numpy as np
import pandas as pd
import seaborn as sns
from scipy import stats
import matplotlib as mpl
import matplotlib.pyplot as plt

sns.set(style="ticks")
np.random.seed(sum(map(ord, "axis_grids")))
tips = sns.load_dataset("tips")
g = sns.FacetGrid(tips, col="time")
g.map(plt.hist, "tip");
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76kgqt5yj30bo05o745.jpg)

```
g = sns.FacetGrid(tips, col="sex", hue="smoker")
g.map(plt.scatter, "total_bill", "tip", alpha=.7)#透明程度
g.add_legend();
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76ktvnp8j30d705oweq.jpg)

自指定类别顺序

```
from pandas import Categorical
ordered_days = tips.day.value_counts().index
print (ordered_days)
ordered_days = Categorical(['Thur', 'Fri', 'Sat', 'Sun'])
g = sns.FacetGrid(tips, row="day", row_order=ordered_days,
                  size=1.7, aspect=4,)
g.map(sns.boxplot, "total_bill");
```

# Facetgrid绘制多变量

指定颜色

```
pal = dict(Lunch="seagreen", Dinner="gray")
g = sns.FacetGrid(tips, hue="time", palette=pal, size=5)
g.map(plt.scatter, "total_bill", "tip", s=50, alpha=.7, linewidth=.5, edgecolor="white")
g.add_legend();
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm76lq5vetj30bg09o0t0.jpg)

```
g = sns.FacetGrid(tips, hue="sex", palette="Set1", size=5, hue_kws={"marker": ["^", "v"]})
g.map(plt.scatter, "total_bill", "tip", s=100, linewidth=.5, edgecolor="white")
g.add_legend();
```

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fm76m1dqylj30bj09oaab.jpg)

```
with sns.axes_style("white"):
    g = sns.FacetGrid(tips, row="sex", col="smoker", margin_titles=True, size=2.5)
g.map(plt.scatter, "total_bill", "tip", color="#334488", edgecolor="white", lw=.5);
g.set_axis_labels("Total bill (US Dollars)", "Tip");
g.set(xticks=[10, 30, 50], yticks=[2, 6, 10]);
g.fig.subplots_adjust(wspace=.052, hspace=.052);
#g.fig.subplots_adjust(left  = 0.125,right = 0.5,bottom = 0.1,top = 0.9, wspace=.02, hspace=.02)
```
![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76mfkghlj30a109oweq.jpg)

# 热度图绘制

```
uniform_data = np.random.rand(3, 3)
print (uniform_data)
heatmap = sns.heatmap(uniform_data)
```

```
[[ 0.94466892 0.52184832 0.41466194]
[ 0.26455561 0.77423369 0.45615033]
[ 0.56843395 0.0187898 0.6176355 ]]
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76n3ba47j309m06vglf.jpg)

```
ax = sns.heatmap(uniform_data, vmin=0.2, vmax=0.5)#设置调色板取值区间
```

```
normal_data = np.random.randn(3, 3)
print (normal_data)
ax = sns.heatmap(normal_data, center=0)
```

```
[[ 1.53277921 1.46935877 0.15494743]
[ 0.37816252 -0.88778575 -1.98079647]
[-0.34791215 0.15634897 1.23029068]]
```

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fm76npnpycj309s06v744.jpg)

```
flights = sns.load_dataset("flights")
flights = flights.pivot("month", "year", "passengers")
print (flights)
ax = sns.heatmap(flights)
```

```
year 1949 1950 1951 1952 1953 1954 1955 1956 1957 1958 1959 \
month 
January 112 115 145 171 196 204 242 284 315 340 360 
February 118 126 150 180 196 188 233 277 301 318 342 
March 132 141 178 193 236 235 267 317 356 362 406 
April 129 135 163 181 235 227 269 313 348 348 396 
May 121 125 172 183 229 234 270 318 355 363 420 
June 135 149 178 218 243 264 315 374 422 435 472 
July 148 170 199 230 264 302 364 413 465 491 548 
August 148 170 199 242 272 293 347 405 467 505 559 
September 136 158 184 209 237 259 312 355 404 404 463 
October 119 133 162 191 211 229 274 306 347 359 407 
November 104 114 146 172 180 203 237 271 305 310 362 
December 118 140 166 194 201 229 278 306 336 337 405
year 1960 
month 
January 417 
February 391 
March 419 
April 461 
May 472 
June 535 
July 622 
August 606 
September 508 
October 461 
November 390 
December 432 
```

![](https://ws1.sinaimg.cn/large/006tKfTcgy1fm76o8bsz9j30b707n74e.jpg)

```
ax = sns.heatmap(flights, annot=True,fmt="d")
```

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fm76ojspr1j30b707nwg0.jpg)

```
ax = sns.heatmap(flights, cmap="YlGnBu")
```

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fm76ot9de8j30b707n3ym.jpg)