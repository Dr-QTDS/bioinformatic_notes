数据框以表格的形式显示的数据。

数据框可以包含不同类型的数据，但每列数据的类型应该相同。

数据框不允许存在重复的行名

# 创建

使用 `data.frame()` 创建数据框

```r
 df1 = data.frame(
 gene = paste0('gene', 1:4),
 change = rep(c('up', 'down'), each=2),
 score = c(5, 3, -2, -4)
 )
```

结果如下：

![](../../attachment/Pasted%20image%2020231017100514.png)

# 从文件读取数据框

```r
df <- read.csv("file_path")
```

# 查询数据框属性

以下代码示例均以如下数据框为例：

```r
df = data.frame(
gene = paste0('gene', 1:4),
change = rep(c('up', 'down'), each=2),
score = c(5, 3, -2, -4)
)
```

## dim(x) 获取对象维度

```r
> dim(df)
[1] 4 3
```

## nrow(x) 获取数据框行数

```r
> nrow(df)
[1] 4
```

## ncol(x) 获取数据框列数

```r
> ncol(df)
[1] 3
```

## length(x) 同ncol(x)获取数据框列数

```r
> length(df)
[1] 3
```

## names(x) 获取列名

结果同 `colnames()`

## rownames(x) 获取行名

返回值为一个向量

```r
> rownames(df)
[1] "1" "2" "3" "4"
```

