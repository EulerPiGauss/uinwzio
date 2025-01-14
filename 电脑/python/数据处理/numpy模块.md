<nav id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#orgheadline43">1. numpy基础</a>
<ul>
<li><a href="#orgheadline1">1.1. 安装</a></li>
<li><a href="#orgheadline2">1.2. 引用惯例</a></li>
<li><a href="#orgheadline3">1.3. numpy ndarray对象和python的列表的区别</a></li>
<li><a href="#orgheadline32">1.4. ndarray对象</a>
<ul>
<li><a href="#orgheadline6">1.4.1. dtype清单</a>
<ul>
<li><a href="#orgheadline4">1.4.1.1. ndarray的dtype变换</a></li>
<li><a href="#orgheadline5">1.4.1.2. dtype对象的从属关系</a></li>
</ul>
</li>
<li><a href="#orgheadline7">1.4.2. shape属性</a></li>
<li><a href="#orgheadline19">1.4.3. 创建一个ndarray对象</a>
<ul>
<li><a href="#orgheadline8">1.4.3.1. 从python数据结构中创建</a></li>
<li><a href="#orgheadline9">1.4.3.2. arrange函数</a></li>
<li><a href="#orgheadline10">1.4.3.3. linspace函数</a></li>
<li><a href="#orgheadline11">1.4.3.4. zeros函数</a></li>
<li><a href="#orgheadline12">1.4.3.5. ones函数</a></li>
<li><a href="#orgheadline13">1.4.3.6. empty函数</a></li>
<li><a href="#orgheadline14">1.4.3.7. indices函数</a></li>
<li><a href="#orgheadline15">1.4.3.8. eye函数</a></li>
<li><a href="#orgheadline16">1.4.3.9. 随机创建</a></li>
<li><a href="#orgheadline17">1.4.3.10. 从文本创建</a></li>
<li><a href="#orgheadline18">1.4.3.11. 从输入字节流创建</a></li>
</ul>
</li>
<li><a href="#orgheadline23">1.4.4. 索引值</a>
<ul>
<li><a href="#orgheadline21">1.4.4.1. 索引多个值或说view</a>
<ul>
<li><a href="#orgheadline20">1.4.4.1.1. copy方法</a></li>
</ul>
</li>
<li><a href="#orgheadline22">1.4.4.2. 布尔值索引</a></li>
</ul>
</li>
<li><a href="#orgheadline24">1.4.5. ndarray对象转置</a></li>
<li><a href="#orgheadline25">1.4.6. 基本的运算</a></li>
<li><a href="#orgheadline26">1.4.7. flatten方法</a></li>
<li><a href="#orgheadline27">1.4.8. sort方法</a></li>
<li><a href="#orgheadline29">1.4.9. 通用的一些方法</a>
<ul>
<li><a href="#orgheadline28">1.4.9.1. 多个item的聚合</a></li>
</ul>
</li>
<li><a href="#orgheadline30">1.4.10. bool值ndarray对象额外的方法</a></li>
<li><a href="#orgheadline31">1.4.11. 子类化ndarray对象</a></li>
</ul>
</li>
<li><a href="#orgheadline36">1.5. 通用的一些函数</a>
<ul>
<li><a href="#orgheadline33">1.5.1. 一元通用函数</a></li>
<li><a href="#orgheadline34">1.5.2. 二元通用函数</a></li>
<li><a href="#orgheadline35">1.5.3. 集合类似操作的函数</a></li>
</ul>
</li>
<li><a href="#orgheadline41">1.6. 矩阵对象</a>
<ul>
<li><a href="#orgheadline37">1.6.1. matrix函数</a></li>
<li><a href="#orgheadline38">1.6.2. 矩阵转置</a></li>
<li><a href="#orgheadline39">1.6.3. 行矢量和列矢量</a></li>
<li><a href="#orgheadline40">1.6.4. 矩阵的点乘</a></li>
</ul>
</li>
<li><a href="#orgheadline42">1.7. 随机数生成支持</a></li>
</ul>
</li>
<li><a href="#orgheadline44">2. 参考资料</a></li>
</ul>
</div>
</nav>


# numpy基础<a id="orgheadline43"></a>

