## numpy数据类型

ndarray是numpy的数据类型，全称为n维数组，要求里面类型统一

- ndarray速度比原生list快
  - ndarray因为内部类型简单且相同，内存存储连续直接放内容。list支持内部类型不同，存储使用额外引用指向内容
  - ndarray可以并行化操作(向量化操作)
  - numpy底层使用c语言编写，且解除了gni(全局解释器锁)，速度不受python解释器限制
- ndarray可以进行批量操作

## ndarray

### ndarray的属性

|     属性名字     |                属性解释                 |
| :--------------: | :-------------------------------------: |
|  ndarray.shape   | 元组，一维是(c, )，二维是(r, c)，r行c列 |
|   ndarray.ndim   |        数组维数，shape内数字个数        |
|   ndarray.size   |    数组中的元素数量，shape内数字相乘    |
|  ndarray.dtype   |        numpy.xxx，数组元素的类型        |
| ndarray.itemsize |        一个数组元素的长度(字节)         |

- 如果创建ndarray时没有指定类型，整数默认类型为int64，浮点数默认float64
- 创建的时候通过参数dtype="uint8"/numpy.uint8这样的格式指定类型，常用类型有int32/64、unit8(0 - 255)、float32/64

## 基本操作

主要是numpy.方法名()和ndarray.方法名()，以下numpy简写np

### 生成数组（ndarray）

#### 生成0数组或1数组

- np.zeros()
- np.ones()

```python
# shape=可以省略，也可以使用列表如[2, 3]表示两行三列形状
zeros = np.zeros(shape=(2, 3), dtype="float32")  # 生成0数组，默认是float64类型
ones = np.ones(shape=(2, 3), dtype="uint8")  # 生成1数组，默认是float64类型
```

#### 从现有数组或py列表中生成

- np.array()
- np.copy()
- np.asarray()

```python
score = np.array([[80, 89, 86, 67, 79],
                  [78, 97, 89, 67, 81],
                  [90, 94, 78, 67, 74],
                  [91, 91, 90, 67, 69],
                  [76, 87, 75, 67, 86],
                  [70, 79, 84, 67, 84],
                  [94, 92, 93, 67, 64],
                  [86, 85, 83, 67, 80]])  # 依靠py列表生成

data1 = np.array(score)  # 深拷贝，和原数组无关系
data2 = np.copy(score)  # 深拷贝
data3 = np.asarray(score)  # 浅拷贝，被原数组影响且该数组改变也会影响原数组
```

#### 生成固定范围数组

- np.linspace()
- np.arange()

```python
data5 = np.linspace(0, 10, 1000)  # 等距的生成1000个[0, 10]左闭右闭之间的数放入一维数组中返回，data5.shape = (1000,)，data5.dtype="float64"
data6 = np.arange(0, 10, 2)  # 与range(0, 10, 2)效果一样，左闭右开，步长为二。生成的是一维数组，data6.shape = (5,)，data5.dtype="int64"，步长是浮点数的话就是float64
```

#### 生成随机数组

##### 均匀分布

- np.random.uniform()

```python
# 生成均匀分布的一组数[low,high)，dtype="float64"
data6 = np.random.uniform(low=-1, high=1, size=1000000)
# 画直方图验证
import matplotlib.pyplot as plt
plt.figure(figsize=(20, 8), dpi=80)
plt.hist(data6, 1000)
plt.show()
```

##### 正态分布

- np.random.normal()

```python
# 生成正态分布的一组数，dtype="float64"，均值为0标准差为1的是标准正态分布
# loc：均值，大部分数据都向均值靠拢
# scale：标准差，越大图像越平，即越大靠向两边的数就越多，稳定性弱/波动大
# size：形状，如果传数字就是一维，可以传元组生成多个正态分布数组，
data7 = np.random.normal(loc=1.75, scale=0.1, size=1000000)
# 画直方图验证
plt.figure(figsize=(20, 8), dpi=80)
plt.hist(data7, 1000)
plt.show()
```

### 改变数组形状

- ndarray.reshape()
- ndarray.resize()
- ndarray.T

```python
stock_change.reshape(10, 8)  # 不改变原数组，将原数组数据按形状顺序放入新数组返回，reshape的后的元素个数必须和原来一直，否则报ValueError
stock_change.resize(10, 8)  # 修改原数组，返回值为None，修改原则和reshape()一样
stock_change.T  # 转置，不修改原数组，返回值是转置后的数组

ndarray.flatten()  #不修改原数组，返回一维数组，相当于reshape为一维数组
```

### 修改类型

- ndarray.astype(np.xxx)  

- ndarray.tostring/tobytes()  

```python
print(stock_change.astype(np.int32))  # 不修改原数组，可以是任意类型如int32/64、uintt8、float32/64，注意如果是浮点数转化为整数会舍弃掉小数
print(stock_change)  # 不改变原数组，返回新的数组
stock_change.tostring/tobytes()  # 不修改原数组，返回byte类型方便序列化到本地。tostring过时了
```

