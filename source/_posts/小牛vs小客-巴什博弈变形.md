---
title: 小牛vs小客(巴什博弈变形)
date: 2018-02-04 18:15:23
tags: 博弈
categories: 题解
copyright: true
---
# 描述
[题目链接](https://www.nowcoder.net/acm/contest/75/D)
>小牛和小客玩石子游戏，他们用n个石子围成一圈，小牛和小客分别从其中取石子，谁先取完谁胜，每次可以从一圈中取一个或者相邻两个，每次都是小牛先取，请输出胜利者的名字。（1 2 3 4 取走 2 13 不算相邻）

<!--more-->
## Intput
>输入包括多组测试数据
>每组测试数据一个n（1≤n≤10^9）

## Output
>每组用一行输出胜利者的名字（小牛获胜输出XiaoNiu，小客获胜输出XiaoKe）

## Examples
* intput
```
2
3
```
* output
```
XiaoNiu
XiaoKe
```

# 思路
>* 当n<=2时，毫无疑问是先手获胜。
>* 当n>2时，先手拿了之后，石子形状就可以看成一条线，后手可以选择拿1个或2个，使得剩下的石子是对称的，然后无论先手怎样拿，后手总能在对称位置拿到石子，最后一定是后手赢。

# 代码
```c++
#include<iostream>
using namespace std;
int main()
{
    int x;
    while(cin>>x)
     {
        if(x<=2)
         cout<<"XiaoNiu\n";
         else
         cout<<"XiaoKe\n";
      }
    return 0;
}
```