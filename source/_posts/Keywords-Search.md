---
title: Keywords Search（AC自动机）
mathjax: true
copyright: true
date: 2019-03-21 00:20:37
tags: [AC自动机,字符串]
categories: 题解
---
# 描述
传送门：[hdu-2222](http://acm.hdu.edu.cn/showproblem.php?pid=2222)

>&emsp;In the modern time, Search engine came into the life of everybody like Google, Baidu, etc.
Wiskey also wants to bring this feature to his image retrieval system.
Every image have a long description, when users type some keywords to find the image, the system will match the keywords with description of image and show the image which the most keywords be matched.
To simplify the problem, giving you a description of image, and some keywords, you should tell me how many keywords will be match.

<!--more-->
## Input
> First line will contain one integer means how many cases will follow by.
Each case will contain two integers N means the number of keywords and N keywords follow. (N <= 10000)
Each keyword will only contains characters 'a'-'z', and the length will be not longer than 50.
The last line is the description, and the length will be not longer than 1000000.

## Output
> Print how many keywords are contained in the description.

## Examples
* intput
```c++
1
5
she
he
say
shr
her
yasherhs
```
* output
```c++
3
```

# 题目大意
>* 给你n个模式串，再给你一个匹配串，让你求有多少种模式串在匹配传中出现过。

# 思路
>* AC自动机裸题，先根据给的模式串建一棵trie树，再在上面bfs建失配指针。

# 代码
```c++
#include <stdio.h>
#include <string.h>
#include <queue>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
const int N=5e5+5;

struct AcAuto{      //Ac自动机
    int tr[N][26],End[N]={0},Fail[N],L; //tr trie树   end[i]:以节点i结束的模式串个数

    void Init(){        //初始化
        CRL(End,0);
        CRL(tr,0);
        L=0;
    }

    void insert(char *s,int Len){       //trie树建立
        int now =0;
        rep(i,0,Len){
            int v=s[i]-'a';
            if(!tr[now][v]) tr[now][v]=++L;
            now=tr[now][v];
        }
        End[now]++;
    }

    void build_fail(){          //构建失配指针
        queue<int> Q;
        rep(i,0,26)if(tr[0][i]) Fail[tr[0][i]]=0,Q.push(tr[0][i]);  //使第一层（根为第0层）所有节点的失配指针指向root
        while(!Q.empty()){
            int u=Q.front();Q.pop();
            rep(i,0,26)
                if(tr[u][i]) {          //如果匹配到了，那么失配指针就指向 当前节点的父亲节点  的Fail指针指向的节点 的儿子节点中和当前节点一样的节点
                    Fail[tr[u][i]]=tr[Fail[u]][i];
                    Q.push(tr[u][i]);
                }
                else
                    tr[u][i]=tr[Fail[u]][i];
        }
    }

    int query(char *s){     //查询函数
        int now=0,ans=0,Len=strlen(s);
        rep(i,0,Len){
            now=tr[now][s[i]-'a'];
            for(int t=now;t&&~End[t];t=Fail[t]){
				ans+=End[t];
            	End[t]=-1;      //避免重复计数
			}
        }
        return ans;
    }
}ac;


char buf[1000010];
int main() {
    int T=1,n;
    scanf("%d",&T);
    while( T-- ) {
        ac.Init();
        scanf("%d",&n);
        for(int i = 0; i < n; i++) {
            scanf("%s",buf);
            ac.insert(buf,strlen(buf));
        }

        ac.build_fail();
        scanf("%s",buf);
        printf("%d\n",ac.query(buf));
    }
    return 0;
}

```