## 安装<a id="orgheadline1"></a>

就简单用pip安装之，如果你是从源码安装，请确认安装了 `cython` 模块。

## 引用惯例<a id="orgheadline2"></a>

    import numpy as np

## numpy ndarray对象和python的列表的区别<a id="orgheadline3"></a>

1.  numpy array内部的item是固定内存size的，改变size将会重新创建一个array。
2.  numpy array内部的item是相同的data type的，因此是固定内存size的。
3.  numpy的array有助于大型数据的高级数学运算或其他操作，比python的序列那些执行会更有效率。
4.  一系列的科学和数学计算python模块都是基于numpy的array的，当然他们支持python的序列类型输入，但都是转变成为numpy的array之后再进行相关计算的，然后他们的输出也通常是numpy的array。

## ndarray对象<a id="orgheadline32"></a>

numpy模块中很核心的一个概念就是ndarray对象。ndarray对象按照numpy官方手册的绘图是这样一个数据结构：

![img](images/ndarray.png "ndarray")

ndarray有一个头header来控制所有接下来存储的数据类型(dtype)，然后存储的数据则必然都是相同的数据类型，这是一个不同于列表的限定条件，这样约定将大大提高数据处理的效率。

你可以利用array函数简单将一个列表变成ndarray对象：

    >>> x = np.array([1,2,3,4,5])
    >>> x
    array([1, 2, 3, 4, 5])
    >>> type(x)
    <class 'numpy.ndarray'>
    >>> x.dtype
    dtype('int32')

在上面的例子中我们看到，每一个ndarray对象都有一个属性( **dtype** )，其存储的就是前面讲的ndarray对象后面一连串数据的数据类型，比如这里的数据类型是“int32”。

### dtype清单<a id="orgheadline6"></a>

这个基本上讨论numpy的资料都会把这个清单列出来，这里也列出来吧。

-   **bool\_:** True or False
-   **int\_:** 相当于C语言的long，一般是int32或int64。整数型其内细分:
    -   **intc:** 等于C语言的int，int32或int64
    -   **intp:** 整数用于索引，和C语言的ssize\_t相同，一般是int32或int64。
    -   **int8:** Byte（-128 ~ 127）
    -   **int16:** Integer（-32769 ~ 32767）
    -   **int32:** Integer
    -   **int64:** Integer
    -   **uint8:** Unsigned Integer（0 ~ 255）
    -   **uint16:** Unsigned Integer（0 ~ 65535）
    -   **uint32:** Unsigned Integer
    -   **uint64:** Unsigned Integer
-   **float\_:** 具体为float64。浮点型细分为:
    -   **float16:** 半精度浮点型
    -   **float32:** 单精度浮点型
    -   **float64:** 双精度浮点型
-   **complex\_:** 就是complex128。 复数型细分为:
    -   **complex64:** 复数型，由32位浮点型组成
    -   **complex128:** 复数形，由64位浮点型组成

具体使用声明如下:

    >>> t = np.array([1,2,3],dtype='int32')
    >>> type(t)
    <class 'numpy.ndarray'>
    >>> t.dtype
    dtype('int32')
    >>>

在实际使用的时候，dtype若指定为int，则实际就是对应的 `np.int_`

    >>> t = np.array([1,2,3],dtype='int')
    >>> t.dtype
    dtype('int64')

类似的 `float` 对应 `np.float_` ; `bool` 对应 `np.bool_` ; `complex` 对应 `np.complex_` 。

#### ndarray的dtype变换<a id="orgheadline4"></a>

在改变某个ndarray对象的dtype的时候，原ndarray对象实际上被删除了，等于重新创建了一个ndarray对象。可以通过上面的类型声明来直接进行转换，如:

    >>> t = np.array([1,2,3],dtype='int8')
    >>> t.dtype
    dtype('int8')
    >>> new_t = np.int32(t)
    >>> new_t.dtype
    dtype('int32')

还可以通过调用ndarray的 `astype` 方法来实现。注意这个方法是 **非破坏型** 方法，具体使用如下面例子所示：

    >>> t = np.array([1,2,3],dtype='int8')
    >>> t.astype('int32')
    array([1, 2, 3], dtype=int32)
    >>> t
    array([1, 2, 3], dtype=int8)

