# 线性回归的假设和推导

假设我们有很多观测自变量 $x1,x2,x3,...,x_n$ 和对应的观测因变量 $y$.这里用排量和马力大小数据来演示

```
# Auto 数据是ISLR2包的一部分，导入ISLR2可以直接用
library(ISLR2)
auto = Auto
# 或者手动添加
# setwd('D:/Download')
# auto = read.csv('Auto.csv')
```

查看数据前6行，看看变量

```
attach(auto)
head(auto)
```

| mpg | cylinders | displacement | horsepower | weight | acceleration | year | origin |                      name                      |
|-----|-----------|--------------|------------|--------|--------------|------|--------|-----------------------------------------------|
|  18 |         8 |          307 |        130 |   3504 |         12.0 |   70 |      1 | chevrolet chevelle malibu                     |
|  15 |         8 |          350 |        165 |   3693 |         11.5 |   70 |      1 | buick skylark 320                             |
|  18 |         8 |          318 |        150 |   3436 |         11.0 |   70 |      1 | plymouth satellite                            |
|  16 |         8 |          304 |        150 |   3433 |         12.0 |   70 |      1 | amc rebel sst                                 |
|  17 |         8 |          302 |        140 |   3449 |         10.5 |   70 |      1 | ford torino                                   |
|  15 |         8 |          429 |        198 |   4341 |         10.0 |   70 |      1 | ford galaxie 500                              |

mpg: 每加仑汽油能跑多远

cylinders； 气缸数

displacement: 排量大小

horsepower: 马力大小

acceleration: 0-100 加速

根据常识，我们认为排量越大，马力肯定就更大，我们来画图检测一下


