
## 测list/array/tuple/dictionary长度
len(a)

## 判断属于某几个之一：

    if ok in ('y', 'ye', 'yes'):

## 为函数添加key-val类型参数

    def parror(voltage, state='a stiff', action='voom'):

这样的函数可以直接用key-val风格的方式调用

    parrot(voltage=5.0, action='kill')

## 参数打包
有\* 和\*\* 两种方式。
\*和列表有关，在定义函数时，\*把收进来的参数按位置排好

    def sum(*values): 
        s = 0;
        for v in values:
          s = s + v
        return s

在调用函数时，把参数解包

    values = (1, 2)
    
`s = sum(*values)`等价于`s = sum(1, 2)`

\*\*和字典有关，在定义函数时，\*\*把收进来的参数按照字典方式展开

    def get_a(*values, **options):
      s = 0
      for v in values:
        s = s + v
      if "neg" in options:
        if options["neg"]:
          s = -s
      return s

在调用函数时，把参数解包。

## list comprehension
`map`: 两个参数，第一个为函数，第二个为list
`squares = map(lambda x: x \*\* 2, range(0, 10))`
用list comprehension方式可以改写为
`squares = [x ** 2 for x in range(10)]`

list comprehension特点：

* 可以加条件
    ```[for in if]```
* 可以连续叠加使用：

    ```
    vec = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    [num for elem in vec for num in elem]
    ```
    
* 可以多变量
 
    ```
    [(x, y) for x in range(0, 3) for y in range(0, 3) if x != y] 
    ```
    
* 可以嵌套
 
    ```
    matrix = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
    [[rows[i] for row in matrix] for i in range(4)]
    ```

## `zip`函数
`zip`用来把传入的各个list的第0、第1、……个元素分别打包。
  
    zip(seq0, [seq1,...]) --> [(seq0[0], seq1[0], ...), (seq0[1], ...)...]
   
    
因此，`zip`可以用来转置矩阵

    matrix = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
    matrix_transposed = map(list, zip(*matrix))


## numpy
* `fromfunction(f, size, dtype=float)`，生成一个`(x,y)`位置为`f(x,y)`的array
* `a[::-1]`翻转向量
* `a[0:3, 0:3]`取a的前2行前2列
* 当没有指定所有维度时，为指定维度认为是整个维度都取：二维`a[-1]=a[-1,:]`
* 可以用...代指剩余各维度:
  * `x[1,2,...]` is equivalent to `x[1,2,:,:,:]`
  * `x[...,3]` to `x[:,:,:,:,3]` and
  * `x[4,...,5,:]` to `x[4,:,:,5,:]`.
* `a.flat`可以展开矩阵`a`
* `a.resize`直接改变`a`，`a.reshape`和`a.transpose`不改变`a`，而是返回一个拷贝。`a.reshape(3, -1)`代表把a的第一维长度改为3，第`2`维长度自行计算。
* `newaxis`用来增加维数。`a=array([0, 1]),a[:,newaxis]`将把`a`变为`array([[0], [1]])`
* `ones/zeros/random.random/vstack/hstack`接受tuple输入，`vstack((a, b))`和`hstack((a, b))`
* 可以用`id(a)`来查看`a`的位置
* 一个对象的`view`是浅拷贝，实际上指向同一个东西，改变`view`的值会同时改变原变量的数值（但可以改变`view`的`shape`而不改变原变量的`shape`）。用`a.copy()`创建的是深拷贝，二者互不影响。可以通过`a.flags.owndata`来查看某个变量是否拥有自己的内存，浅拷贝的`view`该值为`False`，深拷贝的变量该值为`True`。发现,直接用赋值语句`b=a`创建出来的`b`是浅拷贝,但`b.flags.owndata`为`True`.因此目测这个标志位并不能用来判断深浅拷贝.记住:直接赋值都是浅拷贝,二者指向同一个对象,改变一个也会改变另一个。用`.copy()`方式复制是深拷贝.
* 矩阵索引:有这几种方式:
  * 一维向量用一维向量索引:`a = [11, 12, 13], a[[0, 1]] = [11, 12]`
  * 矩阵用一维向量索引,则剩下维度默认整个维度都取:
    ```
    b = arange(12).reshape((3, 4))
    b[[0, 1]] = [[0,1,2,3],[4,5,6,7]]
    ```
  * 矩阵用两个同样大小的矩阵索引,取出与索引矩阵大小相同,位置对应的矩阵:`b[[[1,2],[1,2]], [[0,1], [2,3]] ] = [[4,9],[6,11]]`.可赋值.
  * 矩阵用矩阵索引,可以取一行或者一列,取出来都成为列:`b[ [[1,2],[1,2]], 2]`取出第3列
  * 矩阵用矩阵索引,可以用:符号,用:符号表示的那一维把整个维度都取
  * 除了逻辑索引必须用array以外，矩阵可以用list或者tuple或者array索引,效果都一样。
  * `A[1:, [1, 3]]`取出A从第2行开始每行的第1列和第3列
  * 类似MATLAB：
   
      ```
      a = array([[1, 2], [3, 4]])
      a[array([0, 1]), array([0, 1])] = [1, 4] 
      ```

第一维为1-D，则两个索引每一对对应位置取一个元素，结果和索引矩阵一样大

    a[array([[0], [1]]), array([0, 1])] = [[1, 2], [3, 4]] 
    
第一维为2-D，则按照MATLAB风格取元素
    这给我们提供了不小的便利，我们可以用ix_()函数来索引矩阵。例如想取A的前2行前2列，可以A[ix_((0, 1), (0, 1))]

* 掐头去尾： a[1:]+a[:-1]


* 返回矩阵data每一列的最大值有这样两种方法：
  # 第一种
  data = random.random((5, 4))
  ind = data.argmax(axis=0)
  data\_max = data(ind, xrange(data.shape[1]))
  # xrange与range功能相同，不同的是range一次返回整个list，而xrange返回一个生成器。数据量大时xrange性能好一些
  # 第二种
  data\_max = data.max(axis=0)

* 在python中，bsxfun是自动完成的，一个大小为(3, 1)的矩阵和一个大小为(1, 4)的矩阵相加，结果是(3, 4)的矩阵
* numpy中有矩阵数据类型： from numpy.linalg import *
  * 继承了array类型。
  * \*号为矩阵乘法，而非element-wise
  * m.inv(),eye(),trace(m),det(m)
  * m.I代表逆矩阵，m.T代表转置。
  * 矩阵可以用来做索引，不过一般用来给矩阵做索引，而不是给一般的array
  * 和array的一个不同在于，matrix取元素时返回结果总是一个矩阵，而array取元素时返回的结果总是尽可能地维度小，例如
    M[:,0]返回一个矩阵，而A[:,0]返回一个一维的array向量
  * 在采用逻辑索引时需要主意一个问题：
    # 我们想要取出A或者M中所有最上方元素大于1的列
    A[:, A[0,:]>1] #正确
    M[:, M[0,:]>1] #错误。原因在于M[0,:]>1返回的是一个二维矩阵，不能直接对array进行索引。解决方案是，每个matrix变量都有一个.A元素，代表了M对应的array。M[:, M.A[0, :]>1]可以work


* 字符串有strip函数可以把开头和结尾的空白符都去掉~
* dir(a)查看可以对a进行的所有操作！