#### dtype对象的从属关系<a id="orgheadline5"></a>

用 `np.issubdtype` 函数来判断某个ndarray的dtype对象是不是整型的子集。

    >>> t
    array([1, 2, 3], dtype=int8)
    >>> t.dtype
    dtype('int8')
    >>> np.issubdtype(t.dtype,'int')
    True
    >>> np.issubdtype(t.dtype,'float')
    False

### shape属性<a id="orgheadline7"></a>

此外，每一个ndarray对象都有 `shape` 属性，用于控制后面跟着的这些数据的维度。请看下面的例子：

    >>> x
    array([1, 2, 3, 4, 5, 6])
    >>> x.shape
    (6,)
    >>> x.shape = (2,3)
    >>> x
    array([[1, 2, 3],
           [4, 5, 6]])

shape 属性用来控制对于后面数据维度的理解，一个数字表示一维，二个数字表示二维几行几列（也就是数学中我们常见的概念矩阵），三个数字表示三维等。这里直接修改ndarray对象的shape属性将直接影响程序对于该对象数据的理解，此外更常用的是用 `reshape` 方法，其并不原地修改某个ndarray对象的shape，而是返回一个被修改shape属性的新的ndarray对象。

### 创建一个ndarray对象<a id="orgheadline19"></a>

#### 从python数据结构中创建<a id="orgheadline8"></a>

这个就是前面接触过的 `np.array` 函数，用来接受一个python list 或 tuple ，从而返回一个ndarray对象。

    >>> x = np.array([[1+2j,2+3j],[3+4j,4+5j]])
    >>> x
    array([[ 1.+2.j,  2.+3.j],
           [ 3.+4.j,  4.+5.j]])
    >>> x.dtype
    dtype('complex128')
    >>>

#### arrange函数<a id="orgheadline9"></a>

arange(start,end,step)  参数类似range函数。生成一个数据递增（减）的ndarray对象：

    >>> x = np.arange(5)
    >>> x
    array([0, 1, 2, 3, 4])
    >>> x = np.arange(1,10,0.5)
    >>> type(x)
    <class 'numpy.ndarray'>
    >>> x
    array([ 1. ,  1.5,  2. ,  2.5,  3. ,  3.5,  4. ,  4.5,  5. ,  5.5,  6. ,
            6.5,  7. ,  7.5,  8. ,  8.5,  9. ,  9.5])

其实一维的，但通过reshape操作可以生成二维的ndarray对象，其可以接受 `dtype` 对象来控制dtype属性。

#### linspace函数<a id="orgheadline10"></a>

linspace函数可以看作上面 arange函数的补充，arange函数虽然指定了start和stop，最后的数值是不被包含的，然后具体生成了多少个item是不易知的，而linspace可以接受这样三个参数: `start end number` ，其中start和end一定是在ndarray中包含的，然后number给定了具体生成了多少个item。

    >>> np.linspace(1,10,6)
    array([  1. ,   2.8,   4.6,   6.4,   8.2,  10. ])

结束元素包不包含倒不是很重要，关键是某些情况下你需要控制具体生成了多少个item，那么就需要使用 `linspace` 函数。

#### zeros函数<a id="orgheadline11"></a>

zeros函数用于快速创建一个ndarray对象，其内数据都填充的是 `0.` ，默认dtype是 `float64` 。其接受的一个参数你可以简单看作就是shape属性参数，如下所示：

    >>> np.zeros((10,))
    array([ 0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.])
    >>> np.zeros(10)
    array([ 0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.])
    >>> np.zeros((5,5))
    array([[ 0.,  0.,  0.,  0.,  0.],
           [ 0.,  0.,  0.,  0.,  0.],
           [ 0.,  0.,  0.,  0.,  0.],
           [ 0.,  0.,  0.,  0.,  0.],
           [ 0.,  0.,  0.,  0.,  0.]])

#### ones函数<a id="orgheadline12"></a>

ones函数类似于zeros函数，不同的是填充的数据是1。就不做例子演示了。

#### empty函数<a id="orgheadline13"></a>

