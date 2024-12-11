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

```
plot(horsepower~displacement)
```

![a](https://github.com/Tony980624/Regression-Lasso-Regression-Logistic-regression/blob/main/images/Rplot.png)

果然，我们发现排量和马力之间存在线性关系，那么我们决定开始探究线性回归，线性回归的第一个假设：存在线性关系。也满足了

假设马力(horsepower) 为因变量y,排量(displacement)为自变量x, 在存在线性关系的前提下，他们的关系可以表达为 $y = \beta_0 + \beta_1 x + \epsilon$,在已有的数据中，我们想找到参数$\beta_0$ 和 $\beta_1$使得这个直线的总误差最小

## 最小二乘法

$RSS(误差平方和) = \sum^n_{i=1}(y_i-(\beta_0+\beta_1x))$

对 $\beta_0$ 求导

$\frac{\partial RSS}{\partial \beta_0} = \sum_{i=1}^n(y_i-\beta_0-\beta_1x_i)=0$

求得 $\beta_0 = \bar{y}-\beta_1\bar{x}$

对 $\beta_1$ 求导

$\frac{\partial RSS}{\partial \beta_1} = \sum_{i=1}^nx_i(y_i-\beta_0-\beta_1x_i)=0$ 展开后得到   $\sum_{i=1}^nx_iy_i = \beta_0 \sum_{i=1}^nx_i + \beta_1 \sum_{i=1}^nx_i^2$

将 $\beta_0$ 代入并化简

$\beta_1 = \frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-bar{y})}{\sum_{i=1}^n(x_i-bar{x})^2} = \frac{Cov(x,y)}{Var(x}$

## 几何推导

几何方法的核心思想是，在线性回归中，模型的预测值 $X^T\epsilon  = 0$, 即误差于自变量平面正交，无关联

$X^T\epsilon = 0$ $\implies$ $X^T(Y-X\hat{\beta}) = 0$ $\implies$ $X^TX\hat{\beta} = X^TY$ $\implies$  $\hat{\beta} = (X^TX)^{-1}X^TY$

```
# 计算截距和斜率
Y = auto$horsepower
X = auto$displacement
beta1 = solve(t(X)%*%X)%*%t(X)%*%Y
beta0 = mean(Y) - beta1*mean(X)
abline(beta0, beta1, col = "red", lwd = 2)
```

![a](https://github.com/Tony980624/Regression-Lasso-Regression-Logistic-regression/blob/main/images/Rplot1.png)

计算总误差平方和， 为255697

```
residuals_1 = sum((Y - X %*% beta1)^2)
```

加入控制变量'weight'，总误差平方和瞬间将为118685
```
X = as.matrix(auto[, c(3, 5)]) 
colnames(X) = NULL
beta1 = solve(t(X)%*%X)%*%t(X)%*%Y
beta0 = mean(Y) - beta1*mean(X)
residuals_2 = sum((Y - X %*% beta1)^2)
```

## 控制变量 weight 可能导致模型问题，需要检验

```
cor(auto$weight, auto$horsepower)
cor(auto$weight, auto$displacement)
```
分别是0.86和0.93， 重量和排量高度相关，多半造成多重共线性，multicolinearty,

```
# 检验多重共线性, 膨胀银子VIF
library(car)
model = lm(horsepower ~ displacement + weight, data = auto)
vif(model)
```

vif的值为7，大于5，说明两者确实高度相关

$VIF_j = \frac{1}{1-R_j}$

$R^2公式:1-\frac{残差平方和(SSE)}{总平方和(SST}, SST表示y的实际值和均值的差的平方之和，代表了Y的总变异$

这里的 $R_j$ 是 $X_j~其余自变量X$ 作为Regrion得出, 也就是说想知道Xj的膨胀因子，就把它作为因变量，其余X作为自变量来拟合一个Regression