### 去重

- set()  可以去重一维数组，结果是set
- np.unique(ndarray)  不修改原数组，返回去重后的一维数组

### 索引和切片

- 索引使用一个[]符号进行，一维数组使用[num], 二维使用[row, col], 三维使用[h, r, c]
- 可以使用py的切片操作，切片出来的对象仍然是ndarray，维度根据切片时候索引进行到的维度，每有一个纯数字就降低一个维度

```python
stock_change = np.random.normal(loc=0, scale=1, size=(8, 10))
print(stock_change)
print(stock_change[2, 3])  # 索引第二行第三列数据
stock_change[2, 3] = 100  # 修改第二行第三列数据为100，但是默认是float64类型，会转换为1.00000000e+02

print(stock_change[0, :3])  # 取到一维数组后进行切片，如果直接stock_change[:3]取到的是二维数组
print(type(stock_change[0, :3]))  # np.ndarray
```

## 运算

### 逻辑运算

#### 关系运算

- ndarray 关系运算符 num 

  - 返回布尔(逻辑)数组，关系运算符都可以，如>、>=、<、<=、==、!=

  - 想要&条件时可以使用np.logical_and(ndarray > num1, adarray < num2, ...)
  - 想要|条件时可以使用np.logical_or(ndarray > num1, adarray < num2, ...)

- ndarray[逻辑数组] 

  - 布尔索引，逻辑数组的形状必须和ndarray一样
  - 将符合条件的数(逻辑数组元素为True)索引出来，返回结果是一维数组
  - 可以像下标索引那样直接操作赋值，修改原数组中符合条件的数

#### 判断函数

- np.all(逻辑数组)  返回结果是单个布尔值，逻辑数组如果里面有一个数为假返回False，相当于&

- np.any(逻辑数组)  返回结果是单个布尔值，逻辑数组如果里面有一个数为真返回True，相当于|

#### 三元运算符

- np.where(逻辑数组, num1, num2)
  - 将逻辑数组里面的值重替换返回，不修改原逻辑数组，逻辑数组元素为True替换为num1，否则替换为num2

以上所有需要逻辑数组的，其实不是逻辑数组也行，原理是对里面的每一个元素都进行if判断，数字的话不是0就是True

### 统计运算

#### 统计指标

统计指标有max、min、mean(均值)、median(中位数)、var(方差)、std(标准差)

#### 统计函数

- 可以使用ndarray.方法名()或者np.函数名(ndarray)来求统计指标
  - 默认是对所有元素进行统计，可以传参axis=0表示按列统计，axis=1表示按行统计。三维数组则加1

- 在方法名前加arg求的就不是数而是数的下标

### 数组运算

#### 数组与数运算

- 相当于数组里每个数都和运算一遍，不修改原数组，返回新的数组
  - 可以使用+、-、*、/、//、**、&、|、^

#### 数组与数组运算

数组间运算需要比较两个数组的shape

- 从后到前比较，需要两个数相等或者其中一个数为1
- 运算后的数组维度，原两个数组的shape从前到后取最大的那个数

如果数量为1那就所有数都对那个数运算，如果一样那就一对一对应

### 矩阵运算

矩阵是二维的，在numpy中，有以下两种类型可以表示矩阵

- 二维的ndarray
- np.matrix类型
  - 使用np.mat()方法获得，可传入list或ndarray

#### 矩阵乘法

矩阵乘法和线代里乘法一样

- 如果是ndarray类型，使用np.matmul()或np.dot()方法
  - 按顺序放入两个ndarray，左边的列等于右边的行
  - 返回值是ndarray
- 如果是np.matrix类型直接使用*号
  - 执行是左边的矩阵乘右边的矩阵
  - 返回值是np.matrix

ndarray也可以直接使用@符号直接进行矩阵乘法，matrix类型也可以使用np.matmul()或np.dot()方法

## 合并和分割

### 合并

合并有垂直拼接和水平拼接

- 垂直拼接np.vstack((ndarray1, ndarray2))
  - 列数必须一样
- 水平拼接np.hstack((ndarray1, ndarray2))
  - 行数必须一样

- np.concatenate((ndarray1, ndarray2), axis=num)
  - axis=0为垂直拼接
  - axis=1为水平拼接 

### 分割

使用np.split(ndarray, num)，等距分割成num份

使用np.split(ndarray, [index1, index2, ...])，按下标分割成n份

## IO操作

### 读取

- np.genfromtxt("test.csv", delimiter=",")
  - 读不出字符串，被标记为nan
  - 缺失值也会被标记为nan

### 缺失值处理

- 当数据充足时，可以将含有缺失值的那一行剔除掉
- 数据不充足时，可以进行替换/插补，使用那一列的平均值或中位数