empty函数和前面谈论的 zeros ones 函数类似，除了各个item都是原内存的随机数值，并不做任何修改。

    >>> np.empty((2,3))
    array([[  0.00000000e+000,   4.99297208e-317,   4.94026911e-317],
           [  6.94094003e-310,   1.03878549e-013,   0.00000000e+000]])
    >>>

#### indices函数<a id="orgheadline14"></a>

#### eye函数<a id="orgheadline15"></a>

eye函数生成的就是所谓的对角行列式的东西（numpy有matrix矩阵对象，其是作为ndarray对象的子类实现。为了和后面的matrix对象区分，那么ndarray对象一定要对应到某个数学概念的话，大概就是行列式了吧。）

    >>> np.eye(3)
    array([[ 1.,  0.,  0.],
           [ 0.,  1.,  0.],
           [ 0.,  0.,  1.]])
    >>> np.eye(4,k=1)
    array([[ 0.,  1.,  0.,  0.],
           [ 0.,  0.,  1.,  0.],
           [ 0.,  0.,  0.,  1.],
           [ 0.,  0.,  0.,  0.]])

#### 随机创建<a id="orgheadline16"></a>

#### 从文本创建<a id="orgheadline17"></a>

np.save and np.load

或者存储多个ndarray对象和加载多个ndarray对象

    np.savez('array_archive.npz', a=arr, b=arr)
    arch = np.load('array_archive.npz')
    arch['a']

#### 从输入字节流创建<a id="orgheadline18"></a>

### 索引值<a id="orgheadline23"></a>

ndarray对于值的索引操作和python中列表索引值的操作非常相似，即方括号语法索引 `[index]` :

    >>> x = np.array([[1,2,3],[4,5,6],[7,8,9]])
    >>> x
    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])
    >>> x[0]
    array([1, 2, 3])
    >>> x[0][0]
    1
    >>> y[1][5]
    5

此外你还可以用这种语法:

    >>> x
    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])
    >>> x[0,0]
    1
    >>> x[1,1]
    5
    >>>

通过上面描述的索引值语法可以直接修改该ndarray对象的这个元素的值。此外numpy还提供了另外一种表示语法： `[a,b]` ，对于ndarray对象其和 `[a][b]` 的意思是一样的，推荐ndarray对象就用python的原来表示方法，意思也很明确，是a维的第b个元素。但是矩阵 *不* 支持 `[a][b]` 这种索引语法，而只支持 `[a,b]` 这种表示语法，推荐对于矩阵都用带逗号的这种索引方法，表示矩阵的a行b列。

    >>> A = np.matrix([[1,2,3],[4,5,6],[7,8,9]])
    >>> A[0]
    matrix([[1, 2, 3]])
    >>> A[0][0]#并没有索引下去
    matrix([[1, 2, 3]])
    >>> A[0,0]
    1

#### 索引多个值或说view<a id="orgheadline21"></a>

同样ndarray对象也有在上面谈及的索引规则下 `[start:end:step]` :

    >>> x = np.array([[1,2,3],[4,5,6],[7,8,9]])
    >>> x
    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])
    >>> x[::-1]
    array([[7, 8, 9],
           [4, 5, 6],
           [1, 2, 3]])
    >>> y = np.arange(10)
    >>> y
    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
    >>> y[0::2]
    array([0, 2, 4, 6, 8])

支持索引多个值，但注意上面不是说切片，而是 **view** 视图。因为python的list如果你索引多个值，切片了，则等于制造了一个新的列表，如:

    >>> lst = [1,2,3,4,5]
    >>> lst[0:2]
    [1, 2]
    >>> x = lst[0:2]
    >>> x[0] = 12
    >>> x
    [12, 2]
    >>> lst
    [1, 2, 3, 4, 5]

在调用 `lst[0:2]` 时，python程序是制造一个新的子列表，然后赋值给x，但是我们看ndarray对象不是这样的:

    >>> array = np.array([1,2,3,4,5])
    >>> array
    array([1, 2, 3, 4, 5])
    >>> x = array[:2]
    >>> x
    array([1, 2])
    >>> x[0] = 12
    >>> x
    array([12,  2])
    >>> array
    array([12,  2,  3,  4,  5])

