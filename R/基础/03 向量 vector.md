数据框单独拿出来的一列是向量，视为一个整体

一个向量只能有一种数据类型

# 创建向量

## 用c()逐一放到一起 

```R
> c(1,2,3,4,5)
[1] 1 2 3 4 5
> c('a', 'b', 'c', 'd', 'e')
[1] "a" "b" "c" "d" "e"
```

## 连续的数字用冒号“:”

```R
> 1:10
 [1]  1  2  3  4  5  6  7  8  9 10
```

## 有重复的用rep()

```r
> rep(6,times=6)
[1] 6 6 6 6 6 6
> rep("six",times=6)
[1] "six" "six" "six" "six" "six" "six"
```

还可以这样用，把times换成each

```r
> rep(c('a','b','c','d','e'), each=3)
 [1] "a" "a" "a" "b" "b" "b" "c" "c" "c" "d" "d" "d" "e" "e" "e"
```

## 有规律的序列用seq()

```r
> seq(from=1,to=10,by=2)
[1] 1 3 5 7 9
> seq(from=10,to=1,by=-2)
[1] 10  8  6  4  2
```

## 随机数用rnorm()

```r
> rnorm(n=5)
[1]  1.7364994  0.4086225  0.9205950 -0.9135026  0.6306991
```

## 通过组合，产生复杂向量

```r
> paste0('x', 1:5)
[1] "x1" "x2" "x3" "x4" "x5"
```

# 向量索引

R与python不同之处：

1. R的索引起始为1
2. R可以一条命令取出多个索引不想连的元素，入下面的 `x[c(1,5)]`
3. R没有负数索引，`x[-1]` 表示取出索引不为1的所有元素

```r
> x = c('a','b','c','d','e')
> x[2]
[1] "b"
>
> x[2:4]
[1] "b" "c" "d"
>
> x[-1]
[1] "b" "c" "d" "e"
>
> x[c(1,5)]
[1] "a" "e"
>
> x[-(2:4)]  # 注意要用括号把2:4括起来
[1] "a" "e"
```

# 修改元素值

修改一个元素值

```r
> x = c('a','b','c','d','e')
> x[2] = 'z'
> x
[1] "a" "z" "c" "d" "e"
```

修改多个元素值

```r
> x = c('a','b','c','d','e')
> x[c(2,4)] = c('y','l')
> x
[1] "a" "y" "c" "l" "e"
```

# 向量筛选

向量筛选使用 `[]` ，将TRUE对应的值挑选出来，FALSE丢弃

```r
> x = c(3,7,2,6,8,11)
> x[x == 6]
[1] 6
> x[x > 6]
[1]  7  8 11
```

还可以两个向量间进行筛选

```r
> x = c(3,7,2,6,8,11)
> y = c(2,5,4,3,7,12)
> 
> x[x<y]
[1]  2 11
>
> x[x %in% y]
[1] 3 7 2
```

更神奇的是还可以取反！毕竟是针对TRUE和FALSE进行操作，只要用感叹号取个反就行，例如：

```r
> x = c(3,7,2,6,8,11)
> y = c(2,5,4,3,7,12)
> x[!x<y]
[1] 3 7 6 8
> 
> x[!x %in% y]
[1]  6  8 11
```

# 向量常用函数

## length() 求向量的元素个数

```r
> x = c(1,2,3,4,5,5,3,1)
> length(x)
[1] 8
```

## unique() 去除向量内重复元素

```r
> x = c(1,2,3,4,5,5,3,1)
> unique(x)
[1] 1 2 3 4 5
```

## duplicated() 判断向量内元素是否重复

```r
> x = c(1,2,3,4,5,5,3,1)
> duplicated(x)
[1] FALSE FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE
```  

如果要取反的话，可以在dupicated()前面加个`!` ：

 ```r
> x = c(1,2,3,4,5,5,3,1)
> !duplicated(x)
> [1]  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE
```

## sort() 向量元素排序

默认使用升序排序

```r
> x = rnorm(n=5);x
[1]  0.7698011 -1.3236839  2.2711866 -0.5787031 -1.1189199
> sort(x)
[1] -1.3236839 -1.1189199 -0.5787031  0.7698011  2.2711866
```

改为降序排序

```R
> x = rnorm(n=5); x
[1]  0.7698011 -1.3236839  2.2711866 -0.5787031 -1.1189199
> sort(x,decreasing = TRUE)
[1]  2.2711866  0.7698011 -0.5787031 -1.1189199 -1.3236839
```

## names(x) 获取/添加元素名称

