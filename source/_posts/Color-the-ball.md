---
title: Color the ball（差分入门）
mathjax: true
copyright: true
date: 2018-06-20 14:22:44
tags: 差分
categories: 题解
---
# 描述
传送门：[hdu-1556](http://acm.hdu.edu.cn/showproblem.php?pid=1556)

>&emsp;N个气球排成一排，从左到右依次编号为1,2,3....N.每次给定2个整数a b(a <= b),lele便为骑上他的“小飞鸽"牌电动车从气球a开始到气球b依次给每个气球涂一次颜色。但是N次以后lele已经忘记了第I个气球已经涂过几次颜色了，你能帮他算出每个气球被涂过几次颜色吗？

<!--more-->
## Input
> 每个测试实例第一行为一个整数N,(N <= 100000).接下来的N行，每行包括2个整数a b(1 <= a <= b <= N)。
当N = 0，输入结束。

## Output
> 每个测试实例输出一行，包括N个整数，第I个数代表第I个气球总共被涂色的次数

## Examples
* intput
```
3
1 1
2 2
3 3
3
1 1
1 2
1 3
0
```
* output
```
1 1 1
3 2 1
```

# 思路
>* 这道题可以用区间线段树做，但差分的代码更短。
>* 当输入的区间为[a,b]时，让Sum[a]++，表示a以后的都+1，再让Sum[b]--，表示b以后的都-1,相当于是线段树里面的Lazy标记,最后求一下前缀和就行了。

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define CRL(a) memset(a,0,sizeof(a))
const int N=1e5+5;

int main()
{
    std::ios::sync_with_stdio(false);
    int n,a,b,Sum[N];
    while(cin>>n&&n){
        CRL(Sum);
        for(int i=0;i<n;i++){
            cin>>a>>b;
            Sum[a]++;           //差分
            Sum[b+1]--;
        }

        cout<<Sum[1];
        for(int i=2;i<=n;i++) {     //前缀和
            Sum[i]+=Sum[i-1];
            cout<<" "<<Sum[i];
        }
        cout<<endl;
    }
    return 0;
}
```