这就是ndarray对象索引多个值称之为 **视图** 的原因，其返回的还是指向原处的那个片段！

最后对于索引多个值的视图赋值操作，是所有元素都赋值为那个值:

    >>> x[:] = 99
    >>> array
    array([99, 99,  3,  4,  5])

##### copy方法<a id="orgheadline20"></a>

如果你希望达到原python的那种索引多个值的效果而不影响原ndarray对象，你可以调用ndarrary对象的 `copy` 方法:

    array[:2].copy()

#### 布尔值索引<a id="orgheadline22"></a>

布尔值索引是基于 ndarray对象进行布尔值判断操作，如 `== > <` 等等之类的时候，将输出一个原维度的bool值ndarray对象。然后将这个ndarray对象送入array的索引输入框中，其将返回bool值为True的那些值。

    >>> array
    array([0, 0, 3, 4, 5])
    >>> array == 0
    array([ True,  True, False, False, False], dtype=bool)
    >>> array[array == 0]
    array([0, 0])
    >>> array[array == 0] = 99
    >>> array
    array([99, 99,  3,  4,  5])

布尔值索引返回的也是 **视图** ，对齐操作将改变原ndarray对象。

你还可以用 `&` 和 `|` 来形成组合逻辑，但不能使用 and 和 or 。

一大用法就是利用某个item各个属性的映射关系，利用其他属性来过滤另外某个data:

    >>> data = np.random.randn(7,3)
    >>> data
    array([[-0.82117767,  1.02481308,  0.50908019],
           [ 0.79851282,  0.37692996, -1.0129145 ],
           [-1.30120201,  1.71270027,  0.2113716 ],
           [-1.33386207,  0.02978504, -0.58061781],
           [ 0.72466458,  1.94170572,  2.09521622],
           [-1.24241997, -1.20557331, -0.66292731],
           [-0.66145326,  0.28330579,  0.2803069 ]])
    >>> names = np.array(['a','b','c','a','b','d','a'])
    >>> data[names == 'a']
    array([[-0.82117767,  1.02481308,  0.50908019],
           [-1.33386207,  0.02978504, -0.58061781],
           [-0.66145326,  0.28330579,  0.2803069 ]])

这里将索引的是每一行，其行对应的name是'a'的值。

### ndarray对象转置<a id="orgheadline24"></a>

就是调用ndarray对象的 `T` 属性，这更接近于矩阵中的转置操作（但是对于一维ndarray并没有任何改变）。而之前提及的 `data[::-1]` 这么使用，只是把行翻转了一下，对于一维倒是整个array都翻转了。

    >>> data = np.random.randn(4,3)
    >>> data
    array([[ 0.53700477, -1.30139712,  1.12184318],
           [-0.91918847,  1.52850268,  0.73218978],
           [-1.14840704, -0.0413753 ,  0.52820585],
           [ 1.84307255,  0.21356674,  0.23331023]])
    >>> data.T
    array([[ 0.53700477, -0.91918847, -1.14840704,  1.84307255],
           [-1.30139712,  1.52850268, -0.0413753 ,  0.21356674],
           [ 1.12184318,  0.73218978,  0.52820585,  0.23331023]])
    >>> data[::-1]
    array([[ 1.84307255,  0.21356674,  0.23331023],
           [-1.14840704, -0.0413753 ,  0.52820585],
           [-0.91918847,  1.52850268,  0.73218978],
           [ 0.53700477, -1.30139712,  1.12184318]])

### 基本的运算<a id="orgheadline25"></a>

两个ndarray对象之间进行基本的数学运算，如果两个ndarray维度是相同的，则称之为 `vectorization` ，矢量化操作。大致意思就是 加减乘除幂 具体操作都是 <span class="underline">对应的元素和对应的元素进行加减乘除幂操作</span> :

    >>> x = np.array([[4,0,5],[-1,3,2]])
    >>> x
    array([[ 4,  0,  5],
           [-1,  3,  2]])
    >>> y = np.array([[1,1,1],[3,5,7]])
    >>> y
    array([[1, 1, 1],
           [3, 5, 7]])
    >>> x + y
    array([[5, 1, 6],
           [2, 8, 9]])
    >>> x - y
    array([[ 3, -1,  4],
           [-4, -2, -5]])
    >>> x * 2
    array([[ 8,  0, 10],
           [-2,  6,  4]])
    >>> x ** 2
    array([[16,  0, 25],
           [ 1,  9,  4]])

