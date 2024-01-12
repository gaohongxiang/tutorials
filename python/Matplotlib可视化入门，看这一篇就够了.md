Matplotlib是 Python 最著名的2D绘图库，提供了丰富的数据绘图工具，主要用于绘制一些统计图形。Matplotlib可用于Python脚本，Python和IPython shell，Jupyter笔记本，Web应用程序服务器和四个图形用户界面工具包。

[matplotlib.pyplot API 官方教程](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.html)

[高效使用 Python 可视化工具 Matplotlib](http://www.itboth.com/d/InqUNr/python-matplotlib)

相信初接触matplotlib肯定会有些困惑，同样一个目标有多个解决方案，有人用`plt.xx`，有人用`ax.xx`，对小白及其不友好，照葫芦画瓢搞出来了，但不明白为什么，写出来的代码很混乱。下次遇到问题依然一头雾水。我也是这样，所以，花时间读了很多文章，整理了一下知识点，入门够用了。另，涉及到的文章都贴出了链接，可以一并阅读，加深理解。

## matplotlib架构

matplotlib的架构分为以下三层

- Scripting (脚本）层
- Artist (表现)层。拥有许多可视化元素，如figure、axes、axis等元素。
- Backend (后端)层。包含 pyplot 和 pylab 模块（已弃用，不推荐）

它们之间的访问关系是：

>Scripting 访问 Artist， Artist 访问 Backend

理论上各层都可以画出相同的图形，但越底层的操作越细节越困难，越高层越易于人机交互，越容易。也就是说上层是下层的封装，把一些不需要打交道的事情封装好，实际画图只关心效果即可。

[matplotlib 架构](https://zhuanlan.zhihu.com/p/50872418)

## matplotlib两种绘图API

在Matplotlib库中提供了两种风格的API供开发者使用。这也就是为什么有人用`plt.xx`，有人用`ax.xx`的原因。好的代码应该坚持使用一种风格，否则会显得混乱，阅读起来困难，不利于维护。

- Pyplot编程接口（state-based）`不推荐`
- 面向对象的编程接口（object-based）`推荐`

>之前有三种api，还有一种pylab api，模仿matlab的工作方式，但大量导入全局命名方式导致意外行为，被认为是糟糕的风格，matplotlib最新版中已弃用此方式。[理解matplotlib、pylab与pyplot之间的关系](https://oldpan.me/archives/matplotlib-pylab-pyplot)
>
>另外，pandas自带的df.plot()方法也可以绘制简单的图形，它是使用matplotlib库的plot()方法的简单包装实现的。最好不要用，pandas作为数据分析来用，绘图就交给matplotlib等专业的绘图库来做。下文也介绍一下，下次看到这种解决方案可以心中有数，不迷惑。

[Matplotlib中的两种绘图API说明](https://blog.csdn.net/theonegis/article/details/81230211)

[matplotlib 实际上就是提供了 3 种API](http://www.360doc.com/content/19/0830/09/9824753_857897234.shtml)

[Matplotlib中的plt和ax都是啥？](https://zhuanlan.zhihu.com/p/137057629)

[matplotlib：先搞明白plt. /ax./ fig再画
](https://zhuanlan.zhihu.com/p/93423829)

#### matplotlib图像的结构

![matplotlib_PartsOfAFugure_2021_01_16](https://gitee.com/ghongxiang/picture/raw/master/编程/language/python/matplotlib_PartsOfAFugure_2021_01_16.png)

在matplotlib中，整个图像为一个Figure对象，相当于画板。在Figure对象中可以包含一个或者多个Axes对象（画纸）。每个Axes(ax)对象都是一个拥有自己坐标系统axis的绘图区域。

[Python matplotlib结构详解](http://www.itboth.com/d/iY32Af/python-matplotlib)

以两个例子来介绍两种方法的使用。下面两幅图分别用pyplot编程接口和面向对象编程接口实现。单子图、多子图都涉及到了，用法在代码里表达的很清楚了。

单子图柱状图

![matplotlib_两种绘图api比较示例_单子图_2021_01_18](https://gitee.com/ghongxiang/picture/raw/master/编程/language/python/matplotlib_两种绘图api比较示例_单子图_2021_01_18.png)

多子图柱状图

![matplotlib_两种绘图api比较示例_多子图_2021_01_18](https://gitee.com/ghongxiang/picture/raw/master/编程/language/python/matplotlib_两种绘图api比较示例_多子图_2021_01_18.png)

一些其他图形如折线图、散点图、饼图等跟示例的柱状图大同小异，各种参数自行学习。[不容错过的Matplotlib常见用法小结](https://blog.csdn.net/qq_42103091/article/details/107832442) 非常全面的Matplotlib画图方法、[Matplotlib 1.4W+字教程的图形绘制部分](https://mp.weixin.qq.com/s?__biz=MzUwOTg0MjczNw==&mid=2247487201&idx=1&sn=f5828b89117af26a05f6894cca5879d1&chksm=f90d4abfce7ac3a99ce48484f4ea324e5a686a4c3942a4e237c2fb6820f0beef5563b58c4fdb#rd)

- plot 折线图
- bar 柱状图
- barh 条形图
- hist 直方图
- pie 饼图
- boxplot 箱形图
- scatter 散点图
- polar 极坐标图

#### Pyplot编程接口（state-based）`不推荐`

Pyplot封装了底层的绘图函数提供了一种绘图环境，当使用plt.xx绘制图形的时候，默认的Figure以及Axes等对象会自动创建以支持图形的绘制。此方式屏蔽了一些底层通用的绘图对象的创建细节，书写简洁。另外，pyplot是有状态的，亦即它会保存当前图片和作图区域的状态，新的作图函数会作用在当前图片的状态基础之上，代码的位置要注意。遇到一些比较复杂的图时不方便。官方推荐使用面向对象方式绘图。

```
import numpy as np
import matplotlib.pyplot as plt
 
# 准备数据
dog = (20, 28, 22, 30)
cat = (25, 32, 20, 27)

ind = np.arange(len(dog)) # 生成0-3的数组，用于x轴刻度值
quarter = ['一季度','二季度','三季度','四季度']
width = 0.35    # 图形柱的宽度

#################################################
# 一个子图

# 绘图
# x的位置调节好，不然两个柱会重叠
plt.bar(ind - width/2, dog, width, label='Dog')
plt.bar(ind + width/2, cat, width, label='Cat')

# 调节组件
plt.ylabel('销售额（万）')
plt.title('各季度猫狗产业销售额')
plt.xticks(ind,quarter)
plt.legend(loc=2)

# 保存图形
plt.savefig('matplotlib_pyplot方式单子图.png')
#################################################

#################################################
# 多个子图。有状态，细节调整需在各个subplot内调节

# 绘图 第一个子图
plt.subplot(121)    #1行2列，占用第一个
plt.bar(ind, dog, width, color='dodgerblue', label='Dog')

# 调节组件
plt.ylabel('销售额（万）')
plt.title('各季度狗产业销售额')
plt.xticks(ind,quarter)

# 绘图 第二个子图
plt.subplot(122)    #1行2列，占用第二个
plt.bar(ind, cat, width, color='darkorange', label='Cat')

# 调节组件
plt.ylabel('销售额（万）')
plt.title('各季度猫产业销售额')
plt.xticks(ind,quarter)

# 保存图形
plt.savefig('matplotlib_pyplot方式多子图.png')
#################################################

# 交互展示
plt.show()
```

#### 面向对象的编程接口（object-based）`推荐`

面向对象的绘图方式主要使用matplotlib的两个子类：matplotlib.figure.Figure和matplotlib.axes.Axes。我们需要自己创建figure（画板），axes（画纸）。画板（figure）为matplotlib.figure.Figure的一个实例，每个子图（画纸，axes）为matplotlib.axes.Axes的一个实例，分别可以继承父类的所有方法，也就是说你绘图时，你想设置的元素（网格线啊，坐标刻度啊等）都可以在二者的属性中找出来使用。使用面向对象编程接口有利于我们对于图形绘制的完整控制，处理复杂图形更有优势。但是相对于Pyplot接口可能需要书写更多的代码。

```
import numpy as np
import matplotlib.pyplot as plt

# 准备数据
dog = (20, 28, 22, 30)
cat = (25, 32, 20, 27)

ind = np.arange(len(dog)) # 生成0-3的数组，用于x轴刻度值
quarter = ['一季度','二季度','三季度','四季度']
width = 0.35    # 图形柱的宽度

#################################################
# 一个子图

# 绘图
fig, ax = plt.subplots()    # 实例画板fig画纸ax
# x的位置调节好，不然两个柱会重叠
ax.bar(ind - width/2, dog, width, label='Dog')
ax.bar(ind + width/2, cat, width, label='Cat')

# 调节组件
ax.set_ylabel('销售额（万）')
ax.set_title('各季度猫狗产业销售额')
ax.set_xticks(ind)
ax.set_xticklabels(quarter)
ax.legend(loc=2)

# 保存图形
fig.savefig('matplotlib_两种绘图api比较示例_单子图.png')
#################################################

#################################################
# 多个子图。无状态，ax[0]，ax[1]代表各子图

# 绘图
fig, ax = plt.subplots(1,2) #(1,2)代表1行2列两个区域
# 设置图形对象 ：窗口

# ax[0]代表第一个子图，ax[1]代表第二个子图
ax[0].bar(ind, dog, width, color='dodgerblue', label='Dog')
ax[1].bar(ind, cat, width, color='darkorange', label='Cat')

# 调节组件
ax[0].set_ylabel('销售额（万）')
ax[0].set_title('各季度狗产业销售额')
ax[0].set_xticks(ind)
ax[0].set_xticklabels(quarter)

ax[1].set_ylabel('销售额（万）')
ax[1].set_title('各季度猫产业销售额')
ax[1].set_xticks(ind)
ax[1].set_xticklabels(quarter)

# 保存图形
fig.savefig('matplotlib_两种绘图api比较示例_多子图.png')
#################################################

# 交互展示
plt.show()  # 面向对象接口不能使用交互式的show()方法对图像直接进行显示。如果想交互显示，需要用pyplot方式
```

#### pandas自带df.plot()方法 `不要用`

pandas自带的df.plot()方法也可以绘制简单的图形，它是使用matplotlib库的plot()方法的简单包装实现的。最好不要用，pandas作为数据分析来用，绘图就交给matplotlib等专业的绘图库来做。

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# 准备数据
dog = (20, 28, 22, 30)
cat = (25, 32, 20, 27)
data = {
        'Dog': dog,
        'Cat': cat}

ind = np.arange(len(dog)) # 生成0-3的数组，用于x轴刻度值
quarter = ['一季度','二季度','三季度','四季度']
width = 0.35    # 图形柱的宽度

# 生成DataFrame
df = pd.DataFrame(data,index=ind)

# df.plot()方法绘图。kind代表绘图类型
ax = df.plot(kind='bar')

# 调节组件
ax.set_ylabel('销售额（万）')
ax.set_title('各季度猫狗产业销售额')
ax.set_xticks(ind)
ax.set_xticklabels(quarter)
ax.legend(loc=2)

# 交互展示
plt.show()
```

![matplotlib_两种绘图api比较示例_单子图_2021_01_18](https://gitee.com/ghongxiang/picture/raw/master/编程/language/python/matplotlib_两种绘图api比较示例_单子图_2021_01_18.png)

[Pandas的可视化操作（利用pandas得到图表）](https://www.cnblogs.com/zhuminghui/p/9418820.html)

## matplotlib组件详解

在matplotlib中，整个图像为一个Figure对象。在Figure对象中可以包含一个或者多个Axes对象。每个Axes(ax)对象都是一个拥有自己坐标系统的绘图区域。

matplotlib的图的构成元素

![matplotlib_AnatomyOfAFigure_2021_01_16](https://gitee.com/ghongxiang/picture/raw/master/编程/language/python/matplotlib_AnatomyOfAFigure_2021_01_16.jpg)

- 坐标轴(axis)
- 坐标轴名称(axis label)
- 坐标轴刻度(tick)
- 坐标轴刻度标签(tick label)
- 坐标轴边界（lim）
- 网格线(grid)
- 图例(legend)
- 标题(title)
- 边框（spine）

[建议收藏！Matplotlib常见组件设置整理](https://zhuanlan.zhihu.com/p/137677737)

#### 中文乱码、负号显示

[知乎问题-中文乱码、负号不能正常显示。看猴子的回答](https://www.zhihu.com/question/25404709)

#### 设置标题 `ax.set_title()`

>单子图用`ax.`，多子图用`ax[].`

```
ax.set_title('标题',fontdict={'size':16},loc = 'left')    #设置16px的字体大小，将标题显示在左侧
```
#### 设置边框(spine) `ax.spines`

默认图表中会显示上下左右四条spine，可根据需要设置为不显示
```
ax.spines['right'].set_visible(False)   #去除右边的spines
ax.spines['bottom'].set_color('r')  #设置底部的spines为红色
```
#### 设置坐标轴相关

##### 设置坐标轴名称
```
ax.set_xlabel('季度',fontsize=16)
ax.set_xlabel('销售额',fontsize=16)
```

##### 设置坐标轴刻度标签

```
# 更改刻度标签。此例将0，1，2，3更改为一季度，二季度，三季度，四季度
ax.set_xticks(ind)
ax.set_xticklabels(quarter)

# 设置刻度标签属性
# axis : 可选{‘x’, ‘y’, ‘both’} ，选择对哪个轴操作，默认是’both’
# labelsize设置刻度标签的大小
# direction{‘in’, ‘out’, ‘inout’}刻度线的方向
# color : 刻度线的颜色
# labelcolor : 刻度值颜色
ax.tick_params(axis = 'y', labelsize=14,direction='in',labelcolor='r')
```
[ax.tick_params()参数详解](https://blog.csdn.net/qq_35240640/article/details/89478662)

##### 设置刻度间隔

```
#从pyplot导入MultipleLocator类，这个类用于设置刻度间隔
from matplotlib.pyplot import MultipleLocator

#把x轴的刻度间隔设置为1，并存在变量里
x_major_locator=MultipleLocator(1)

#把y轴的刻度间隔设置为10，并存在变量里
y_major_locator=MultipleLocator(10)

#把x轴的主刻度设置为1的倍数
ax.xaxis.set_major_locator(x_major_locator)

#把y轴的主刻度设置为10的倍数
ax.yaxis.set_major_locator(y_major_locator)
```

##### 设置坐标轴边界
```
ax.set_xlim([0,12])  # x轴边界
ax.set_ylim([0,50])  # y轴边界
```

#### 设置图例 `ax.legend()`

图例是对图形所展示的内容的解释，比如在一张图中画了三条线，那么这三条线都代表了什么呢？这时就需要做点注释。

两种方式设置图例

```
# 第一种
# 绘图的时候加上label,之后调用ax.legend()
ax.bar(ind - width/2, dog, width, label='Dog')
ax.bar(ind + width/2, cat, width, label='Cat')
ax.legend()

# 第二种
# 使用ax.legend()按顺序设置好图例
ax.bar(ind - width/2, dog, width)
ax.bar(ind + width/2, cat, width)
ax.legend(['Dog','Cat'])
```

##### 图例位置

loc参数用来规定图例的位置

如：将图例放在左上角：`ax.legend(loc=2)`

图例各位置如下

- 0: ‘best'
- 1: ‘upper right'
- 2: ‘upper left'
- 3: ‘lower left'
- 4: ‘lower right'
- 5: ‘right'
- 6: ‘center left'
- 7: ‘center right'
- 8: ‘lower center'
- 9: ‘upper center'
- 10: ‘center'

#### 设置网格线 `ax.grid()`

网格线多用于辅助查看具体的数值大小，横纵坐标都可以设置相应的网格线，视具体情况而论。

```
# b参数设置是否显示网格
# linestyle：线型
# color：颜色
# linewidth：宽度
# axis：x，y，both，显示x/y/两者的格网。默认both
ax.grid(b = True, linestyle = "--",color = "gray", linewidth = "0.5",axis = 'y') 
```
#### 设置背景颜色

设置整个图像背景颜色
```
fig.set_facecolor('white')
```
设置某个子图背景颜色
```
ax.set_facecolor('white')
```

#### 添加文本

[Python数据可视化：往图表中添加文本就是这么简单](https://zhuanlan.zhihu.com/p/234971599?utm_source=wechat_session)

#### 保存图像 `fig.savefig`

保存图像的时候可以设置分辨率

```
fig.savefig('xxx.png',dpi=300)
```

#### 交互展示 `ax.show()`

显示最终绘制的图像
```
ax.show()
```