创建Rmarkdown后，写上下面一句话，可以避免不必要的信息乱显示
<pre> 
<code> 
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, message = F, warning = F)
```
</code> 
</pre>

创建代码块的快捷方式 `Ctrl` + `Alt` + `i`


导出为markdown时，需要在console中输入图下代码：

```r
knitr::knit('file_path')
```

参考资料：

[Introduction (rstudio.com)](https://rmarkdown.rstudio.com/lesson-1.html)