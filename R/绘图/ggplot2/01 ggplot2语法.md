# 入门级绘图模板

模板：

`<DATA>`：填写数据

`GEOM_FUNCTION` ：填写函数

`<MAPPING>` ：填写横纵坐标

```r
ggplot(data = <DATA>)+
  <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>),
                                           stat = <stat>,
                                           position = <position>)+
<COORDNATE_FUNCTION>+
<FACET_FUNCTION>
```

实用举例：

```r
ggplot(data = iris)+
  geom_point(mapping = aes(x = Sepal.Length,
                                               y = Petal.Length))
```

# 属性设置

## 手动设置

使用举例

注意：此时 `color` 和其他一些属性和 `mapping` 是平级关系，是`geom_point()` 的参数。

```r
ggplot(data = iris) + 
  geom_point(mapping = aes(x = Sepal.Length, 
                                               y = Petal.Length), 
                       color = 'blue'
                       size = 5,     # 点的大小5mm
                       alpha = 0.5,  # 透明度 50%
                       shape = 8)  # 点的形状
```

### color 颜色

颜色可以是英文颜色的单词，也可以是十六进制颜色代码

### size 大小

单位为mm

### shape 形状

形状用数字编号表示

![](../../../attachment/Pasted%20image%2020231018195718.png)

### alpha 透明度

### fill 填充颜色

## 映射

### 使用默认条件

按照数据框的某一列来定义图的某个属性

实用举例：根据内置数据 `iris` 的 `Species` 列的内容来分配颜色

注意：此时 `color` 位于 `aes()` 内部，是 `aes()` 的参数，此时 `color` 的值是列名

```r
ggplot(data = iris)+
  geom_point(mapping = aes(x = Sepal.Length,
                                               y = Petal.Length,
                                               color = Species))
```

### 自定义映射颜色

注意：`scale_color_manual()` 和 `geom_point()` 是平级关系

颜色也可以是十六进制颜色代码

```r
ggplot(data = iris)+
  geom_point(mapping = aes(x = Sepal.Length,
                           y = Petal.Length,
                           color = Species))+
  scale_color_manual(values = c("blue","green","#808069"))
```

如果要改变形状，注意此时 `shape` 参数的位置， `shape` 位于 `geom_point()` 函数内部，与 `mapping` 平级：

```r
ggplot(data = iris)+
  geom_point(mapping = aes(x = Sepal.Length,
                           y = Petal.Length,
                           color = Species),
                      shape = 11)+
  scale_color_manual(values = c("blue","green","#808069"))  # 注意values不能省略
```

### 使用配色

[R Color Brewer’s palettes – the R Graph Gallery (r-graph-gallery.com)](https://r-graph-gallery.com/38-rcolorbrewers-palettes.html)

```r
ggplot(data = iris)+
  geom_point(mapping = aes(x = Sepal.Length,
                           y = Petal.Length,
                           color = Species),
                       shape = 20)+
  scale_color_brewer(palette = 'Set3')   # 注意，palette不能省略
 ```

# 分面

把一张图分成多张子图

### facet_wrap() 简单分面

注意 `facet_wrap()` 和 `geom_point()` 是平级关系

```r
ggplot(data = iris) + 
  geom_point(mapping = aes(x = Sepal.Length, y = Petal.Length)) + 
  facet_wrap(~ Species)  # 波浪线和Species有个空格，是说根据Species进行分面
```

当数据差异大的时候，不希望让子图使用相同的y轴刻度，可在`facet_wrap()` 添加一个参数scales，如 `facet_wrap(~ Species, scales = 'free')`

### facet_grid() 双分面

注意 `facet_grid()` 和 `geom_point()` 是平级关系

```r
dat = iris
dat$Group = sample(letters[1:5],150,replace = T) 
# 上面的代码只是为了方便演示，创建了一个新的数据集
ggplot(data = dat) + 
  geom_point(mapping = aes(x = Sepal.Length, y = Petal.Length)) + 
  facet_grid(Group ~ Species) 
```

# 几何对象

几何对象可以叠加

例如使用iris数据集画一个散点图和一个平滑曲线图，且将他们叠加到一起，代码如下：

```r
ggplot(data = iris) + 
  geom_smooth(mapping = aes(x = Sepal.Length, y = Petal.Length))+
  geom_point(mapping = aes(x = Sepal.Length, y = Petal.Length))
```

### 全局和局部变量

仔细观察上面的代码，可发现 `geom_smooth()` 和 `geom_point()` 函数内的参数是一样的，这样写没毛病，就是不太优雅，可以改写成这样：

```r
ggplot(data = iris, mapping = aes(x = Sepal.Length, y = Petal.Length))+
  geom_smooth()+
  geom_point()
```

注意，只有用的数据是同一组数据，才能这样改写，这种就属于全局设置，对所有图层都有效；而前一种写法为局部设置，仅对所属图层有效。

# 统计变换

## 直接根据原始数据画柱状图

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut))