如果两个ndarray对象的维度（多维的情况不讨论了吧），如果 列维数目相同，则似乎也是可以的，但应该不推荐这么使用。而如果列维数目不同，则会抛出 `ValueError` 。

    >>> z = np.array([1,2,3])
    >>> x+z
    array([[5, 2, 8],
           [0, 5, 5]])

然后我们知道矩阵里面还有其他一些算法，如点乘之类的，这个后面再讨论吧。

### flatten方法<a id="orgheadline26"></a>

flatten，拉平。flatten是ndarray对象（包括矩阵）的一个方法，可将其变为一维形式， *非破坏型* 方法。

这里将flatten方法归到矩阵这里是因为多维数组必须各个维度所含元素数目相等（也就是必须要有类似矩阵的空间矩形排布感）才有意义。然后矩阵返回的是行矢量形式。

    >>> x = np.array([[1,2,3],[4,5,6],[7,8,9]])
    >>> x
    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])
    >>> x.flatten()
    array([1, 2, 3, 4, 5, 6, 7, 8, 9])
    >>> x
    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])
    >>> y = np.array([[1,2,3],[4,5,6,7]])
    >>> y
    array([[1, 2, 3], [4, 5, 6, 7]], dtype=object)
    >>> y.flatten()
    array([[1, 2, 3], [4, 5, 6, 7]], dtype=object)
    >>> z
    matrix([[1, 2, 3],
            [4, 5, 6]])
    >>> z.flatten()
    matrix([[1, 2, 3, 4, 5, 6]])

### sort方法<a id="orgheadline27"></a>

sort方法虽然可以作用多维，但似乎对一维更显的有意义些，其实一个 *破坏型* 方法。

如下所示，注意看，每一行并没有变动，只在行内一维情况下排序。

    >>> data
    array([[ 0.68518059,  1.05271585,  1.00174264],
           [-1.44506879,  1.45532422,  1.30856608],
           [ 0.1121552 , -3.04487041, -0.03301996]])
    >>> data.sort()
    >>> data
    array([[ 0.68518059,  1.00174264,  1.05271585],
           [-1.44506879,  1.30856608,  1.45532422],
           [-3.04487041, -0.03301996,  0.1121552 ]])

### 通用的一些方法<a id="orgheadline29"></a>

这些方法不仅适用于一维ndarray对象也适用矩阵对象等。

#### 多个item的聚合<a id="orgheadline28"></a>

-   sum
-   mean
-   std
-   var
-   min
-   max
-   argmin
-   argmax
-   cumsum
-   cumprod

### bool值ndarray对象额外的方法<a id="orgheadline30"></a>

-   all
-   any

### 子类化ndarray对象<a id="orgheadline31"></a>

## 通用的一些函数<a id="orgheadline36"></a>

这些函数都是针对ndarray对象或矩阵对象等的所有item的，这些函数具有通用性。

### 一元通用函数<a id="orgheadline33"></a>

-   **np.sqrt:** 

    >>> data = np.arange(10)
    >>> data
    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
    >>> np.sqrt(data)
    array([ 0.        ,  1.        ,  1.41421356,  1.73205081,  2.        ,
            2.23606798,  2.44948974,  2.64575131,  2.82842712,  3.        ])

-   **np.exp:** \(e^{item}\)
-   **np.abs:** abs(item)
-   **np.square:** \(item^2\)

log
log2
log10
sign
floor
rint
modf
isnan
isfinite
isinf
cos
sin
tan
tanh
cosh
sinh
arccos
arcsin
arctan
arcsinh
arccosh
arctanh

### 二元通用函数<a id="orgheadline34"></a>

两个ndarray对象item对item的操作

add
substract
multiply
divide
power
maximum
minimum
mod
copysign
greater
less
less\_equal
not\_equal
logical\_and
logical\_or
logical\_xor

### 集合类似操作的函数<a id="orgheadline35"></a>

unique(x) 
intersect1d(x, y)
union1d(x, y) 
in1d(x, y) 
setdiff1d(x, y)
setxor1d(x, y)