```r
> score = c(100, 99, 97, 98)
> score
[1] 100  99  97  98
> names(score) <- c('Tom', 'Mike', 'John', 'Linda')  # 添加名称
> score
  Tom  Mike  John Linda 
  100    99    97    98 
> class(score)
[1] "numeric"
> score[1]
Tom 
100
> names(score)  # 获取名称
[1] "Tom"   "Mike"  "John"  "Linda"
```

元素有了名称，获取元素的值就可以通过名称来获取了，例如

```r
> score[c('Tom', 'Linda')]
  Tom Linda 
  100    98
```

更骚的操作是

```r
> names(score)[score > 98]
[1] "Tom"  "Mike"
```

## table() 向量中元素计数

```r
> x = c('a', 'b', 'c', 'c', 'd')
> table(x)
x
a b c d 
1 1 2 1
```

# 操作一个向量

## 数学计算

[算数运算符](02%20运算符.md#算数运算符) 都可以使用

```r
> x = 1:5
> x+2
[1] 3 4 5 6 7
```

## 使用数学函数进行计算

```r
> x = c(2,4,6,8)
> log2(x)
[1] 1.000000 2.000000 2.584963 3.000000
```

## 比较运算

```r
> x = c(2,4,6,8)
> x>3
[1] FALSE  TRUE  TRUE  TRUE
```

# 操作两个向量

## 数学计算

[算数运算符](02%20运算符.md#算数运算符) 都可以使用

```r
> x = c(1,2,3,4,5)
> y = c(5,4,3,2,1)
> x + y
[1] 6 6 6 6 6
```

## 比较运算

生成等长的逻辑向量

```r
> x = c(1,2,3,4,5)
> y = c(5,4,3,2,1)
> x == y
[1] FALSE FALSE  TRUE FALSE FALSE
```

## paste 连接向量

```r
> x = c(1,2,3,4,5)
> y = c(5,4,3,2,1)
> paste(x,y,sep=',')
[1] "1,5" "2,4" "3,3" "4,2" "5,1"
```

## %in% 查看x向量中的每个元素是否在y向量中

```r
> x = c(3, 1, 7, 5)
> y = c(2, 3, 5, 6)
> x %in% y
[1]  TRUE FALSE FALSE  TRUE
```

要注意 `x %in% y` 和`x == y` 的区别，前者是判断x中的元素是否在y中，后者是判断相同位置的元素是否相等。 

注意，取交集会去除重复，而 `%in%` 不会去除重复，例如

```r
> a = c(1,2,2,3,4,5,5)
> b = c(2,3,6)
> intersect(a,b)
[1] 2 3
> a[a %in% b]
[1] 2 2 3
```

## 循环补齐
 
两个向量在进行**等位运算**时（比较运算，数学运算，paste和paste0），短的向量进行循环，从而补齐长的向量

```r
> x = c(1,3,5,6,2)
> y = c(3,2,5)
> x == y
[1] FALSE FALSE  TRUE FALSE  TRUE
Warning message:
In x == y : longer object length is not a multiple of shorter object length
```

此时R语言进行了循环补齐操作，原理如下图：

![](../../attachment/Pasted%20image%2020231020125157.png)

## 集合运算

以下示例中，x 和 y向量均为：

```r
x = c(1,3,5,1)
y = c(3,5,2,6)
```

### intersect(x,y)交集

```r
> intersect(x,y)
[1] 3 5
```

注意，取交集会去除重复，而 `%in%` 不会去除重复，例如

```r
> a = c(1,2,2,3,4,5,5)
> b = c(2,3,6)
> intersect(a,b)
[1] 2 3
> a[a %in% b]
[1] 2 2 3
```

### union(x,y) 并集

并集会去除重复值

```r
> union(x,y)
[1] 1 3 5 2 6
```

### setdiff(x,y) 差集

```r
> setdiff(x,y)
[1] 1
```

```r
> setdiff(y,x)
[1] 2 6
```

注意观察上面两个函数的运行结果的差异，可以发现差集取的是 `setdiff()` 函数的第一个参数向量中有的，而第二个参数向量中没有的值。

换句话说 `setdiff(x,y)` 取的是x中有，而y中没有的值。

# 简单作图

```r
> k1 = rnorm(12);k1
 [1]  0.91392409 -2.19790986 -0.46998427 -0.05376909 -0.81742363  1.19042121 -1.04556256
 [8]  0.48461811 -0.74758760 -2.25441275 -0.62615547 -0.86849101
> k2 = rep(c('a','b','c','d'), each=3);k2
 [1] "a" "a" "a" "b" "b" "b" "c" "c" "c" "d" "d" "d"
> plot(k1)
> boxplot(k1~k2)
```

`plot(k1)` 的结果：

![](../../attachment/Pasted%20image%2020231016232840.png)

`boxplot(k1~k2)` 的结果：

![](../../attachment/Pasted%20image%2020231016233030.png)