ggplot(data = diamonds) + 
  stat_count(mapping = aes(x = cut))
```

## 直接根据原始数据画比例

其实就是变相的频率分布直方图

还是以 diamonds 数据集为例，画出每种切工所占的比例

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = after_stat(prop), group = 1))
```

注意此时需要有 `y = after_stat(prop)` 和 `group = 1` 参数，后者是说这几种切工加起来总共是1

## 只有统计结果，画柱状图

例如，现在有一个数据框变量 `fre`，它的内容如下：

| Var1      | Freq  |
| --------- | ----- |
| Fair      | 1610  |
| Good      | 4906  |
| Very Good | 12082 |
| Premium   | 13791 |
| Ideal          |  21551     |

此时如果要做柱状图，就需要指定`y` ，并且添加一个参数 `stat = 'identity'`

代码如下：

```r
ggplot(data = fre) +
  geom_bar(mapping = aes(x = Var1, y = Freq), 
                                      stat = "identity")
```

# 位置关系

## geom_jitter() 绘制抖动的点

画图时，尤其是多个图层花在一起时，要注意数据在图中的位置关系，例如：

```r
ggplot(data = iris, mapping = aes(x = Species, y = Sepal.Width, fill = Species))+
  geom_boxplot()+
  geom_point() 
```

上面这段代码绘制出来的图片图下，初看问题不大，细看会发现实际每个Species有50个数据，但下面这个图片中每个Species中的点并没有50个，这是因为 `geom_point()` 绘制出来的点重合到了一起

![](../../../attachment/Pasted%20image%2020231019083234.png)

为了解决这个问题，可以使用 `geom_jitter()` 函数来解决这个问题，该函数的描述：The jitter geom is a convenient shortcut for `geom_point(position = "jitter")`. It adds a small amount of random variation to the location of each point, and is a useful way of handling overplotting caused by discreteness in smaller datasets.

代码及结果如下：

```r
ggplot(data = iris, mapping = aes(x = Species, y = Sepal.Width, fill = Species))+
  geom_boxplot()+
  geom_jitter() 
```

![](../../../attachment/Pasted%20image%2020231019083829.png)

## position = "dodge"

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill=clarity))
```

上面这个普通代码画出来的图图下：

![](../../../attachment/Pasted%20image%2020231019085643.png)

给代码加上 `position = "dodge"` 后，结果如下：

![](../../../attachment/Pasted%20image%2020231019090225.png)

## position="fill"

书接上文

如果把 `position="dodge"` 改为 `position="fill"`，结果图下：

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut,fill=clarity),
           position = "fill")+
  labs(y = 'frequency')
```

由于加了 `position = "fill"` 后，绘制出来的图的含义变成了每种clarity在每种cut中的频率分布，所以加上了 `labs(y = 'frequency')` 来修改y轴坐标

![](../../../attachment/Pasted%20image%2020231019090355.png)

# 坐标系

正常情况下会出来的图片：

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = cut),
           width = 1) + 
  theme(aspect.ratio = 1) +
  labs(x = NULL, y = NULL) +
```

![](../../../attachment/Pasted%20image%2020231019084752.png)

## coord_flip() 翻转坐标系

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = cut),
           width = 1) + 
  theme(aspect.ratio = 1) +
  labs(x = NULL, y = NULL) +
  coord_flip()
```

![](../../../attachment/Pasted%20image%2020231019084814.png)

## coord_polar() 极坐标系

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = cut),
           width = 1) + 
  theme(aspect.ratio = 1) +
  labs(x = NULL, y = NULL) +
  coord_polar() 
```

![](../../../attachment/Pasted%20image%2020231019084855.png)

# 保存

```r
ggsave("file_path")
或
ggsave(pic_object, filename="file_path")
```