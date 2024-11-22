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
