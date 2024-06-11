## pandas数据结构

### DataFrame

有行索引和列索引的二维数组

- 行索引，表明不同行，横向索引，叫 index
- 列索引，表明不同列，纵向索引，叫 columns

#### 生成对象

- pd.DataFrame(ndarray, index=, columns=)
  - index是行索引，可以使用列表里面填字符串
  - columns是列索引，可以使用列表里面填字符串
  - 如果索引想要使用日期，可以使用pd.date_range("20180101", periods=5, freq="B")，periods是天数，freq如果是"B"就是商业模式不包含周六日
  - 默认索引类型是RangeIndex，使用列表里填字符串的话类型是Index，使用pd.date_range()方法的类型是DatetimeIndex

#### 属性

- shape 其中ndarray的形状

- index 行索引列表

- columns 列索引列表

- values 直接获取其中的 ndarray

- T 行列转置，返回新的df对象，不修改原df对象的数组

#### 方法

下面两个方法常用于观看一下df对象的格式，但是又不想全部打印的情况

- head() 开头5行，可数字指定几行
- tail() 最后5行，可数字指定几行

#### 索引设置

除了实例化的时候设置索引，也可以实例化后重新设置索引。这些除了通过.index属性设置外，其余操作不会修改原df，而是返回新df

- df.reset_index()
  - 设置一个新的默认行索引，原来的行索引变成普通的一列
  - 如果在参数里添加drop=True，则设置新行索引后原来索引那一列会被删除掉，默认是False

- df.index = 新的行索引
  - 通过index属性重新赋值行索引
  - 注意不能使用df.index[num] = ""这样来单独修改行索引中的某个值，只能对整个index属性重新赋值

- df.set_index("列索引名称")
  - 通过set_index方法来对index属性重新赋值行索引，将某一列设置为行索引
  - 默认重新赋值index后，用来赋值索引的那一列会被销毁，可通过参数drop=False以保留
  - 想拿多个列设置行索引的话，可将多个列的名称放入列表中，这样子index属性的类型就变成了MultiIndex

```python
# 修改行索引值

# 重设索引
df1.reset_index(drop=True) # drop=True把之前的索引删除，默认是False

# data.index[num] = "" 不能单独修改索引
# 使用index属性重新设置行索引
stock = ["股票{}".format(i) for i in range(10)]
df1.index = stock


# 使用set_index()重新设置行索引
df2 = pd.DataFrame({'month': [1, 4, 7, 10],
                    'year': [2012, 2014, 2013, 2014],
                    'sale':[55, 40, 84, 31]})
# 以月份设置新的索引
df2.set_index("month", drop=True)
# 设置多个索引，以年和月份
df2.set_index(["year", "month"], drop=True)
```

### MultiIndex和Panel

#### MultiIndex

- multiindex是df对象的index属性的一种，它有两个属性，一个是levels，一个是names
- levels就是索引的那几列的值
- names是索引的那几列的名称

#### Panel

存储 3 维数组的 Panel 结构，创建方法pd.Panel(ndarray, items=, major_axis=, minor_axis=, copy=False,dtype=None)

- items - axis 0，每个item对应于内部包含的数据帧(DataFrame)
- major_axis - axis 1，它是每个数据帧(DataFrame)的索引(行)
- minor_axis - axis 2，它是每个数据帧(DataFrame)的列

```python
p = pd.Panel(np.arange(24).reshape(4,3,2),
                 items=list('ABCD'),
                 major_axis=pd.date_range('20130101', periods=3),
                 minor_axis=['first', 'second'])
# 取出里面的item，即一个df对象
p["A"]
p.major_xs("2013-01-01")
p.minor_xs("first")
```

注：Pandas 从版本 0.20.0 开始弃用

### Series

带索引的一维数组，通过df转置列索引变成行索引，然后选取某一列获得

#### 属性

- index属性，行索引(df对象的列索引)
- values属性，一维数组

#### 创建对象

- pd.Series(ndarray, index=)
  - 通过一维数组创建，index用于指定索引
  - 不指定索引的话会和df对象一样使用默认的RangeIndex
- pd.Series({"": , "": ,})
  - 通过字典创建，字典的key会变成索引