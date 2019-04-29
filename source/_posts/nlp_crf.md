---
title: Conditional Random Fields论文阅读和理解
date: 2019-04-22 14:50:52
tags:
categories: NLP
updated: 2019-04-22 16:29:00
---

## CRF
### 概率模型
#### 朴素贝叶斯
朴素贝叶斯算法是一个用来分类的生成式模型，也可以说是一种概率模型。找出联合概率分布，然后通过贝叶斯公式求出后验概率。
假设所有的特征是条件独立分布(conditionally independent)的，是一种生成式模型。



#### 隐马尔可夫模型（Hidden Markov Model），

- 联合概率分布（joint probability distribution.）指的是包含多个条件且所有条件同时成立的概率，记作P(X=a,Y=b)或P(a,b)，有的书上也习惯记作P(ab)，但是这种记法个人不太习惯，所以下文采用以逗号分隔的记法。

#### Maximum Entropy Model(最大熵模型)