## 矩阵对象<a id="orgheadline41"></a>

矩阵对象是ndarray对象的子类，也就是说ndarray对象的一些属性和方法它都是可以使用的。行矢量和列矢量是属于矩阵中的特殊情况。矩阵这个概念在以后的数学运算中较为重要，然后对于一些概念，比如转置啊，点乘啊等，总之和矩阵的数学运算相关的，虽然ndarray对象也可以做，但推荐将其变成矩阵（matrix）对象之后再处理，这样容易理清概念。

### matrix函数<a id="orgheadline37"></a>

用numpy的matrix函数可以创建一个矩阵对象:

    >>> data = np.random.randn(3,3)
    >>> data
    array([[-0.79589206, -0.97535141,  1.05750453],
           [ 0.05051448,  0.19753523,  0.99618112],
           [ 2.09805081, -0.33623748,  0.26033154]])
    >>> x = np.matrix(data)
    >>> type(x)
    <class 'numpy.matrixlib.defmatrix.matrix'>
    >>> x
    matrix([[-0.79589206, -0.97535141,  1.05750453],
            [ 0.05051448,  0.19753523,  0.99618112],
            [ 2.09805081, -0.33623748,  0.26033154]])

### 矩阵转置<a id="orgheadline38"></a>

`transpose` 方法，将矩阵转置过来。只返回结果， *非破坏型* 方法。

    >>> x = np.matrix([[1,2,3],[4,5,6],[7,8,9]])
    >>> x
    matrix([[1, 2, 3],
            [4, 5, 6],
            [7, 8, 9]])
    >>> x.transpose()
    matrix([[1, 4, 7],
            [2, 5, 8],
            [3, 6, 9]])

### 行矢量和列矢量<a id="orgheadline39"></a>

行矢量和列矢量是矩阵的特殊情况，需要用matrix函数创建之。行矢量转置之后就是列矢量请注意看它的写法。

    >>> x = np.matrix([1,2,3,4,5])
    >>> x
    matrix([[1, 2, 3, 4, 5]])
    >>> x.transpose()
    matrix([[1],
            [2],
            [3],
            [4],
            [5]])

### 矩阵的点乘<a id="orgheadline40"></a>

学过线性代数印像最深的可能就是矩阵那个怪异的乘法运算了。这里有了numpy模块的支持，就可以直接用 `*` 来执行两个矩阵的乘法，或者 `np.dot` 函数。

    >>> A = np.matrix([[1,0,3,-1],[2,1,0,2]])
    >>> B = np.matrix([[4,1,0],[-1,1,3],[2,0,1],[1,3,4]])
    >>> A * B
    matrix([[ 9, -2, -1],
            [ 9,  9, 11]])
    >>> x = np.matrix([1,2,3])
    >>> y = np.matrix([4,5,6]).transpose()
    >>> x * y
    matrix([[32]])
    >>> y * x
    matrix([[ 4,  8, 12],
            [ 5, 10, 15],
            [ 6, 12, 18]])
    >>> np.dot(x,y)
    matrix([[32]])
    >>> np.dot(y,x)
    matrix([[ 4,  8, 12],
            [ 5, 10, 15],
            [ 6, 12, 18]])
    >>>

diag Return the diagonal (or off-diagonal) elements of a square matrix as a 1D array, or convert a 1D array into a square
matrix with zeros on the off-diagonal
dot Matrix multiplication
trace Compute the sum of the diagonal elements
det Compute the matrix determinant
eig Compute the eigenvalues and eigenvectors of a square matrix
inv Compute the inverse of a square matrix
pinv Compute the Moore-Penrose pseudo-inverse inverse of a square matrix
qr Compute the QR decomposition
svd Compute the singular value decomposition (SVD)
solve Solve the linear system Ax = b for x, where A is a square matrix
lstsq Compute the least-squares solution to y = Xb

多个如下的线性方程的组合叫做线性方程组。其中$a\_1a\_2 \ldots b$可以是实数或复数。

\begin{equation}
a_1x_1 + a_2x_2 + \cdots + a_nx_n = b
\end{equation}

