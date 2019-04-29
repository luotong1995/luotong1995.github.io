---
title: 似然和概率的区别
date: 2019-04-23 19:50:52
tags:
categories: 机器学习
统计学
updated: 2019-04-23 21:29:00
---

## 概率(Probability)
概率是对随机变量发生的可能性的度量，是一个在0--1之间的实数，概率也是一有严格的定义的是符合柯尔莫哥洛夫定理的数学对象。可以参考wikipedia[概率](https://en.wikipedia.org/wiki/Probability, '概率')

概率表示在给定$\theta$下随机变量$X=x$的可能性，可以用概率密度函数$f(x|\theta)$来表示，表示在给定$\theta$下$x$的概率密度，这里$x$表示随机变量$X$所取的值，即$X=x$。

## 似然(Likelihood)
似然也叫做似然函数，似然函数的表达式为$L(\theta|x)$，也是一个条件概率或者条件分布的形式，似然函数表示在已知随机变量$X=x$的条件下，估计参数$\theta$的值，所以似然函数是一个关于$\theta$的函数。

## 概率和似然的联系
- 假设$X$式离散随机变量，则其概率密度函数$f(x|\theta)$可以写为$f(x|\theta)=\mathbb{P}_{ \theta }\left( X=x \right)$，表示在参数$\theta$下随机变量$X$取到值$x$的可能性。如果发现
  $$L(\theta_1 | \textbf{x} ) = \mathbb{P}_{\theta_1}(\textbf{X} = \textbf{x}) > \mathbb{P}_{\theta_2}(\textbf{X} = \textbf{x}) = L(\theta_2 | \textbf{x})$$

那么似然函数就得到如下推测：在参数$\theta_1$下随机向量$X$取到值$x$的可能性大于在参数$\theta_2$下随机向量$X$取到值$x$的可能性。换句话说，我们更有理由相信(相对于$\theta_2$来说)$\theta_1$，更有可能是真实值。(这里的可能性由概率来刻画)

- 如果$X$是连续的随机向量，那么其密度函数$f(x|\theta)$本身（如果在$x$连续的话）在$x$处的概率为0，为了方便考虑一维情况：给定一个充分小$\epsilon > 0$，那么随机变量X取值在$(x - \epsilon, x + \epsilon)$区间内的概率即为

$$\mathbb{P}_\theta(x - \epsilon < X < x + \epsilon) = \int_{x - \epsilon}^{x + \epsilon} f(x | \theta) dx \approx 2 \epsilon f(x | \theta) = 2 \epsilon L(\theta | x)$$
  
综上，概率(密度)表达给定$\theta$下样本随机向量$X = x$的可能性，而似然表达了给定样本$X = x$下参数$\theta_1$(相对于另外的参数$\theta_2$)为真实值的可能性，由于该参数不是随机变量所以只能用可能性来描述，而不是用概率来描述。
  


