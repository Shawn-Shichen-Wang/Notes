[TOC]

# Data Wrangling

## 定义和类比

[wrangle](https://www.merriam-webster.com/dictionary/wrangle) 的意思是围拢、饲养，或负责牲畜，比如马或羊。

> 一个牧羊人的主要任务是把羊带到牧场，让羊在牧场吃草，把它们赶到市场剪毛，最后再把它们赶到畜棚里睡觉。但是，在此之前，必须将它们赶到一个良好组织的羊群中。但如果没有这样做呢？那么完成上面这些任务可能需要更长的时间。如果羊群分散了，一些羊可能会跑掉，丢失，甚至狼可能会偷偷溜进羊群，享受肥羊美餐。

### 类比

执行操作前，组织数据的想法对于那些牧羊人来说是一样的。为了获取满意的结果，我们需要整理数据，否则可能会出现一些意外后果。如果我们在整理之前就开始分析、设想或建模数据，结果可能会出现错误，缺乏冷静的思考，从而浪费掉了宝贵的时间。所以最好的做法是先整理，而且应该始终如此。

Python 及其库的发展使得数据整理变得更加容易。在这节课中，我们将学习如何像数据工程师那样整理数据。

***把每个问题写下来，停一会儿，揣摩要做的事，整理工作就变得让人感到快乐，像是一个执行任务的侦探在有条不紊的解开谜题***

当数据来源较多时，容易形成脏数据

[坡度图](https://charliepark.org/slopegraphs/)

核心步骤

1. 收集
2. 评估
3. 清洗数据

---

## 数据来源

由一个亚美尼亚人力资源门户网站发布的 [从2004 年到 2015 年19000 个在线职位数据集](https://www.kaggle.com/udacity/armenian-online-job-postings)。

---

## 收集

### 最好的方法：以编程方式下载文件

从互联网下载文件时，可以通过单击下载按钮（或者有时右键单击链接，然后单击*"将文件另存为"*）手动完成下载。但实际上最好的方法是以编程方式下载文件，即使用代码，原因有两点：**可拓展性**和**重复性**。

1. **可扩展性**：想象一下，你需要在上千个不同的网页下载上千个文件，而不是一个。要一个文件一个文件点击上千次，就太浪费时间了。但可以选择用几行代码来代替你完成这个任务。
2. **重复性**：无论是你还是别人，将来都可能需要运行分析，所以还是要尽可能地轻松下载数据集或数据集。重复性也是 [科学方法](https://en.wikipedia.org/wiki/Scientific_method#Documentation_and_replication) 的主要原则之一。你想向人们证明自己的分析、可视化等是合乎规则的。人们需要知道，他们可否根据你的数据、你的计算环境、你的代码等重现你的结果！此外，数据集或其网页可能会发生变化，因此，如果添加下载数据集的日期，就可以让这些未来的学习者有机会访问数据集的归档副本，或至少了解其结果不同的原因。

### Python解压

- [ZipFile Objects](https://docs.python.org/3/library/zipfile.html#zipfile-objects)

  - ZipFile是一个上下文管理器(Context Manager)，支持With语句
    - [生命周期：什么是 ZIP 文件？](https://www.lifewire.com/zip-file-2622675)
    - [Jeff Knupp: 上下文管理器](https://jeffknupp.com/blog/2016/03/07/python-with-context-managers/) 
      如果上面链接打不开，可以参考[这里](http://book.pythontips.com/en/latest/context_managers.html)。
  - Context Manager确保打开的文件会被关闭

  ```Python
  with zipfile.ZipFile('archive.zip', 'r') as myzip:
      myzip.extractall()
  ```

  

#### CSV

- Comma Separated Values
- 第一行代表列名

- [read_csv文档](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html)

### 其他

- Pandas [官方文档](https://pandas.pydata.org/pandas-docs/stable/10min.html)
- [DigitalOcean：Aliasing 模块](https://www.digitalocean.com/community/tutorials/how-to-import-modules-in-python-3#aliasing-modules)
- [Stack Overflow：你能否为在 Python 中导入的模块定义别名？](https://stackoverflow.com/questions/706595/can-you-define-aliases-for-imported-modules-in-python)

![party](DataWrangling.assets/party.gif)

---

## 评估

### **质量**

**低质量数据**通常被称为**脏数据**。脏数据存在**内容**问题。

常见的数据质量问题包括：

- 数据丢失，如 Juan 缺少的身高值。
- 数据无效，如单元格中的值不符合常规，例如像 Kwasi 的身高为负数。从技术角度来说，身高中具有 "英寸" 和 “厘米" 也是无效的，因为如果存在身高条目时，身高的数据类型变为字符串。身高的数据类型应为整数或浮点数。
- 数据不准确，比如，Jane 的实际身高是 58 英寸，而不是 55 英寸。
- 数据不一致，比如，使用不同的长度单位（英寸和厘米）。

数据质量指查看或评估数据在特定情境下是否能准确表示其目的。但这是一个模糊不清的定义，很重要的是：数据质量是没有一个精确标准的。一个数据集可能非常适用于一个应用，但不一定也适用于另一个。

### **整洁度**

**不整洁数据***通常被称为 **"杂乱" 数据**。杂乱数据存在**结构**问题。

整洁度数据是统计学家、教授和全能数据专家 [Hadley Wickham](http://hadley.nz/) 提出的一个相对较新的概念。我准备在这个主题中引用他的优秀 [论文](https://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html)：

> 据说 80％ 的数据分析用于清洗和准备数据。这不仅仅是第一步，但必须在分析过程中重复多次，因为会出现新的问题或收集到新的数据。为了解决这些问题，这节课重点讲述了一个很小但重要的方面，我将其称之为数据整理：构造数据集，以进行分析。

数据集是杂乱还是具有条理，这取决于行、列和表如何与观察值、变量和类型匹配。具有条理的数据具有以下特征：

- 每个变量构成一列。
- 每个观察结果构成一行。
- 每种类型的观察单位构成一个表格。

 [Python 中的整洁度数据](http://www.jeannicholashould.com/tidy-data-in-python.html) 这篇文章的作者是 Jean-Nicholas Hould

### 目测评估

小数据集使用Google Sheets打开并滚动浏览

- [pandas: 显示选项和设置](https://pandas.pydata.org/pandas-docs/stable/options.html)

### 编程评估

- [`head`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.head.html)
- [`tail`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.tail.html)
- [`info`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.info.html)
- [`value_counts`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.value_counts.html)

### 评估记录

- 评估记录不应当包含动词，而是应当只包括对数据存在问题的观察，例如*“非描述性的列标题”*。我们只有在定义清洗操作时，才把动词包括进去。

- 最好将所有评估记录在数据整理模板**评估**部分的底部，即**清洗**标题的正上方。定义清洗操作时，参考这些记录可使数据清洗更简单，还可以避免使你手忙脚乱。

### 更多信息

- [维基百科：转义字符](https://en.wikipedia.org/wiki/Escape_character)
- [Jeff Leek：无整洁度的数据](https://simplystatistics.org/2016/02/17/non-tidy-data/)

---

## 清洗

清洗指为提高质量和整洁度而进行的一个清理动作。

### **提高质量**

- 提高质量并不意味着更改数据，换句话说 - **更改数据就是数据欺诈**。

- 提高质量的例子包括：

  - 数据不正确时进行校正，如将老鼠的体重修改为 0.023 公斤，而不是 230 公斤

  - 移除不相关数据，删除苹果一行，原因是苹果是一种水果，而不是动物

  - 补充缺少的数据，比如，腕龙的脑重数据丢失，应该补充

  - 结合，如下面所示，连接 *more_animals* DataFrame 中缺失的行

### **提高整洁度**

提高整洁度指转换数据集，使每个变量成一列，每个观察结果成一行，每种类型的观察单位构成一个表格。pandas 的特殊功能正好可以帮助我们做到这一点。

###  编程数据清洗过程

1. 定义

2. 编码

   -  [`rename`文档](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.rename.html)

3. 测试

   - [Python 中的 assertion](https://www.tutorialspoint.com/python/assertions_in_python.htm)。

   ```Python
   assert 2+2 == 4
   # 无输出
   assert 2+2 == 5
   # 输出错误
   ```

   

**定义**指以书面形式定义数据清洗计划，其中我们需将评估转变为定义的清洗任务。这个计划也可作为一个指导清单，所以其他人（或我们自己将来）也可以回顾和重现自己的工作。

**编码**指将这些定义转换为代码并执行该代码。

**测试**指测试我们的数据集，通常使用代码，以确保有效完成我们的清洗工作。

### 更多信息

- [Data Carpentry：在 Python 中复制对象与引用对象](https://datacarpentry.org/python-ecology-lesson/03-index-slice-subset/index.html)

---

## 重新评估和迭代

- 很难一次就解决所有问题

- 解决一个问题后会出现新问题

***从小处着手，整个过程多循环几次 即可***

## 整理与 EDA

EDA 的一个 [定义](https://www.epa.gov/caddis-vol4/exploratory-data-analysis)：*即一种分析方法，重点是确定数据中的一般模式，以及确定数据的意外异常值和特征。*

那么，**数据整理以什么**结束，而 **EDA** 以什么开始？

**数据整理** 关于收集正确的数据，评估数据的质量和结构，然后修改数据，确保数据准确无误。但是，将所做的评估转换为清洗操作不会使你的分析，或者说建模更顺利地进行。而我们的目标就是使它们成为可能，即发挥一定作用。

**EDA** 关于探索数据，然后再扩大数据，最大限度发挥我们分析、可视化和建模的潜力。探索数据时，通常会使用简单的可视化来总结数据的主要特征。以便于进行其他操作，例如删除异常值，并根据现有数据创建新的和更具描述性的功能，我们也将这称为 [特征工程](https://en.wikipedia.org/wiki/Feature_engineering)。或者检测和删除异常值，使建模更具适用性。

在实践过程中，整理和 EDA 可以并且经常一起进行

## ETL

你可能已经听说过 [extract-transform-load process](https://en.wikipedia.org/wiki/Extract,_transform,_load)，也称为 **ETL**。 ETL 与数据整理有三个不同点：

1. 用户不同
2. 数据不同
3. 用例不同

Wei Zhang 的篇文章（[数据整理与 ETL 有什么区别？](https://tdwi.org/articles/2017/02/10/data-wrangling-and-etl-differences.aspx)）也解释了这三个不同之处。

# 总结

[数据整理模板链接](https://s3.amazonaws.com/video.udacity-data.com/topher/2018/June/5b2b5723_data-wrangling-template/data-wrangling-template.ipynb)

### 收集

- 根据数据来源及其格式，收集数据的步骤各不相同。
- 高级收集过程：获取数据（从互联网下载文件、抓取网页、查询 API 等），然后将该数据导入编程环境（例如 Jupyter Notebook）。

### 评估

- 评估数据的目的包括：
  - 质量：内容问题。低质量数据也称为脏数据。
  - 整洁度：使分析难易进行的问题。不整洁数据也称为杂乱数据。条理数据的要求包括：
    1. 每个变量成一列。
    2. 每个观察结果成一行。
    3. 每种观察单位构成一个表格。
- 评估类型：
  - 目测评估：使用你喜欢的软件应用程序（Google 表格、Excel、文本编辑器等）观察数据。
  - 编程评估：使用代码来查看数据的特定部分和摘要（例如 pandas 的 `head`、`tail` 和 `info`方法）。

### 清洗

- 清洗类型：
  - 手动（不推荐，除非问题是一次性出现）
  - 编程
- 编程数据清洗过程：
  1. 定义：将评估转换为定义的清洗任务。这些定义也可以作为指令列表，以便其他人（或你自己将来）可以回顾和重现自己的工作。
  2. 代码：将这些定义转换为代码并运行。
  3. 测试：可视上或使用代码练习数据集，确保清洗操作可顺序进行。
- 清洗之前，请务必备份原始数据！

### 重新评估与迭代

- 清洗后，如有必要，请重新评估和迭代任何数据整理步骤。

### 存储（可选）

- 例如，如果将来使用，可将数据存储到文件或数据库中。

