矩阵是一个包含列和行的二维数据集。

没有列名和行名。

# matrix() 创建矩阵

指定 `nrow` 和 `ncol` 参数来获取行和列的数量（通常这两个参数用其中一个即可）:

```r
> m <- matrix(1:12, ncol = 4)
> m
     [,1] [,2] [,3] [,4]
[1,]    1    4    7   10
[2,]    2    5    8   11
[3,]    3    6    9   12
```

# 属性

dim(x), nrow(x), ncol(x), length(x), summary(x) 这几个函数也可以作用于矩阵

```r
> dim(m)
[1] 3 4
> nrow(m)
[1] 3
> ncol(m)
[1] 4
> length(m)
[1] 12
> summary(m)
       V1            V2            V3            V4      
 Min.   :1.0   Min.   :4.0   Min.   :7.0   Min.   :10.0  
 1st Qu.:1.5   1st Qu.:4.5   1st Qu.:7.5   1st Qu.:10.5  
 Median :2.0   Median :5.0   Median :8.0   Median :11.0  
 Mean   :2.0   Mean   :5.0   Mean   :8.0   Mean   :11.0  
 3rd Qu.:2.5   3rd Qu.:5.5   3rd Qu.:8.5   3rd Qu.:11.5  
 Max.   :3.0   Max.   :6.0   Max.   :9.0   Max.   :12.0
```

## 给矩阵添加行名

```r
> m
     [,1] [,2] [,3] [,4]
[1,]    1    4    7   10
[2,]    2    5    8   11
[3,]    3    6    9   12
> rownames(m) <- c('A','B','C')  # 添加行名
> m
  [,1] [,2] [,3] [,4]
A    1    4    7   10
B    2    5    8   11
C    3    6    9   12
```

## 给矩阵添加列名

```r
> m
  [,1] [,2] [,3] [,4]
A    1    4    7   10
B    2    5    8   11
C    3    6    9   12
> colnames(m) <- c('a','b','c','d')  # 添加列名
> m
  a b c  d
A 1 4 7 10
B 2 5 8 11
C 3 6 9 12
```

# 矩阵的增删改查同数据框

# t(x) 转置

注：运行 `t(x)` 后，原来的矩阵不会发生改变

```r
> m
  a b c  d
A 1 4 7 10
B 2 5 8 11
C 3 6 9 12
> t(m)
   A  B  C
a  1  2  3
b  4  5  6
c  7  8  9
d 10 11 12
```

# as.data.frame(x) 转换为数据框

注：运行 `as.data.frame(x)` 后，原来的矩阵不会发生改变

```r
> class(m)
[1] "matrix" "array" 
> class(as.data.frame(m))
[1] "data.frame"
```

