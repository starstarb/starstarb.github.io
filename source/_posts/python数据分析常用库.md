---
title: python数据分析常用库
tags: python
categories: python
abbrlink: 460b5ed
date: 2019-08-08 15:09:19
---
sklearn/surprise
![](https://github.com/starstarb/clouding/raw/master/head/f2deaa2d8bcb907b31c759089e262f09.jpg)
<!--more-->
网上有很多关于sklearn的学习教程，大部分都是简单的讲清楚某一方面，其实最好的教程就是官方文档。

　　官方文档地址：https://scikit-learn.org/stable/

（可是官方文档非常详细，同时许多人对官方文档的理解和结构上都不能很好地把握，我也打算好好学习sklearn，这可能是机器学习的神器），下面先简单介绍一下sklearn。

　　自2007年发布以来，scikit-learn已经成为Python重要的机器学习库了，scikit-learn简称sklearn，支持包括分类，回归，降维和聚类四大机器学习算法。还包括了特征提取，数据处理和模型评估者三大模块。

　　sklearn是Scipy的扩展，建立在Numpy和matplolib库的基础上。利用这几大模块的优势，可以大大的提高机器学习的效率。

　　sklearn拥有着完善的文档，上手容易，具有着丰富的API，在学术界颇受欢迎。sklearn已经封装了大量的机器学习算法，包括LIBSVM和LIBINEAR。同时sklearn内置了大量数据集，节省了获取和整理数据集的时间。




#### 安装surprise库报错
Surprise是一个开源推荐系统，包含了常用的矩阵分解算法、包括SVD、SVD++，NMF等等，GitHub地址：https://github.com/NicolasHug/Surprise
参考官方安装方式
其中缺少Microsoft Visual C++14，因此需要安装:
**安装地址**
链接：https://pan.baidu.com/s/1F2vTVhU8SQbMEoSBCk5jPw 
提取码：re39 
我的windows10安装surprise报错如下：
![](https://github.com/starstarb/clouding/raw/master/python/surprise%E6%8A%A5%E9%94%99.png)
**import surprise库不报错，但是没有相关SVD Dataset库**
cl.exe不存在环境路径配置好后，依然不行，打开cl.exe，报mspdb140.dll不存在，未解决。

###  surprise库官方文档分析（一）

**一：入门**

　1、基本用法

　（1）、自动交叉验证

　　Surprise有一套内置的 算法和数据集供您使用。在最简单的形式中，只需几行代码即可运行交叉验证程序：
```

from surprise import SVD
from surprise import Dataset
from surprise.model_selection import cross_validate


# Load the movielens-100k dataset (download it if needed),  
# 加载movielens-100k数据集（如果需要，下载）
data = Dataset.load_builtin('ml-100k')

# ＃我们将使用SVD算法。
# We'll use the famous SVD algorithm.
algo = SVD()

#运行5倍交叉验证并打印结果
# Run 5-fold cross-validation and print results
cross_validate(algo, data, measures=['RMSE', 'MAE'], cv=5, verbose=True)
```


输出结果：
```

Evaluating RMSE, MAE of algorithm SVD on 5 split(s).

                  Fold 1  Fold 2  Fold 3  Fold 4  Fold 5  Mean    Std     
RMSE (testset)    0.9398  0.9321  0.9413  0.9349  0.9329  0.9362  0.0037  
MAE (testset)     0.7400  0.7351  0.7400  0.7364  0.7370  0.7377  0.0020  
Fit time          5.66    5.47    5.46    5.60    5.77    5.59    0.12    
Test time         0.24    0.14    0.18    0.15    0.15    0.17    0.04    
```
该load_builtin()方法将提供下载movielens-100k数据集（如果尚未下载），并将其保存.surprise_data在主目录的文件夹中（您也可以选择将其保存在其他位置）。

我们在这里使用众所周知的 SVD 算法，但是有许多其他算法可用。

该cross_validate() 函数根据cv参数运行交叉验证过程，并计算一些accuracy度量。我们在这里使用经典的5倍交叉验证，但可以使用更高级的迭代器

（2）、测试集分解和fit()方法

如果您不想运行完整的交叉验证程序，可以使用对 train_test_split() 给定大小的训练集和测试集进行采样，并使用您的选择。您将需要使用将在列车集上训练算法的方法，以及将返回从testset进行的预测的方法：accuracy metricfit()test()

 
```

from surprise import SVD
from surprise import Dataset
from surprise import accuracy
from surprise.model_selection import train_test_split

# Load the movielens-100k dataset (download it if needed),
data = Dataset.load_builtin('ml-100k')

# sample random trainset and testset #  随机测试集和训练集
# test set is made of 25% of the ratings. # 将25%的数据设置成测试集
trainset, testset = train_test_split(data, test_size=.25)

# We'll use the famous SVD algorithm.
algo = SVD()

# Train the algorithm on the trainset, and predict ratings for the testset # 在训练集中训练算法，并预测数据
algo.fit(trainset)
predictions = algo.test(testset)

# Then compute RMSE
accuracy.rmse(predictions)
```


执行结果：

RMSE: 0.9461

（3）、训练整个训练集和predict()方法

显然，我们也可以简单地将算法拟合到整个数据集，而不是运行交叉验证。这可以通过使用build_full_trainset()将构建trainset对象的方法来完成 ：
复制代码
```
from surprise import KNNBasic
from surprise import Dataset

# Load the movielens-100k dataset
data = Dataset.load_builtin('ml-100k')

# Retrieve the trainset.
# 检索训练集
trainset = data.build_full_trainset()

# Build an algorithm, and train it.
# 构建算法并训练
algo = KNNBasic()
algo.fit(trainset)

uid = str(196)  # raw user id (as in the ratings file). They are **strings**!
iid = str(302)  # raw item id (as in the ratings file). They are **strings**!

# get a prediction for specific users and items.
# ＃获取特定用户和项目的预测。
pred = algo.predict(uid, iid, r_ui=4, verbose=True)
```


预测结果：
```
user: 196        item: 302        r_ui = 4.00   est = 4.06   {'actual_k': 40, 'was_impossible': False}
# est表示预测值
```
 以上都是使用内置的数据集。

2、使用自定义数据集

Surprise有一组内置 数据集，但您当然可以使用自定义数据集。加载评级数据集可以从文件（例如csv文件）或从pandas数据帧完成。无论哪种方式，您都需要Reader为Surprise定义一个对象，以便能够解析文件或数据帧。
复制代码
```
# 要从文件（例如csv文件）加载数据集，您将需要以下 load_from_file()方法：

from surprise import BaselineOnly
from surprise import Dataset
from surprise import Reader
from surprise.model_selection import cross_validate
import os

# path to dataset file
# 数据集路径
file_path = os.path.expanduser(r'C:/Users/FELIX/.surprise_data/ml-100k/ml-100k/u.data')

# As we're loading a custom dataset, we need to define a reader. In the
# movielens-100k dataset, each line has the following format:
# 'user item rating timestamp', separated by '\t' characters.
# ＃当我们加载自定义数据集时，我们需要定义一个reader。在
# ＃movielens-100k数据集中，每一行都具有以下格式：
# ＃'user item rating timestamp'，以'\ t'字符分隔。
reader = Reader(line_format='user item rating timestamp', sep='\t')

data = Dataset.load_from_file(file_path, reader=reader)

# We can now use this dataset as we please, e.g. calling cross_validate
# ＃我们现在可以随意使用这个数据集，例如调用cross_validate
cross_validate(BaselineOnly(), data, verbose=True)



# 要从pandas数据框加载数据集，您将需要该 load_from_df()方法。您还需要一个Reader对象，但只能rating_scale指定参数。数据框必须有三列，对应于用户（原始）ID，项目（原始）ID以及此顺序中的评级。因此，每行对应于给定的评级。这不是限制性的，因为您可以轻松地重新排序数据框的列

import pandas as pd

from surprise import NormalPredictor
from surprise import Dataset
from surprise import Reader
from surprise.model_selection import cross_validate


# Creation of the dataframe. Column names are irrelevant.
# ＃创建数据帧。列名无关紧要。
ratings_dict = {'itemID': [1, 1, 1, 2, 2],
                'userID': [9, 32, 2, 45, 'user_foo'],
                'rating': [3, 2, 4, 3, 1]}
df = pd.DataFrame(ratings_dict)

# A reader is still needed but only the rating_scale param is requiered.
# ＃仍然需要一个reader，但只需要rating_scale param。
reader = Reader(rating_scale=(1, 5))

# The columns must correspond to user id, item id and ratings (in that order).
# ＃列必须对应于用户ID，项目ID和评级（按此顺序）。
data = Dataset.load_from_df(df[['userID', 'itemID', 'rating']], reader)

# We can now use this dataset as we please, e.g. calling cross_validate
# ＃我们现在可以随意使用这个数据集，例如调用cross_validate 
cross_validate(NormalPredictor(), data, cv=2)
```


3、使用交叉验证迭代器

对于交叉验证，我们可以使用cross_validate()为我们完成所有艰苦工作的功能。但是为了更好地控制，我们还可以实现交叉验证迭代器，并使用split()迭代器的test()方法和算法的 方法对每个拆分进行预测 。这是一个例子，我们使用经典的K-fold交叉验证程序和3个拆分：
```

from surprise import SVD
from surprise import Dataset
from surprise import accuracy
from surprise.model_selection import KFold

# Load the movielens-100k dataset
data = Dataset.load_builtin('ml-100k')

# define a cross-validation iterator
# define一个交叉验证迭代器
kf = KFold(n_splits=3)

algo = SVD()

for trainset, testset in kf.split(data):

    # train and test algorithm.
    #训练和测试算法。
    algo.fit(trainset)
    predictions = algo.test(testset)

    # Compute and print Root Mean Squared Error
    # 计算并打印输出
    accuracy.rmse(predictions, verbose=True)

```

可以使用其他交叉验证迭代器，如LeaveOneOut或ShuffleSplit。在这里查看所有可用的迭代器。Surprise的交叉验证工具的设计源于优秀的scikit-learn API。

交叉验证的一个特例是当折叠已经被某些文件预定义时。例如，movielens-100K数据集已经提供了5个训练和测试文件（u1.base，u1.test ... u5.base，u5.test）。惊喜可以通过使用surprise.model_selection.split.PredefinedKFold 对象来处理这种情况：
复制代码
```
from surprise import SVD
from surprise import Dataset
from surprise import Reader
from surprise import accuracy
from surprise.model_selection import PredefinedKFold

# path to dataset folder
files_dir = os.path.expanduser('~/.surprise_data/ml-100k/ml-100k/')

# This time, we'll use the built-in reader.
reader = Reader('ml-100k')

# folds_files is a list of tuples containing file paths:
# [(u1.base, u1.test), (u2.base, u2.test), ... (u5.base, u5.test)]
train_file = files_dir + 'u%d.base'
test_file = files_dir + 'u%d.test'
folds_files = [(train_file % i, test_file % i) for i in (1, 2, 3, 4, 5)]

data = Dataset.load_from_folds(folds_files, reader=reader)
pkf = PredefinedKFold()

algo = SVD()

for trainset, testset in pkf.split(data):

    # train and test algorithm.
    algo.fit(trainset)
    predictions = algo.test(testset)

    # Compute and print Root Mean Squared Error
    accuracy.rmse(predictions, verbose=True)
```

当然，也可以对单个文件进行训练和测试。但是folds_files参数仍然要列表的形式。

4、使用GridSearchCV调整算法参数

该cross_validate()函数报告针对给定参数集的交叉验证过程的准确度度量。如果你想知道哪个参数组合能产生最好的结果，那么这个 GridSearchCV类就可以解决了。给定一个dict参数，该类详尽地尝试所有参数组合并报告任何精度测量的最佳参数（在不同的分裂上取平均值）。它受到scikit-learn的GridSearchCV的启发。

 
```

from surprise import SVD
from surprise import Dataset
from surprise.model_selection import GridSearchCV

# Use movielens-100K
data = Dataset.load_builtin('ml-100k')

param_grid = {'n_epochs': [5, 10], 'lr_all': [0.002, 0.005],
              'reg_all': [0.4, 0.6]}
gs = GridSearchCV(SVD, param_grid, measures=['rmse', 'mae'], cv=3)

gs.fit(data)

# best RMSE score
print(gs.best_score['rmse']) # 输出最高的准确率的值

# combination of parameters that gave the best RMSE score
print(gs.best_params['rmse']) # 输出最好的批次，学习率参数

```

通过上面操作得到最佳参数后就可以使用该参数的算法：
```
# We can now use the algorithm that yields the best rmse:
algo = gs.best_estimator['rmse']
algo.fit(data.build_full_trainset())

```