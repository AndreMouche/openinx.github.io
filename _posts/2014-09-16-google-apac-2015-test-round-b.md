---
layout: post
title: "Google2015校招笔试 Round B"
description: ""
category:
tags: [ algorithm ]
---

昨天做了下Google在线校招笔试，算法一天不做题，水平就擦擦往下掉。 

### Poblem A. Password Attacker 

* 描述

问由N个不同字符组成的长度为M的密码串有多少个？ 其中对每个密码串所有的N个不同字符都必须出现过. 

* 答案1 Brute Force

下面方程的每一组解作全排列之后的所有计数累加，就是答案。假设有一组解为X1,...,Xn,那么该组解的排列之后有 M!/(X1! * X2! * ... * Xn!)，所有解累加即答案。 

```
sigma(Xi) = M , Xi >= 1 且 1<=i<=N

```

M<=15的小数据可以通过DFS过掉，但是M<=100的大数据无法过掉。 


* 答案2 DP 

dp[i,j]表示从N中字符中选择j种不同字符组成的长度为i的密码串的个数。 那么所求答案为dp[M, N]. 递推式为: 

```
dp[0, 0] = 1
dp[0, i] = 0 ( 1<=i<=M )
dp[i,j] = dp[i-1, j] * j + dp[i-1, j-1] * (n - (j-1))
```
其中dp[i-1,j-1] * (n - j + 1) 代表前面i-1个密码串只用了j-1个字符，那么第i个密码可以从剩余的n-(j-1)个字符总任选一个。

* 答案3 [第二类stirling数](http://en.wikipedia.org/wiki/Stirling_numbers_of_the_second_kind)

第二类stirling数的意义是: 将n个不同的元素分成k个等价类, 记为S(n,k)， 递推式为: 

```
S(n,k) = S(n-1, k-1) + S(n-1, k) * k
```

将M个密码分成N个等价类，但是等价类之间是有区别的，所以需要对N个等价类进行一次排列。 总数为`N! * S(M, N)`


### Problem B. New Years Eve

* 描述

有一堆250容量的杯子，第1层有1个杯子，第2层有3个杯子，第3层有10个杯子...第N层有N*(N+1)/2个杯子。 第i层的标号为j的杯子，由放在第i+1层的标号为j, j+i, j+i+1这三个杯子之上。当第i层的杯子的酒满了时，会等分的溢出到第i+1层的j,j+i,j+i+1这三个杯子里。 问当在顶层往被子里灌B瓶750的酒时，第L层的第N个被子的酒量会是多少？ 

* 答案

模拟题， 没啥好说的。 

### Problem C. Card Game 

* 描述

给一组长度为N的数列 A1, A2, ... , An 和一个整数K 。 给出这种操作： 每次从数列中选出连续的3个公差为K的等差数列，将这个三元组从数列中删除掉。 问怎样操作使得最后剩下的数列元素个数最少? 输出最少个数.

* 答案 DP 

dp[i, j]表示对数列Ai,...Aj这一段进行若干次上述操作之后，剩余的最少元素个数。 所求答案为dp[1,n]。 

考虑以下几种情况： 

1. 假设Ai这个元素剩下了，就是`dp[i+1, j] + 1`: 
2. 假设Aj剩下了，就是`dp[i,j -1] + 1`; 
3. 假设Ai, Aj都剩下了就是`dp[i+1,j-1] + 2` ; 
4. 假设Ai, Aj都被拿掉的情况, `Ai...Am...Aj`这三个元素被拿掉了，那么就是`dp[i+1,m-1] + dp[m+1,j-1] (i<m<j)` ；  
   或者被分成两端`Ai..Am`, `Am+1...Aj`这两端分别被拿掉了，那么就是`dp[i+1, m] + dp[m+1, j]`


### Problem D. Parentheses Order 

* 描述

给出一个N和一个数K , 求在所有N对括号的匹配字符串中，按照字典序列从小到大的第K小的括号匹配字符串是多少? 

* 答案 DP 

转化成一个计数问题： 在确定前缀为j个连续左括号，且有i个剩余括号对可供选择的情况下， 符合括号匹配的字符串总数为t(n, k)。 例如`((((*)*)*)*)`，有4个连续左括号, `*`表示0个或者多个匹配的括号对，所有`*`的括号对的总数为10, 那么对应的状态为t(10,4)。 递推式为: 

```
t(n, k ) = sigma( h(i) * t(n - i, k - 1 ) )  ( 0<=i <= n) 
```
其中h(i)为[Catalan数](http://en.wikipedia.org/wiki/Catalan_number)， 对应的递推式为: 

```
h(0) = h(1) = 1 
h(n) = sigma(h(i) * h(n-1-i))   (0<=i<n)
```