线性方程组的解有三种情况：无解，有唯一的解，有无穷多的解。其中如果是无解的，那么称这个线性方程组是不相容的，反之是相容的。

\section{解线性方程组}
这个线性方程组，

\begin{align}
x_1 - 2x_2 + x_3 =0 \\
2x_2 - 8x_3  = 8\\
-4x_1 + 5x_2 + 9x_3 = -9
\end{align}

用矩阵形式$ax = b$表示就是：
\\[

\begin{bmatrix}
1 & -2 & 1 \\
0 & 2 & -8 \\
-4 & 5 & 9
\end{bmatrix}

\begin{bmatrix}
x_1  \\
x_2 \\
x_3
\end{bmatrix}

=

\begin{bmatrix}
0 \\
8 \\
-9
\end{bmatrix}

\\]

用numpy模块的linalg\sidenote{linear algebra}子模块的solve函数可以解上面谈及的$ax = b$的线性方程组，其中solve的第一个参数是a第二个参数是b，上面的线性方程组解法如下：

\begin{xverbatim}[129]{py}
import numpy as np
a = np.matrix([[1,-2,1],[0,2,-8],[-4,5,9]])
b = np.matrix([0,8,-9]).transpose()
x = np.linalg.solve(a,b)
print(x)
\end{xverbatim}

上面矩阵形式$ ax = b $中，a有个名字叫\textbf{系数矩阵}，如果你把a和b横向合并成为一个新的矩阵，那么这个矩阵叫做\textbf{增广矩阵}，即用numpy模块的hstack命令处理之，如下所示：

\begin{tcbpython}[]
import numpy as np
a = np.matrix([[1,-2,1],[0,2,-8],[-4,5,9]])
b = np.matrix([0,8,-9]).transpose()
c = np.hstack((a,b))
\end{tcbpython}

上面c的结果这里用稍微漂亮的形式输入如下：
\[\begin{bmatrix}
1& -2& 1&  0 \\
0&  2& -8&  8 \\
-4& 5&  9& -9
\end{bmatrix}
\]

关于计算机的内部解法细节我还不太清楚。

手工解法简单的就是代换等等常规运算，这里略过，其基础就是基本的两边等式操作法则，比如同时乘以一个数，两个不变等等。更复杂一点的需要将其做成增广矩阵的形式，然后运用高斯消元法。就是不断地运用以下操作：

\begin{itemize}
\item 一行同时乘以一个非零的数
\item 两行对换
\item 一行换成它自身和另一行的倍数的和
\end{itemize}

增广矩阵对应的值不变。

操作的目的是将增广矩阵的左下角都化为零，最后形成阶梯矩阵形式。

基于上面的阶梯矩阵形式，我们对于这个线性方程组的解的大致情况，马上就有一个直观的判断了：如果有一行系数矩阵那一行都是零了，而最后有个非零的数，那么就是类似于$ 0x\_1 + 0x\_2 &ctdot;  = 2$这样的形式，所有的参数都和零相乘了反而得到一个数，这显然是荒谬的，那么肯定这个线性方程组无解。如果这个增广矩阵有一行全是零，然后我们有两个未知量就需要两个线性方程，三个未知量需要三个有意义的线性方程，如果现在n个未知量有n个线性方程，现在推出有一行全为零也就是少了一个约束条件，那么可以肯定这个线性方程组有多个解。

## 随机数生成支持<a id="orgheadline42"></a>

seed Seed the random number generator
permutation Return a random permutation of a sequence, or return a permuted range
shuffle Randomly permute a sequence in place
rand Draw samples from a uniform distribution
randint Draw random integers from a given low-to-high range
randn Draw samples from a normal distribution with mean 0 and standard deviation 1 (MATLAB-like interface)
binomial Draw samples a binomial distribution
normal Draw samples from a normal (Gaussian) distribution
beta Draw samples from a beta distribution
chisquare Draw samples from a chi-square distribution
gamma Draw samples from a gamma distribution
uniform Draw samples from a uniform [0, 1) distribution

# 参考资料<a id="orgheadline44"></a>

1.  numpy user手册
2.  numpy reference 手册
3.  python for data analysis; Author:Wes McKinney; Version: 2012
4.  线性代数及其应用