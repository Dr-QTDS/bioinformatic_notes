和 `ggplot2` 最大的区别，就是没有映射，列名需要带引号

示例代码：

```r
ggscatter(iris, x="Sepal.Length", y="Petal.Length", color="Species")
```

![](../../attachment/Pasted%20image%2020231019092327.png)

# 绘制箱线图

```r
ggboxplot(iris, 
                  x = "Species", 
                  y = "Sepal.Length",
                  color = "Species", 
                  shape = "Species",
                  add = "jitter")
```

![](../../attachment/Pasted%20image%2020231019093238.png)

## 在箱线图中加上组间p值比较

```r
my_comparisons <- list( c("setosa", "versicolor"), 
                                        c("setosa", "virginica"), 
                                        c("versicolor", "virginica") )

ggboxplot(iris, 
                 x = "Species", 
                 y = "Sepal.Length",
                 color = "Species", 
                 shape = "Species",
                 add = "jitter")+
  stat_compare_means(comparisons = my_comparisons)+  # 这个是组间比较
  stat_compare_means(label.y = 9)   # 这个是总体比较，并设置结果的摆放位置
```

![](../../attachment/Pasted%20image%2020231019093914.png)