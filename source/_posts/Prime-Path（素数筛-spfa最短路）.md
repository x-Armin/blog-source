---
title: Prime Path（素数筛+spfa最短路/bfs）
mathjax: true
copyright: true
date: 2018-08-01 02:04:42
tags: [最短路,素数筛,bfs,图论,SPFA]
categories: 题解
---
# 描述
传送门：[poj-3126](http://poj.org/problem?id=3126)
传送门：[swustoj-2](http://acm.swust.edu.cn/#/problems/2/-1?_k=sosot0)

>&emsp;The ministers of the cabinet were quite upset by the message from the Chief of Security stating that they would all have to change the four-digit room numbers on their offices. 
>* It is a matter of security to change such things every now and then, to keep the enemy in the dark. 
>* But look, I have chosen my number 1033 for good reasons. I am the Prime minister, you know! 
>* I know, so therefore your new number 8179 is also a prime. You will just have to paste four new digits over the four old ones on your office door. 
>* No, it’s not that simple. Suppose that I change the first digit to an 8, then the number will read 8033 which is not a prime! 
>* I see, being the prime minister you cannot stand having a non-prime number on your door even for a few seconds. 
>* Correct! So I must invent a scheme for going from 1033 to 8179 by a path of prime numbers where only one digit is changed from one prime to the next prime. 

<!--more-->

>Now, the minister of finance, who had been eavesdropping, intervened. 
>
>* No unnecessary expenditure, please! I happen to know that the price of a digit is one pound. 
>* Hmm, in that case I need a computer program to minimize the cost. You don't know some very cheap software gurus, do you? 
>* In fact, I do. You see, there is this programming contest going on... Help the prime minister to find the cheapest prime path between any two given four-digit primes! The first digit must be nonzero, of course. Here is a solution in the case above. 
1033
1733
3733
3739
3779
8779
8179
>
>&emsp;The cost of this solution is 6 pounds. Note that the digit 1 which got pasted over in step 2 can not be reused in the last step – a new 1 must be purchased.

## Input
>&emsp;One line with a positive number: the number of test cases (at most 100). Then for each test case, one line with two numbers separated by a blank. Both numbers are four-digit primes (without leading zeros).

## Output
>&emsp; One line for each case, either with a number stating the minimal cost or containing the word Impossible.

## Examples
* intput
```
3
1033 8179
1373 8017
1033 1033
```
* output
```
6
7
0
```

# 题目大意
>* 如果两个四位的素数有且只有一位数字不一样时，这两两个素数可以互相变化成对方。
>* 每组数据给你两个素数，求从一个到另一个素数需要变化的最小次数。

# 思路
>* 用素数筛预处理出一万内所有素数。
>* $O(N^2)$预处理出边，建图完毕。
>* 链式向前星存图。
>* 最后从起点到终点跑一遍spfa，求得最短路，即答案。
>* 用bfs跑也是可以过的。

# 代码
```c++
#include<iostream>
#include<vector>
#include<queue>
#include<string.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a) memset(a,0,sizeof(a))
#define CRL2(a,x) memset(a,x,sizeof(a))
const int mod = 1e9 + 7;
const int N = 1e4+5;

int prime[N] = { 0 }, num_prime = 0;
int isNotPrime[N] = { 1, 1 }, BeginP=-1;
struct node {int To, S, Next;}; //链式向前星存图

vector <node> Edge;
int Begin[N], Inque[N], Dis[N], n;

void GetPrim() {        //线筛
    rep(i,2,N) {
        if (!isNotPrime[i]) {
            prime[num_prime++] = i;
            if (i>1000&&BeginP==-1)
                BeginP = num_prime-1;       //BeginP用来存第一个四位数的，注意是-1这个点wa了好久
        }

        for (int j = 0; j<num_prime&&i*prime[j]<N; j++) {
            isNotPrime[i*prime[j]] = 1;
            if (!(i%prime[j]))
                break;
        }
    }
}

void add(int a, int b) {        //加边
    node tem;
    tem.To = b;
    tem.S = 1;
    tem.Next = Begin[a];
    Begin[a] = Edge.size();
    Edge.push_back(tem);
}

bool Judge(int x, int y) {  //判断是否只有一位不同
    int Flag = 0;
    rep(i, 0, 4) {
        if (x % 10 != y % 10) Flag++;
        x /= 10; y /= 10;
    }
    return Flag==1;
}

void spfa(int s) {//最短路
    rep(i,0,N+1) Dis[i] = 9999999;Dis[s] = 0;
    CRL(Inque);Inque[s] = 1;
    queue<int> Q;Q.push(s);
    while (!Q.empty()) {
        int T = Q.front();
        Q.pop();Inque[T] = 0;
        for (int i = Begin[T]; i != -1; i = Edge[i].Next) {
            if (Dis[Edge[i].To]>Dis[T] + Edge[i].S) {
                Dis[Edge[i].To] = Dis[T] + Edge[i].S;
                Q.push(Edge[i].To);
                Inque[Edge[i].To] = 1;
            }
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    int x, Star, End, T;
    GetPrim();

    CRL2(Begin, -1);
    rep(i,BeginP,num_prime)
        rep(j,i,num_prime)
            if (Judge(prime[i], prime[j])) {
                add(prime[i], prime[j]);
                add(prime[j], prime[i]);
            }
    cin >> T;
    while (T--) {
        cin >> Star >> End;
        spfa(Star);
        if(Dis[End]!=9999999)
            cout << Dis[End] << endl;
        else
            cout << "Impossible" << endl;
    }
    return 0;
}

```
