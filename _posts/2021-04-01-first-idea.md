---
title: 红白球的概率
author: Chunyu
layout: post
categories: [ideas]
---
# 问题一
有一个盒子，里面有一个黑球和一个白球。每次随机取出盒子中的一个球，并将两个与取出球同色的球放进盒子（就是随机一种颜色将其个数+1）。求n次取球后，盒子中白色球数目的期望。

可采用动规+数学归纳法，用$dp[i][j]$表示第i次取球后盒子中有j个白球的概率。
- 当i为0时，总共有2个球，
    $$\left\{
    \begin{aligned}
    dp[0][0] &= 0\\
    dp[0][1] &= 1\\
    dp[0][2] &= 0
    \end{aligned}
    \right.
    $$
- 当i为1时，总共有3个球
    $$\left\{
    \begin{aligned}
    dp[1][0] &= 0\\
    dp[1][1] &= \frac{1}{2}\\
    dp[1][2] &= \frac{1}{2}\\
    dp[1][3] &= 0
    \end{aligned}
    \right.
    $$
- 设当i为k-1时，总共有$k+1$个球
    $$\left\{
    \begin{aligned}
    dp[k-1][0] &= 0\\
    dp[k-1][j] &= \frac{1}{2},j=1,2,\cdots,k\\
    dp[k-1][k+1] &= 0
    \end{aligned}
    \right.
    $$
- 当i为k时，总共有$k+2$个球，$dp[k][0]=0，dp[k][k+2]=0$，
    $$
    \begin{aligned}
    dp[k][1] &= dp[k-1][j]*\frac{k}{k+1}\\
             &= \frac{1}{k}*\frac{k}{k+1}\\
             &= \frac{1}{k+1}.\\
    \end{aligned}
    $$
    $$
    \begin{aligned}
    dp[k][j] &= dp[k-1][j-1]*\frac{j-1}{k+1} + dp[k-1][j]*\frac{k-j+1}{k+1}\\
             &= \frac{1}{k}*\frac{j-1}{k+1} + \frac{1}{k}*\frac{k-j+1}{k+1}\\
             &= \frac{1}{k+1}.\\
           j &=1,2,\cdots,k
    \end{aligned}
    $$
    $$
    \begin{aligned}
    dp[k][k+1] &= dp[k-1][k]*\frac{k}{k+1}\\
             &= \frac{1}{k}*\frac{k}{k+1}\\
             &= \frac{1}{k+1}.\\
    \end{aligned}
    $$
综上所述，取第n次球时，箱内的有$j$个白球概率为$1/(n+1)$，其中$j=1,2,\cdots,n+1$。当$j$为0或$n+2$时，概率为0。可求此时白球个数的期望为：
$$
\begin{aligned}
E(num) &= \frac{(1+n+1)*n+1}{2}*\frac{1}{n+1}\\
       &= \frac{n+2}{2}
\end{aligned}
$$

# 问题二
有一个盒子，里面有$n$个白球。每次随机取出盒子中的一个球，如果是白球就放回红球，如果是红球就直接放回。求n次取球后，盒子中红球数目的期望。

该题依旧可以采用动规求解，但难以通过数学归纳法得到一个通向公式。随即我尝试用隐马尔可夫链表示这个问题。

考虑有$n+1$个状态，状态$i$表示有$i-1$个红球，$n-i+1$个白球。

初始状态有0个红球，$n$个白球，此时红球的概率分布为：
$$
 \pi = 
 \begin{bmatrix}
   1 & 0 & 0 & \cdots & 0\\
  \end{bmatrix}^T
$$
状态之间的转移矩阵为：
$$
 A = 
 \begin{bmatrix}
   0 & 1 & 0 & 0 & \cdots & 0 & 0\\
   0 & \frac{1}{n} & \frac{n-1}{n} & 0 & \cdots & 0 & 0\\
   0 & 0 & \frac{2}{n} & \frac{n-2}{n} & \cdots & 0 & 0\\
   \vdots & \vdots & \vdots & \ddots & \ddots & \vdots & \vdots\\
   0 & 0 & 0 & \cdots & \frac{n-2}{n} & \frac{2}{n} & 0\\
   0 & 0 & 0 & 0 & \cdots & \frac{n-1}{n} & \frac{1}{n}\\
   0 & 0 & 0 & 0 & \cdots & \vdots & 1\\
  \end{bmatrix}
$$
对于每一个状态，红球的数量为（观测矩阵）：
$$
 B = 
 \begin{bmatrix}
   0 & n \\
   1 & n-1 \\
   2 & n-2 \\
   \vdots & \vdots\\
   n & 0\\
  \end{bmatrix}
$$
问题变为了求$\pi^TA^nB$。因为$A$为上三角矩阵且对角线元素各不相同，因此$A$可对角化，可以分解为$Q^{-1}\Sigma Q$，其中$Q$为特征向量组成的矩阵，$\Sigma$为特征值组成的对角矩阵。
$$
\begin{aligned}
\pi^TA^nB &= \pi^T(Q^{-1}\Sigma Q)^nB\\
          &= \pi^TQ^{-1}\Sigma ^n QB.
\end{aligned}
$$
$$
\begin{aligned}
E(num) &= \pi^TQ^{-1}\Sigma ^n QB[0].
\end{aligned}
$$
