---
title: ACboy needs your help(分组背包)
date: 2018-02-16 15:28:32
tags: [动态规划,背包问题]
categories: 题解
copyright: ture
mathjax: ture
---
# 描述
传送门：[hdu-1712](http://acm.hdu.edu.cn/showproblem.php?pid=1712)

>ACboy has N courses this term, and he plans to spend at most M days on study.Of course,the profit he will gain from different course depending on the days he spend on it.How to arrange the M days for the N courses to maximize the profit?

<!--more-->
## Input
>The input consists of multiple data sets. A data set starts with a line containing two positive integers N and M, N is the number of courses, M is the days ACboy has.
Next follow a matrix A[i][j], (1<=i<=N<=100,1<=j<=M<=100).A[i][j] indicates if ACboy spend j days on ith course he will get profit of value A[i][j].
N = 0 and M = 0 ends the input.

## Output
>For each data set, your program should output a line which contains the number of the max profit ACboy will gain.

## Examples
* intput
```
2 2
1 2
1 3
2 2
2 1
2 1
2 3
3 2 1
3 2 1
0 0
```
* output
```
3
4
6
```

# 思路
>* 这是一个典型的分组背包问题，我们可以转换成01背包求解。
>* 核心代码

```c++
for(int i=1;i<=n;i++)
   for(int j=V;j>=0;j--)
     for(int k=0;k<=V;k++)	//转01背包，注意三重循环顺序
       if(j>=k)
        dp[j]=max(dp[j],dp[j-k]+map[i][k]);
```