这个函数也可以给行添加/修改名称，参考[给矩阵添加行名](05%20矩阵%20matrix.md#给矩阵添加行名)

## colnames(x) 获取列名

返回值为一个向量

```r
> colnames(df)
[1] "gene"   "change" "score"
```

这个函数也可以给列改名，参考[给矩阵添加列名](05%20矩阵%20matrix.md#给矩阵添加列名)

## summary(x) 总结数据框

```r
> summary(df)
     gene                  change                   score     
 Length:4               Length:4                Min.   :-4.0  
 Class :character    Class :character     1st Qu.:-2.5  
 Mode :character   Mode  :character    Median : 0.5  
                                                             Mean   : 0.5  
                                                             3rd Qu.: 3.5  
                                                             Max.   : 5.0
```

# 增

以下代码示例均以如下数据框为基础：

```r
df = data.frame(
gene = paste0('gene', 1:4),
change = rep(c('up', 'down'), each=2),
score = c(5, 3, -2, -4)
)
```

## 增加行

使用 `rbing()` 函数增加行

```r
> df <- rbind(df, c('gene5', 'up', 1))
> df
   gene change score
1 gene1     up     5
2 gene2     up     3
3 gene3   down    -2
4 gene4   down    -4
5 gene5     up     1
```

## 增加列

### 使用$新列名

由于原始的df数据框中没有p_value这一列，此时下面的代码会创建一个名为p_value的列

```r
> df$p_value <- c(0.05, 0.001, 0.007, 0.08)
> df
   gene change score p_value
1 gene1     up     5   0.050
2 gene2     up     3   0.001
3 gene3   down    -2   0.007
4 gene4   down    -4   0.080
```

### 使用cbind()

除了上面这个方法外，还可以使用 `cbind()` 函数来增加列

```r
> df <- cbind(df, tissue = c('brain', 'liver', 'heart', 'lung')) 
> df
   gene change score tissue
1 gene1     up     5  brain
2 gene2     up     3  liver
3 gene3   down    -2  heart
4 gene4   down    -4   lung
```

### 使用mutate()

`mutate()` 是 `dplyr` 包中的函数，使用前要提前载入 `dplyr`

举例1：在df1中新增一列，列名为 `new_frame`，列中的内容为 `gene` 和 `change` 列使用下划线相连

```r
> mutate(df1, new_frame = paste(gene, change, sep="_"))  # gene 和 change 不要加引号
   gene change score  new_frame
1 gene1     up     5   gene1_up
2 gene2     up     3   gene2_up
3 gene3   down    -2 gene3_down
4 gene4   down    -4 gene4_down
```

举例2：再来举个例子

要处理的信息如下：

```r
> test = iris[c(1,2,56,57,133,134),];test
    Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
1            5.1         3.5          1.4         0.2     setosa
2            4.9         3.0          1.4         0.2     setosa
56           5.7         2.8          4.5         1.3 versicolor
57           6.3         3.3          4.7         1.6 versicolor
133          6.4         2.8          5.6         2.2  virginica
134          6.3         2.8          5.1         1.5  virginica
```

新增一列，名为 `result`，其结果为 `Sepal.Length` × `Sepal.Width`，代码如下：

```r
> mutate(test, result = Sepal.Length * Sepal.Width)  # 列名不可加引号
    Sepal.Length Sepal.Width Petal.Length Petal.Width    Species result
1            5.1         3.5          1.4         0.2     setosa  17.85
2            4.9         3.0          1.4         0.2     setosa  14.70
56           5.7         2.8          4.5         1.3 versicolor  15.96
57           6.3         3.3          4.7         1.6 versicolor  20.79
133          6.4         2.8          5.6         2.2  virginica  17.92
134          6.3         2.8          5.1         1.5  virginica  17.64
```

# 删

去除某一行某一列，其实就是切片，把需要的切出来，然后重新赋值给原来的变量。

例如去除第2行和第2列，代码应写为 `df[-2, -3]`，其中-2代表提取不包含第2行，-3表示不包括第3列：

```r
> df[-2,-3] 
   gene change
1 gene1     up
3 gene3   down
4 gene4   down
> df  # df数据框的内容并没有改变
   gene change score
1 gene1     up     5
2 gene2     up     3
3 gene3   down    -2
4 gene4   down    -4
> df <- df[-2,-3];df  # 重新复制后df才会改变
   gene change
1 gene1     up
3 gene3   down
4 gene4   down
```

# 改

## 改一个元素值

```r
> df[1,2]
[1] "up"
>  df[1,2] <- 'down'
```

结果为：

```r
> df[1,2]
[1] "down"
```

## 改一整列

```r
> df$score
[1]  5  3 -2 -4
> df$score <- c(3,2,-1,-4)
```

结果为：

```r
> df$score
[1]  3  2 -1 -4
```

# 查 即取子集

类似于python 的 numpy 中对数组进行切片

以下代码示例均以如下数据框为基础：

```r
df = data.frame(
gene = paste0('gene', 1:4),
change = rep(c('up', 'down'), each=2),
score = c(5, 3, -2, -4)
)
```

## 注意事项

先来看一下如下代码：

```r
> df[3]
  score
1     5
2     3
3    -2
4    -4
```

```r
> df[,3]
[1]  5  3 -2 -4
```

可以发现 `df[3]` 和 `df[,3]` 输出的内容是一样的，但他们的展示格式不同，这是因为 `df[3]` 返回的是一个数据框，而 `df[,3]` 返回的是一个向量

```r
> class(df[3])
[1] "data.frame"

> class(df[,3])
[1] "numeric"
```


## 提取单个元素

根据坐标提取，坐标的写法为 `[行号,列号]`

```r
> df[2,1]
[1] "gene2"
> df[2,2]
[1] "up"
> df[2,3]
[1] 3
```

## 提取列

### 提取一列

使用 `$列名` 提取列信息，这个方法只能每次提取一列

```r
> df$gene
[1] "gene1" "gene2" "gene3" "gene4"
```

使用 `[,列名]` 提取列信息

```r
> df[,'score']   # 有逗号，返回向量
[1]  5  3 -2 -4
> df['score']   # 无逗号，返回数据框
  score
1     5
2     3
3    -2
4    -4
```

使用 `[,列号]` 提取一列

```r
> df[,3]  # 有逗号，返回向量
[1]  5  3 -2 -4
> df[3]   # 无逗号，返回数据框
  score
1     5
2     3
3    -2
4    -4
```

### 提取多列

使用 `[,列名向量]` 提取多列，逗号可以省略，提取出来的结果均为数据框

```r
> df[c('gene','score')]
   gene score
1 gene1     5
2 gene2     3
3 gene3    -2
4 gene4    -4
```

使用 `[,列号向量]` 提取多列，逗号可以省略，提取出来的结果均为数据框

```r
> df[1:2]
   gene change
1 gene1     up
2 gene2     up
3 gene3   down
4 gene4   down
> df[c(1,3)]
   gene score
1 gene1     5
2 gene2     3
3 gene3    -2
4 gene4    -4
```

### select()

`select()` 是 `dplyr` 包中的函数，使用前要提前载入 `dplyr`

`select()` 用于提取列，可以提取一列也可以提取多列

| 示例                    | 结果                                |
| ----------------------- | ----------------------------------- |
| `select(iris, 1)`       | 提取iris的第1列                     |
| `select(iris, c(1,3))`  | 提取iris的第1,3列                   |
| `select(iris, -2)` |  提取iris除第2列以外的列    |
| `select(iris, Species)` | 提取iris的Species列，注意不能加引号 |
| `select(iris, Sepal.Width:Species)` |     提取iris从Sepal.Width至Species的列 |

玩法还有很多，不一一列举

## 提取行

### 提取一行

使用 `[行号,]` 提取一行（注意行号后面有个逗号，且不可省略）

```r
> df[3,]
   gene change score
3 gene3   down    -2
```

### 提取多行

使用 `[行号向量,]` 提取多行（注意后面有个逗号，且不可省略）

```r
> df[2:3,]
   gene change score
2 gene2     up     3
3 gene3   down    -2
```

```r
> df[c(1,3),]
   gene change score
1 gene1     up     5
3 gene3   down    -2
```

### filter()

`filter()` 是 `dplyr` 包中的函数，使用前要提前载入 `dplyr`

`filter()` 用于提取行，可以提取一行也可以提取多行

| 示例                       | 结果                                    |
| -------------------------- | --------------------------------------- |
| `filter(mtcars, mpg < 20)` | 在mtcars中根据mpg这一列，选出小于20的列 |
| `filter(iris, Species == 'versicolor')` | 在iris中，选出Species为versicolor的列|

## 按照逻辑值取子集

数据框可以按照逻辑值取自己，TRUE对应的行/列留下，FALSE对应的行/列去掉

例如，针对df数据框，保留score>0 的行

```r
> df1[df1$score>0,]
   gene change score
1 gene1     up     5
2 gene2     up     3
```

# 合并
## merge() 连接两个数据框

```
merge(x, y, by = intersect(names(x), names(y)),
      by.x = by, by.y = by, all = FALSE, all.x = all, all.y = all,
      sort = TRUE, suffixes = c(".x",".y"), no.dups = TRUE,
      incomparables = NULL, ...)
```

示例

例如现有三个csv文件，要将它们合并到一起，这三个文件的内容分别为：

```csv
name,height,weight
Mike,177,75
Candy,168,55
John,181,85
Mary,165,49
```

```
name,gender,salary
Mike,male,6666
Candy,female,7777
John,male,8888
Linda,female,9999
```

```
NAME,gender,salary
Mike,male,6666
Candy,female,7777
John,male,8888
Linda,female,9999
```

读取这三个文件后，由上至下分别将数据框命名为info1 info2 info3

### 有共同列名

若要合并info1和info2，仔细观察这两个数据框，可知这两个数据框有相同的列名，这种情况，可直接运行下面代码合并两个数据框

```r
> merge(info1, info2, by='name')
   name height weight gender salary
1 Candy    168     55 female   7777
2  John    181     85   male   8888
3  Mike    177     75   male   6666
```

其中的 `by='name'` 是指根据 `name` 列来进行合并

### 无共同列名

若要合并info1和info3，仔细观察这两个数据框，可知这两个数据框虽然有含义相同的列名（即name），但大小写不匹配，如直接运行 `merge(info1, info3, by='name')` 则会报错：

```r
> info3 = read.csv('people_info3.csv')
> merge(info1, info3, by='name')
Error in fix.by(by.y, y) : 'by' must specify a uniquely valid column
```

为解决这个问题，可以直接简单粗暴的将info3中的 `NAME` 改为 `name`，然后在运行 `merge(info1, info3, by='name')` 进行合并。

也可以按照下面的代码进行合并：

```r
> merge(info1, info3, by.x = 'name', by.y = 'NAME')
   name height weight gender salary
1 Candy    168     55 female   7777
2  John    181     85   male   8888
3  Mike    177     75   male   6666
```

### 合并后保留不匹配项

默认情况下，`merge()` 函数只会保留两个数据框中都存在的匹配项。要保留不匹配的项，你可以使用 `all.x = TRUE` 或 `all.y = TRUE` 参数，具体取决于你想保留哪个数据框的不匹配项。

保留info3中的不匹配项

```r
> merge(info1, info3, by.x = 'name', by.y = 'NAME', all.y = TRUE)
   name height weight gender salary
1 Candy    168     55 female   7777
2  John    181     85   male   8888
3 Linda     NA     NA female   9999
4  Mike    177     75   male   6666
```

保留所有不匹配项

```r
> merge(info1, info3, by.x = 'name', by.y = 'NAME', all.x= TRUE,all.y = TRUE)
   name height weight gender salary
1 Candy    168     55 female   7777
2  John    181     85   male   8888
3 Linda     NA     NA female   9999
4  Mary    165     49   <NA>     NA
5  Mike    177     75   male   6666
```

## inner_join() 只保留两个表中有交集的内容

`inner_join()` 是 `dplyr` 包中的函数，使用前要提前载入 `dplyr`

## full_join() 取全集，一个都不能少

`full_join()` 是 `dplyr` 包中的函数，使用前要提前载入 `dplyr`

缺失的信息用NA表示

## left_join() 只保留左侧数据框中项

`left_join()` 是 `dplyr` 包中的函数，使用前要提前载入 `dplyr`

缺失的信息用NA表示

## right_join() 只保留右侧数据框中项

`left_join()` 是 `dplyr` 包中的函数，使用前要提前载入 `dplyr`

缺失的信息用NA表示

# 排序

根据某一列排序

## 使用order()

可以使用 `order()` 函数来对数据框进行排序。 `order()` 函数返回一个向量，该向量包含按升序排序的行索引。然后，使用这些行索引来重新排列数据框的行。

例如我们对合并前的info1的 `weight` 进行升序排序：

```r
> # 使用order()函数获取排序后的行索引
> sorted_index = order(info1$weight)
> sorted_index
[1] 4 2 1 3
> # 根据排序后的行索引重新排列数据框
> info1[sorted_index,]
   name height weight
4  Mary    165     49
2 Candy    168     55
1  Mike    177     75
3  John    181     85
```

如要降序，则在运行 `order()` 函数时加上参数，如 `order(info1$weight, decreasing = TRUE)`

## 使用arrange()

`arrange()` 是 `dplyr` 包中的函数，使用前要提前载入 `dplyr`

使用 `arrange()` 时，传入的列名不可以加引号！

同上，我们对合并前的info1的 `weight` 进行升序排序：

```r
> library(dplyr)
> arrange(info1, weight)  # weight 不可以加引号
   name height weight
1  Mary    165     49
2 Candy    168     55
3  Mike    177     75
4  John    181     85
```

降序排列代码如下：

```r
> arrange(info1, desc(weight))  # weight 不可以加引号
   name height weight
1  John    181     85
2  Mike    177     75
3 Candy    168     55
4  Mary    165     49
```

# 去除重复

根据某一列去重复

## 使用distinct() 

`distinct()` 是 `dplyr` 包中的函数，使用前要提前载入 `dplyr`

```r
> test = iris[c(1,2,56,57,133,134),];test
    Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
1            5.1         3.5          1.4         0.2     setosa
2            4.9         3.0          1.4         0.2     setosa
56           5.7         2.8          4.5         1.3 versicolor
57           6.3         3.3          4.7         1.6 versicolor
133          6.4         2.8          5.6         2.2  virginica
134          6.3         2.8          5.1         1.5  virginica
```

例如要对上面的数据去除重复，要求根据Species这一列内容，保留第一个数据，代码如下：

```r
> distinct(test, Species,.keep_all = T)  # Species 不能加引号 
  Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
1          5.1         3.5          1.4         0.2     setosa
2          5.7         2.8          4.5         1.3 versicolor
3          6.4         2.8          5.6         2.2  virginica
```

`.keep_all = T` 是指其他行的信息也要保留，如果没有这个参数，结果如下：

```r
> distinct(test, Species)
     Species
1     setosa
2 versicolor
3  virginica
```

# 常用函数

## rownames_to_column() 行名变为一列


