---
title: 概率、似然、先验概率、后验概率、贝叶斯公式、极大似然估计、最大后验概率
date: 2019-04-23 19:50:52
tags:
categories: 机器学习
mathjax: true
updated: 2019-04-23 21:29:00
---

## 概率(Probability)
概率是对随机变量发生的可能性的度量，是一个在[0-1]之间的实数，概率也是一有严格的定义的是符合柯尔莫哥洛夫定理的数学对象。可以参考wikipedia[概率](https://en.wikipedia.org/wiki/Probability, '概率')。

概率表示在给定$\theta$下随机变量$X=x$的可能性，可以用概率密度函数$f(x|\theta)$来表示，表示在给定$\theta$下$x$的概率密度，这里$x$表示随机变量$X$所取的值，即$X=x$。

## 似然(Likelihood)
似然也叫做似然函数，似然函数的表达式为$L(\theta|x)$，也是一个条件概率或者条件分布的形式，似然函数表示在已知随机变量$X=x$的条件下，估计参数$\theta$的值，所以似然函数是一个关于$\theta$的函数。

## 概率和似然的联系
概率和似然是对于概率密度函数的不同角度的描述，都是表示事件发生的概率。

- 假设$X$式离散随机变量，则其概率密度函数$f(x|\theta)$可以写为$f(x|\theta)=\mathbb{P}_{ \theta }\left( X=x \right)$，表示在参数$\theta$下随机变量$X$取到值$x$的可能性。如果发现
  $$L(\theta_1 | \textbf{x} ) = \mathbb{P}_{\theta_1}(\textbf{X} = \textbf{x}) > \mathbb{P}_{\theta_2}(\textbf{X} = \textbf{x}) = L(\theta_2 | \textbf{x})$$

那么似然函数就得到如下推测：在参数$\theta_1$下随机向量$X$取到值$x$的可能性大于在参数$\theta_2$下随机向量$X$取到值$x$的可能性。换句话说，我们更有理由相信(相对于$\theta_2$来说)$\theta_1$，更有可能是真实值。(这里的可能性由概率来刻画)

- 如果$X$是连续的随机向量，那么其密度函数$f(x|\theta)$本身（如果在$x$连续的话）在$x$处的概率为0，为了方便考虑一维情况：给定一个充分小$\epsilon > 0$，那么随机变量X取值在$(x - \epsilon, x + \epsilon)$区间内的概率即为:

$$\mathbb{P}_\theta(x - \epsilon < X < x + \epsilon) = \int_{x - \epsilon}^{x + \epsilon} f(x | \theta) dx \approx 2 \epsilon f(x | \theta) = 2 \epsilon L(\theta | x)$$

并且两个未知参数的情况下做比就能约掉$2\epsilon$，所以和离散情况下的理解一致，只是此时似然所表达的那种可能性和概率$f(x|\theta) = 0$无关。

综上，概率(密度)表达给定$\theta$下样本随机向量$X = x$的可能性，而似然表达了给定样本$X = x$下参数$\theta_1$(相对于另外的参数$\theta_2$)为真实值的可能性，由于该参数不是随机变量所以只能用可能性来描述，而不是用概率来描述。其实$L(\theta|\textbf{x})$和$f(\textbf{x}|\theta)$是对于事件从两种角度进行分析，都是表示的事件发生的概率。$f(\textbf{x}|\theta)$表示对于给定的$\theta$条件下，$x$出现的可能性是多大。而$L(\theta|\textbf{x})$表示在给定样本x的时候，哪个$\theta$使得$x$出现的可能性是多大。

## 先验概率(Prior probability)
先验概率就是根据现在目前的经验能够得到的最多的信息。下面举出两个例子作出解释。
### 投掷骰子 
比如说目前投掷一个骰子每一面向上的概率都是$\frac{1}{6}$，这就是一个先验证概率。
### 选水果
假设目前有两个盒子，一个红色的，一个蓝色的，红色盒子中有2个苹果6个橘子，蓝色盒子中有3个苹果和1个橘子。现在假定我们随机选择一个盒子，从这个盒子中我们随机选择一个水果，观察一下选择了哪种水果，然后放回盒子中。假设我们重复这个过程很多次。假设我们在40%的时间中选择红盒子，在60%的时间中选择蓝盒子，并且我们选择盒子中的水果时是等可能选择的。

在这个例子中，我们要选择的盒子的颜色是一个随机变量，记作B。这个随机变量可以取两个值中的一个，即r(对应红盒子)或b(对应蓝盒子)。类似地，水果的种类也是一个随机变量，记作F。它可以取a(苹果)或者o(橘子)。我们把一个事件的概率定义为事件发生的次数与试验总数的比值，假设总试验次数趋于无穷，则$p(B= r)=\frac{4}{10} = \frac{2}{5}$和$p(B=b)=\frac{6}{10}=\frac{3}{5}$，现在有问题1:根据现在的条件如果选出的水果是橘子，那么这个水果最有可能出自于哪个盒子。

在这个问题中选择盒子的概率$p(B)$即为先验概率，因为这个是已经知道的，选择红色的和蓝色的盒子的概率就是已知的。
## 后验概率(Posterior probability)
后验概率是事件已经发生，要求这个事件发生的原因是由某个因素引起的可能性的大小。

问题1中，在知道取出来的水果是橘子的时候，要求这个橘子是属于哪个盒子$p(B|F)$的时候就是在求一个后验概率，因为已经$F$事件已经发生。

## 贝叶斯公式

$$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$$

## 极大似然估计(Maximum Likelihood Estimation, MLE)
在统计学中，极大似然估计是一种估计统计模型参数的方法，意思就是根据样本数据估计模型参数的方法。按照字面意思就是找出参数$\theta$来似然函数$L(\theta|\textbf{x})$最大，其中使得其最大的参数$\hat {\theta}$就是极大似然估计，可以由如下表达式表示：
$${\displaystyle {\hat {\theta \,}}\in \{\underset {\theta \in \Theta }{\operatorname {arg\,max} }\ {\mathcal {L}}(\theta \,;x)\}}$$
or
$${\displaystyle {\hat {\theta }}_{\mathrm {MLE} }(x)={\underset {\theta }{\operatorname {arg\,max} }}\ f(x\mid \theta )\!}$$

通常情况下对于使用对数似然进行求解，因为对数似然是一个严格递增函数
$${\displaystyle \ell (\theta \,;x)=\ln {\mathcal {L}}(\theta \,;x)}$$
### 求解过程
假设$X$是独立同分布的数据，已知$X$的数据集大小为$m$，则似然函数可以有如下描述：
$$L(\theta|x)=f(x|\theta)=f(x_1,x_2,...,x_m|\theta)=\prod _{ i=1 }^{ m }{f\left( X={ x }_{ i }|\theta\right)} $$
使用对数似然进行求解，将乘积转化为求和
$$\log{L(\theta|x)}=\sum_{ i=1 }^{ m }{\log{f\left( X={ x }_{ i }|\theta\right)}}$$
则极大似然估计为：
$${\displaystyle {\hat {\theta \,}}\in \{\underset {\theta \in \Theta }{\operatorname {arg\,max} }\ \log{L(\theta|x)}\}}=\{\underset {\theta \in \Theta }{\operatorname {arg\,max} }\sum_{ i=1 }^{ m }{\log{f\left( X={ x }_{ i }|\theta\right)}}\}$$

### 极大似然估计举例说明
#### 例子
假设袋子里面有若干个球，里面只有黑球和白球两种，从里面放回的依次取出来10次，其中出来的7次是黑球、3次是白球，那么认为袋子里面黑球的比例为0.7。这个参数是通过极大似然估计求出来的。其实这是一个典型的二项分布，并且已知部分样本数据，可以根据极大似然估计求出二项分布的参数。假设二项分布黑球的概率为$p$，则白球的概率为$1-p$，可以得到似然函数为：
$$L(p|x)=\prod_{i=1}^{10}f\left(x_i|p\right)=p^7(1-p)^3$$

使用对数似然求出使得似然函数最大的参数$p$：
$$\log{L(p|x)}=\sum_{ i=1 }^{ 10 }{\log{f\left( X={ x }_{ i }|p\right)}}=7\log{p}+3\log{(1-p)}$$
对上面表达式求导得到：
$$L^{'}(p|x)=\frac { 7 }{ x } + \frac{3}{x-1} $$
令倒数等于零得到参数$p=0.7$即为极大似然估计的值。

## 最大后验概率估计(Maximum a Posteriori estimation，MAP)
根据最大似然估计可以知道，根据已知样本数据$X$估计出模型的参数$\theta$，这是一个求极大似然估计的过程，最大似然估计的公式如下所示：
$${\displaystyle {\hat {\theta }}_{\mathrm {MLE} }(x)={\underset {\theta }{\operatorname {arg\,max} }}\ f(x\mid \theta )\!}$$
在求极大似然估计的过程中，认为模型参数$\theta$不是一个随机变量，现在假设$\theta$是一个随机变量，且存在着先验概率$P(\theta)$，然后使用贝叶斯公式计算后验概率：
$$P(\theta|X) = \frac{P(X|\theta)P(\theta)}{P(X)}$$

其中$P(X|\theta)$为似然函数，假设随机变量$\theta$的概率密度函数为$g(\theta)$，由于随机变量$X$是一个已经发生的事件，现在$P(X)$中的值不受$\theta$的影响，所以$P(X)$所以这个后验概率的求解过程不受$P(X)$的影响。所以使用概率密度函数的方式可以得到:

$${ \hat { \theta } }_{MAP}(x) \ \  ={\underset{\theta}{arg\,max}}P(\theta \mid X)\\ \qquad \qquad= { \underset { \theta  }{ { arg\, max} } }f(\theta \mid x) \\ \ \ \  \qquad \qquad \qquad \qquad = { \underset { \theta  }{ { arg\, max } }  }{ \frac { f(x\mid \theta )\, g(\theta ) }{ \int _{ \Theta } f(x\mid \vartheta )\, g(\vartheta )\  d\vartheta }} \\ \qquad \qquad \qquad= { \underset {\theta  }{ { arg\, max } }f(x\mid \theta )\, g(\theta )} $$

最大后验概率的做法可以认为极大似然估计乘上一个先验证概率，也就是相当于做了一个乘法的正则化。但是当一个随机变量的样本数量接近无数次，则最大后验概率的结果会更加接近极大似然估计的值。

其实极大似然估计和最大后验概率分别代表两个学派，一个是频率学派另一个是贝叶斯学派。

## 参考文献

- Bishop C M. Pattern recognition and machine learning[M]. springer, 2006.
- [知乎专栏](https://zhuanlan.zhihu.com/p/46737512, '知乎专栏')
- [Prior Probability](https://en.wikipedia.org/wiki/Prior_probability, 'Prior Probability')
- [Posterior Probability](https://en.wikipedia.org/wiki/Posterior_probability, 'Posterior Probability')
- [Likelihood]('https://en.wikipedia.org/wiki/Likelihood_function', 'Likelihood')
- [Maximum likelihood estimation](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation, 'Maximum likelihood estimation')
- [Maximum a Posteriori estimation](https://en.wikipedia.org/wiki/Maximum_a_posteriori_estimation, 'Maximum a Posteriori estimation')