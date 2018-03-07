主要是针对R使用过程中需要的注意事项和一些常用的使用建议，不针对专门的教程或者课程总结。

# 目录
* [1. 建议](#建议)
	* [1.1 子集选取与赋值](#子集选取与赋值)
	* [1.2 旋转坐标轴值](#旋转坐标轴值)
	* [1.3 绘图中的中文使用](#绘图的中文支持)
	* [1.4 多图一个pic](#单画面输出多图)
	* [1.5 绘图总体思路](#图形调整思路)

* [2. 注意事项](#注意事项)
	* [2.1 子集的负数索引值](#赋索引值)
	* [2.2 NA值的比较](NA值比较)
	* [2.3 对图形添加辅助信息](#添加辅助信息)
	* [2.4 相关性函数使用注意](#相关性函数的盲点)
   	* [2.5 factor转换](#因子转换)
   	* [2.6 绘图三种等级解释](#三种绘图等级)

* [3. 参考](#参考)





## 建议
建议主要是针对一些存在优化的使用方法，而进行记录。

### 子集选取
子集选取可以通过方括号的方式和subset方法调用的方式，两者都可以通过index的数值或者名称、布尔值来进行子集筛选。但是需要注意的是⚠️subset方法调用后，不能对结果进行复制；而方括号方法是可以的。例子如下：

```{r}
# A sample vector
v <- c(1,4,4,3,2,2,3)

subset(v, v<3)
#> [1] 1 2 2
v[v<3]
#> [1] 1 2 2


# Another vector
t <- c("small", "small", "large", "medium")

# Remove "small" entries
subset(t, t!="small")
#> [1] "large"  "medium"
t[t!="small"]
#> [1] "large"  "medium"

# A sample data frame
data <- read.table(header=T, text='
 subject sex size
       1   M    7
       2   F    6
       3   F    9
       4   M   11
 ')


subset(data, subject < 3)
#>   subject sex size
#> 1       1   M    7
#> 2       2   F    6
data[data$subject < 3, ]
#>   subject sex size
#> 1       1   M    7
#> 2       2   F    6


# Subset of particular rows and columns
subset(data, subject < 3, select = -subject)
#>   sex size
#> 1   M    7
#> 2   F    6
subset(data, subject < 3, select = c(sex,size))
#>   sex size
#> 1   M    7
#> 2   F    6
subset(data, subject < 3, select = sex:size)
#>   sex size
#> 1   M    7
#> 2   F    6
data[data$subject < 3, c("sex","size")]
#>   sex size
#> 1   M    7
#> 2   F    6

# Logical AND of two conditions
subset(data, subject < 3  &  sex=="M")
#>   subject sex size
#> 1       1   M    7
data[data$subject < 3  &  data$sex=="M", ]
#>   subject sex size
#> 1       1   M    7


# Logical OR of two conditions
subset(data, subject < 3  |  sex=="M")
#>   subject sex size
#> 1       1   M    7
#> 2       2   F    6
#> 4       4   M   11
data[data$subject < 3  |  data$sex=="M", ]
#>   subject sex size
#> 1       1   M    7
#> 2       2   F    6
#> 4       4   M   11


# Condition based on transformed data
subset(data, log2(size) > 3 )
#>   subject sex size
#> 3       3   F    9
#> 4       4   M   11
data[log2(data$size) > 3, ]
#>   subject sex size
#> 3       3   F    9
#> 4       4   M   11


# Subset if elements are in another vector
subset(data, subject %in% c(1,3))
#>   subject sex size
#> 1       1   M    7
#> 3       3   F    9
data[data$subject %in% c(1,3), ]
#>   subject sex size
#> 1       1   M    7
#> 3       3   F    9
```

针对子集赋值，subset方法会报错。两者的差异如下：

```{r}
v[v<3] <- 9

subset(v, v<3) <- 9
#> Error in subset(v, v < 3) <- 9: could not find function "subset<-"
```

### 旋转坐标轴值
需要旋转坐标轴值的时候，在使用ggplot2时候可以通过更改图层theme的方式来改变坐标轴的值

```{r}
ggplot(mf,aes(x=V2,y=V1))+geom_boxplot() +
 theme(axis.text.x = element_text(angle = 45, hjust = 0.5, vjust = 0.5))
```

还有一种方式是直接通过text方法中srt参数调整方向

```
data=c(4.51,10.69,9.33,7.34,5.09,11.68,4.47,8.53,13.99,5.22,4.22,9.23,7.86)
labs=c(“Species1″,”Species2″,”Species3”, “Species4”, “Species5”, “Species6”, “Species7”, “Species8”, “Species9”, “Species10”, “Species11”, “Species12”, “Species13”)
barplot(data,col=c(“steelblue”,”steelblue”,”steelblue”,”mediumturquoise”,”mediumturquoise”,”mediumturquoise”,”mediumturquoise”,”mediumturquoise”,”mediumturquoise”,”sandybrown”,”hotpink”,”hotpink”,”hotpink”),ylim=c(0,14),width=1,space=1,ylab=”%(……)”,las=1)
text(x=seq(1.5,25.5,by=2),y=-0.15, srt = 45, adj = 1, labels = labs,xpd = TRUE)
abline(h=c(2,4,6,8,10,12,14),col=”#00000088″,lwd=2)
abline(h=0)
```

### 因子转换
因子转换有三种常用方式，factor，levels以及ordered，其中factor是三者中主要方式。factor方法可以调用相关参数来完成ordered以及levels的相应功能。注意factor中order参数需要的是逻辑值，以判断是否需设置为顺序型数据，levels参数可以覆盖原有的level。**另外需要注意，如果在设置了order为TRUE之后，没有人为设定level，那么其level将按照字母顺序排序**

```
factor(c("Poor", "Improved", "Excellent, "Poor"), order=TRUE, levels=c("Poor", "Imporved", "Excellent"))
```

对于数值型转换为因子性，也可以通过factor的方法来设置，但是需要通过设置levels和labels参数来定义相关值。

```
factor(c(1, 2, 3, 1, 2, 2, 1, 2, 4), levels=c(1, 2), labels=c("Male", "Female"))

#result
[1] Male   Female <NA>   Male   Female Female Male   Female <NA>
Levels: Male Female
```

以上数据中只有1和2被设置为了对应的因子型，其他对应的值为NA。当然也可以设置为order型，方法如下；

```
factor(c(1, 2, 3, 1, 2, 2, 1, 2, 4), levels=c(1, 2), labels=c("Male", "Female"), order=TRUE)

#result
[1] Male   Female <NA>   Male   Female Female Male   Female <NA>
Levels: Male < Female
```

### 绘图中的中文使用
R中绘图时，可能会遇见中文支持的问题——中文在图中显示为[]。可以通过更改图中字体家族参数来达到显示中文的目的——可以通过设置par的family参数。其他设置方式——可以直接在图片中family参数和启用cairo模式，如下

```
png("foo.png", type="cairo",  , family="SimSun")	#启用cairo模式
plot(1, type="n")
text(1, 1, "这是cairo模式")
```
另外可以支持黑体(SimHei)、楷体(KaiTi_GB2312)、幼圆(YouYuan)、隶属(LiSu)，或则会更一般的图形设备(非cairo模式下)——需要设置family="GB1"。

### 单画面输出多图
单画面输出多图，有基础的方式，也可以使用第三方的package。基础方式时利用par函数、layout函数或者split.screen()函数，第三方的package是使用gridExtra。基础方式如下：

1. 修改绘图参数，par(mfrow=c(2,2))或者par(mfcol=c(2,2))
2.  使用更为强大的layout函数，可以设置图形绘制顺序和图形大小
3. split.screen()函数

### 图形调整思路
从R的功能来看，其根本就不是一个"合格的编程工具"，但是在图形表现和数据分析上确实是有不可比拟的优势。因此在制图之前需要对数据进行Wragling，了解数据表达对意义。之后才是绘图，基于绘图的总体思路如下：

1. 选择plot或者ggplot2中的简单工具，先进行绘图——绘制出需要表现的idea
2. 根据idea，不断通过scale工具对坐标轴、图形等进行调整
3. 图形基本确认后，再去调整其他相关的内容。此时才去考虑使用theme函数，对图形中xy轴、legend等的位置、颜色进行调整

## 注意事项⚠️
需要注意在coding过程中需要知道的一些trap，或者说和其他相关语言具有差异需要注意的。

### 索引值为负数
在一般情况下，索引值为负是从后向前取值的方式，但是在R中是表示不要该index上的值。在一些方法用使用select参数的时候也可以使用负值来表示不需要改列或者该索引位置上的值，例子如下：

```{r}
# Here's the vector again.
v
#> [1] 1 4 4 3 2 2 3

# Drop the first element
v[-1]
#> [1] 4 4 3 2 2 3

# Drop first three
v[-1:-3]
#> [1] 3 2 2 3
```
### 众数方法
需要注意⚠️mode方法不是调用众数的方法，其返回的值是**数据类型**；如果要使用统计学上众数的方法，需要调用mfv()方法——(most frequent value)

### NA值的比较
R中有几类比较特殊的值，NULL、NA、NaN、Int、+Int、-Int。NULL表示的是没有值，而后面两者则是表示的是值存在错误，Int表示的无穷大的意思。

因为NULL不表示值，所以在数据集中存在NULL的时候是可以直接计算的，但是后两者NA和NaN会返回同类型的值——因为该两者是表示值错误。因此在进行计算的时候需要进行相关的处理，使用!is.na或者!is.nan方法，如下

```
vy
# 1  2  3 NA  5
vy[ !is.na(vy) ]
# 1  2  3  5

vz
# 1   2   3 NaN   5
vz[ !is.nan(vz) ]
# 1  2  3  5
```
同样在进行比较的时候，不论比较的两者中错误值在哪个里面都会返回错误值。因此如果要对错误值进行比较需要采用一些特殊方法进行处理：

```
# This function returns TRUE wherever elements are the same, including NA's,
# and FALSE everywhere else.
compareNA <- function(v1,v2) {
    same <- (v1 == v2) | (is.na(v1) & is.na(v2))
    same[is.na(same)] <- FALSE
    return(same)
}
```

### 添加辅助信息
当在使用ggplot的时候，添加辅助统计信息，需要申明使用stat参数，调用的函数方法需要相关参数如fun.y，以及函数参数的调用需要fun.args——fun.args=list(probs=.9)。此过程中需要⚠️已经和之前版本存在差异。相关使用方法如下：

```{r}
ggplot(aes(age, friend_count), data=pf) +
  xlim(13, 90) +
  geom_point(alpha=0.05, position=position_jitter(h=0), color="orange") +
  coord_trans(y="sqrt") +
  geom_line(stat="summary", fun.y=mean) +
  geom_line(stat="summary", fun.y=quantile, fun.args=list(probs=.1), linetype=2, color="blue") +
  geom_line(stat="summary", fun.y=quantile, fun.args=list(probs=.9), linetype=2, color="blue") +
  geom_line(stat="summary", fun.y=quantile, fun.args=list(probs=.5), color="blue")
```
### 相关性函数盲点
在使用相关性函数cor以及cor.test的时候，它们是对整个数据集进行相关性分析。可能会因为因为数据是周期性的相关，而形成相关性变弱。推荐使用energy这个package中的[dcor.ttest方法](https://www.rdocumentation.org/packages/energy/versions/1.7-0/topics/dcor.ttest)来探索相关性，

### 三种绘图等级
R中绘图包括三种等级，即是高级(High_level)、低级(Low_level)以及交互式(Interactive)三种命令方式。高级方式指的是在图形设备傻姑娘绘制新图；低级方式指的是利用绘图命令在已经存在的图形傻姑娘添加更多的绘图信息，如点、线、多边形等；交互式方式指利用鼠标等定点方式来添加或者提取绘图信息。

## 参考
1. [Data wrangling package-dplyr & tidyr](https://s3.amazonaws.com/udacity-hosted-downloads/ud651/DataWranglingWithR.pdf)
