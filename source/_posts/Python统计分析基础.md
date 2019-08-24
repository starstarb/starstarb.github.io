---
title: Python统计分析基础
tags: Python统计分析
categories: python
abbrlink: e35b35f9
date: 2019-07-18 09:10:43
password: 753159
message: Welcome to my blog, enter password to read.  
abstract: Welcome to my blog, enter password to read.  
---
Python统计分析基础代码练习及总结
![](https://github.com/starstarb/clouding/raw/master/head/Cg-4k1oDG4KIGCdgACYDwGi8noIAAN44APLck0AJgPY537.jpg)
<!--more-->
### 一、打开jupyter notebook，方便记录python代码
cmd->pip install ipython->pip install notebook(pip install jupyter)->ipython notebook(jupyter notebook)
### 二、python基础语法

  




### 三、数据预处理
```
import numpy as np
import timeit 
def python_multi(n):
    a=range(n)
    b=range(n)
    c=[]
    for i in range(len(a)):
        a[i]=i**2
        b[i]=i**3
        c.append(a[i]*b[i])
    return c
def numpy_multi(n):
    c=np.arrange(n)**2*np.arange(n)**3
    return c
#上述结果测试
a=timeit.Timer("python_multi",setup="from __main__ import python_multi")
print(a.timeit(number=1000000))
b=timeit.Timer("numpy_multi",setup="from __main__ import numpy_multi")
print(b.timeit(number=1000000))
#明显numpy内置的基本库函数快于自己实现的

#测试
def func():
    normal_list=range(10000)
    L = [i**2 for i in normal_list]
timer1 = timeit.Timer("func()", setup="from __main__ import func")
print(timer1.timeit(number=10))
#0.122326436316035`

```
Python3 range() 函数返回的是一个可迭代对象（类型是对象），而不是列表类型， 所以打印的时候不会打印列表。
Python3 list() 函数是对象迭代器，可以把range()返回的可迭代对象转为一个列表，返回的变量类型为列表。



### Python的数据描述



### Python统计图形与可视化



###### matplotlib基本绘图






### Python简单统计推断
   常用数据分析工具库：
   scipy常用功能：
   * scipy.cluster:矢量量化、K-均值
   * scipy.integrate：积分
   * scipy.interpolate：插值
   * scipy.fftpack:傅里叶变换
   * scipy.linalg:线性代数程序
   * scipy.singal:信号处理
   * scipy.stats:统计
   * scipy.spatial:空间数据结构和算法
   * scipy.optimize:优化
   * scipy.ndimage:n维图像包
   * scipy.io:数据输入输出
   * scipy.sparse:稀疏矩阵
 ```
 from scipy import stats
 import statsmodels.api as sm
 from sklearn import svm


 ```


### 数据分析步骤

  1数据源选择

　　数据分析团队面对大量的数据源，各个数据源之间交叉联系，各个数据域之间具有逻辑关系，各个产品统计口径不同，不同的时间段数值不同等。这一系列问题多会影响数据分析结果，因此确定数据源选择和数据整理至关重要。

　　DBA可以基于数据分析需要，找到相关数据，建立一张数据宽表，将数据仓库的数据引入到这张宽表当中，基于一定的逻辑关系进行汇总计算。这张宽表作为数据分析的基础，然后再依据数据分析需要衍生出一些不同的表单，为数据分析提供干净全面的数据源。宽表一方面是用于集中相关分析数据，一方面是提高效率，不需要每次分析时都查询其他的数据表，影响数据仓库效率。

　　2数据抽样选择

　　简单的数据分析可以调用全体数据进行分析，数据抽样主要用于建模分析，抽样需考虑样本具有代表性，覆盖各种客户类型，抽样的时间也很重要，越近的时间窗口越有利于分析和预测。在进行分层抽样时，需要保证分成出来的样本比例同原始数据基本一致。

　　3数据类型选择

　　数据类型分为连续型和离散型，建模分析时需要确定数据类型。进行业务收入趋势分析、销售额预测分析、RFM分析时，一般采用连续型变量。信用评级、分类预测时一般采用离散变量。

　　4缺失值处理

　　数据分析过程中会面对很多缺失值，其产生原因不同，有的是由于隐私的原因，故意隐去。有的是变量本身就没有数值，有的是数据合并时不当操作产生的数据缺失。

　　缺失值处理可以采用替代法（估值法），利用已知经验值代替缺失值，维持缺失值不变和删除缺失值等方法。具体方法将参考变量和自变量的关系以及样本量的多少来决定。

　　5异常值检测和处理

　　异常值对于某些数据分析结果影响很大，例如聚类分析、线性回归（逻辑回归）。但是对决策树、神经网络、SVM支持向量机影响较小。

　　一般异常值是指明显偏离观测值的平均值，例如年龄为200岁，平均收入为10万元时，有个异常值为300万元。第一个异常值为无效异常值，需要删掉，但是第二个异常值可能属于有效异常值，可以根据经验来决定是否保留或删掉。

　　6数据标准化

　　数据标准化的目的是将不同性质、不同量级的数据进行指数化处理，调整到可以类比的范围。例如在建立逻辑回归模型时，性别的取值是0或以，但是收入取值可能就是0-100万，跨度较大，需要进行标准化。

　　一般可以采用最佳／最大标准化（Min－Max标准化法）将数值定在0和1之间，便于计算。Z分数法和小数定标标准化法也可以采用。

　　7数据粗分类（Categorization）处理

　　归类和分类的目的是减少样本的变量，常有的方法由等间距分类，等频数分类。可以依据经验将自变量分成几类，分类的方法可以不同，建议采用卡方检验来决定采用哪种分类方法。连续型变量可以用WOE变化方法来简化模型，但降低了模型的可解释性。

　　8变量选择

　　数据分析过程中会面对成百上千的变量，一般情况下只有少数变量同目标变量有关，有助于提高预测精度。通常建模分析时，有意义的变量不会超过10-15个，称他们为强相关变量（聪明变量）。可以利用变量过滤器的方法来选择变量。


