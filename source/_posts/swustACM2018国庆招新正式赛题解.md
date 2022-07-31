---
title: swustACM2018国庆招新正式赛题解
mathjax: true
copyright: true
date: 2018-10-05 10:31:16
tags: 比赛题解
categories: 题解
---
这次题其实不难，比较偏思维。
按照出题人的预期，题目难度分布是这样的：

|难度|培训来了的都能A|签到题|中等题|防AK|
|-|-|-|-|-|
|题号|A  D|E  I|B  J  F|C  H|

结果，看看榜...

<!--more-->

# A.我还真是个天使呢 
{% tabs A %}
<!-- tab 思路 -->
{% note default %}
天使出题人给的签到题，注意单词之间的两个空格。
{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目连接](http://scpc.openjudge.cn/2018newchong/A/)
## 描述
> 输入一个整数x，判断是奇数还是偶数。
如果是奇数就输出"It  is  odd!",否则"It  is  even!"
### 输入
> 输入一个整数x

### 输出
> 如题

### 样例输入
{% codeblock lang:cpp %}
2
1
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
It  is  even!
It  is  odd!
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}
#include<stdio.h>
int main(){
    int x;
    scanf("%d",&x);
    puts(x%2? "It  is  odd!\n":"It  is  even!\n");
    return 0;
}
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***

# B.探丸蓝月 
{% tabs B %}
<!-- tab 思路 -->
{% note default %}
没啥坑，字符串用得熟一点就能A。
用一个变量记录当前匹配的是"tanwanlanyue"的第几个字母，匹配了就+1。
{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目链接](http://scpc.openjudge.cn/2018newchong/B/)
## 描述
> 死库水学长才从五食堂吃完晚饭出来，就有两个人对他说：我系轱天乐,我四渣渣辉,探丸蓝月，介四里没有丸过的船新版本,挤需体验三番钟,里造会干我一样,爱象介款游戏。于是死库水学长回去试玩了三分钟后，发现这款游戏好玩无比，只需一刀就可以999级。然而三分钟试玩结束，必须充钱购买激活码才能继续玩。然而死库水学长很穷，但是他发现激活码只需要满足以下条件就可以成功激活，继续享受一刀999级的快感。死库水学长很笨，作为他的可爱的学弟学妹们，你们可以帮他判断一下他构造的激活码能成功激活该游戏吗？

> 条件如下：这里有一个全为小写字母组成的激活码s,长度为n（1<=n<=10000），.现在，我们可以将此激活码s中的一些字母任意进行删除，使得剩下的激活码可以是“tanwanlanyue”,那么就可以成功激活，否则就不能成功激活。
### 输入
> 第一行，为一个整数n（1<=n<=10000），第二行为激活码字符串s。

### 输出
> 如果可以成功激活则输出"YES"，否则就输出"NO"。

### 样例输入
{% codeblock lang:cpp %}
12
tanwanlanyue
15
wantanlanyueyue
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
YES
NO
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}
#include<bits/stdc++.h>
using namespace std;
const int N=1e4+5;
char S[N],T[]="tanwanlanyue";

int main()
{
    scanf("%d",&n);
    getchar();gets(S);
    int Ti=0,Si=0,LenS=strlen(S),LenT=strlen(T);//Si是当前的S所要判断的位置，Ti是当前的T所要判断的位置。

    while(Si<LenS&&Ti<LenT){
        if(T[Ti]==S[Si]) Ti++;  //如果相等，匹配T的下一个位置
        Si++;                   //匹配S的下一个位置
    }
        
    puts(Ti==LenT?"YES":"NO");

    return 0;
}
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***
# C.左学姐的骨牌 
{% tabs C %}
<!-- tab 思路 -->
{% note default %}
思维题。
> 一般同学们都能想到模拟的做法：用一个数组来存，每次输入一个L,R，就
```C
for(int i=L;i<=R;i++)
    sum[i]++;
```
> 最后查询摆放多米诺骨牌的最左端到最右端是否有为0的地方。但是魔鬼出题人卡掉了这种做法，这样做会T。

> 正解是用一个数组，当输入的区间为[l,r]时，让sum[l]++，表示l以后的都+1，再让sum[r]--，表示r以后的都-1,最后求一下前缀和就行了。
{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目链接](http://scpc.openjudge.cn/2018newchong/C/)
## 描述
> 这一天，左学姐的室友都去上课了，她一个人实在无聊，于是在宿舍里玩起了多米诺骨牌。由于自己放骨牌过于麻烦，左学姐在某宝买了一个骨牌放置机。

> 只要左学姐给这个神奇的小东西两个数字L、R，它就可以在l到r这一段区间上放上一排骨牌（包括l和r，如果某个位置上已经有了一个骨牌则不会再放）。经过一系列

> 操作后，左学姐完成了骨牌的搭建并推到了第一块骨牌，每块骨牌会击倒后面与它距离为1的骨牌，如果所有的骨牌全部倒下，那么左学姐就能顺利完成这一次游戏。

> 机智的你能帮左学姐看看她搭建的多米诺骨牌能顺利完成吗？
### 输入
> 第一行一个n，表示有n个操作。接下来n行，每一行两个数L,R。（n<=200,000,1<=L<=R<=200,000）

### 输出
> 一行，左学姐能顺利完成此次游戏则输出"Yes!"，否则输出"No!"。

### 样例输入
{% codeblock lang:cpp %}
3
1 1
2 2
3 3
2
1 3
5 7
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
Yes!
No!
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}
#include<bits/stdc++.h>
using namespace std;
const int N=2e5+5;
int n,a[N]={0},l,r,L=200005,R=0,sum=0,Flag=0;

int main()
{
    scanf("%d",&n);
    while(n--){
        scanf("%d%d",&l,&r);
        a[l]++;a[r+1]--;        //表示l以后的都+1,r以后的都-1
        L=min(L,l);R=max(R,r);  //记录最左边和最右边
    }

    for(int i=L;i<=R;i++) { //求前缀和
        a[i]+=a[i-1];       //求前缀和
        if(a[i]==0){        //如果有一个没放骨牌的就说明不成功，跳出
            Flag=1;
            break;
        }
    }

    puts(Flag?"No!":"Yes!");
    return 0;
}

{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***
# D.这真的是真的签到题
{% tabs D %}
<!-- tab 思路 -->
{% note default %}
送分题啊，你们怎么就不自己写呢？
等差求和，注意数据范围是long long。
{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目链接](http://scpc.openjudge.cn/2018newchong/D/)
## 描述
> 啊啊啊，签到题太难出了啊，要是全场爆零多尴尬啊。这是一道送分题，防止大家A不出题。题目很简单，求整数1到n的和。不会写怎么办？算了吧，既然是送分题，我就帮你们写好吧，下面是代码：
```C++
#include<stdio.h>
int main(){
    int n, sum = 0;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++)
        sum = sum + i;
    
    printf("%d\n", sum);
    return 0;
}
```
### 输入
> 一行，一个整数n （1<=n<=10,000,000）

### 输出
> 一个整数，即整数1到n的和。

### 样例输入
{% codeblock lang:cpp %}
3
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
6
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}
#include<bits/stdc++.h>
using namespace std;

int main(){
    long long n;
    scanf("%lld",&n);
    printf("%lld\n",(1+n)*n/2);
    return 0;
}
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***
# E.拯救左学姐
{% tabs E %}
<!-- tab 思路 -->
{% note default %}
魅惑死的也是杀死的啊。
然后答案就是 $2*n+1-n/(k+1)$， $n(K+1)$ 就是只能杀一个的开枪数。
{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目链接](http://scpc.openjudge.cn/2018newchong/E/)
## 描述
> 学姐由于过度劳累而休克死亡，永远的离开了我们。然而作为左学姐的男朋友死库水学长被告知只要到达瓦罗兰大地去寻找快乐风男，就能获得召唤水晶，使左学姐复活。然而在过程中，死库水学长会碰到洛克萨斯的战士。

> 这天，死库水学长来到了瓦罗兰大地，突然一个战士向死库水学长袭来，死库水学长二话不说直接魅惑死了这个战士，之后成功的获得了战士遗留下的火箭枪（弹药有无限）。死库水学长是个神枪手，他每次开枪都能把两个战士给干掉，但是他每开k枪后下一枪只能杀死一个人，然后下一枪又恢复到正常一枪杀死两个人。最后他终于获得了召唤水晶救活了左学姐并过上了幸福的生活。由于兴奋，死库水只知道自己开了n枪，但不知道杀了多少战士，你能帮帮死库水学长算算他杀了多少战士吗？
### 输入
> 第一行，为两个整数n，k（|n|<=10^16，1<=k<=100），n表示开枪数，k表示每开k枪后只能杀一个人。

### 输出
> 当输入的n不合法时输出"Impossible",否则输出死库水学长杀了多少人。

### 样例输入
{% codeblock lang:cpp %}
-1 1
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
Impossible
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}
#include<bits/stdc++.h>
using namespace std;

long long n,k;

int main(){
    scanf("%lld%lld",&n,&k);
    n>=0? printf("%lld\n",2*n+1-n/(k+1)):puts("Impossible");
    return 0;
}

{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***
# F.比武（yan）招亲
{% tabs F %}
<!-- tab 思路 -->
{% note default %}
思维题。
很明显，无论怎么分，整个数列最大值一定是一个对的最大值。
分两种情况讨论：
> * 最大值被分到了前面一个组，那么无论怎么分，后面的那个组的最大值 $x$ 一定 $x<=a[n]$ ,那么很明显当 $x==a[n]$ 时，有最优解，此时只有把a[n]单独分成一组。
> * 最大值被分到了后面一个组一样的，当 $x==a[1]$ 时，有最优解;
然后答案就是
<font size=5 >$$ans = Max-min(a[1],a[n])$$</font>

> 这本来是一道中等题，没想到起到了防AK的作用（18无人AC）。

{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目链接](http://scpc.openjudge.cn/2018newchong/F/)
## 描述
> 死库水学长实在是太优秀了，追他的女生排了长长的一条队，死库水学长虽然表面一副很高冷的样子，实际上他已经早早地暗地里调查过这些女生并得到了她们的能力（yan）值。但是他还是要假装选拔一下，他这样把一女生分成两队：
>
>    女生们排好队，然后他选定一个女生，这个女生和她之前的所有女生一队，后面的一队。
>    
>    分好之后，每队的最高能力值就是这个队的能力值，能力值较高的那个队获胜。
>    
> 但是！死库水学长有一点心理变态，他最喜欢看别人被虐，所以他希望这两个队的能力值之差最大，这样他就会特别快乐。天噜啊，帮帮这个学长找出这个最大的能力值之差吧。


### 输入
> 第一行一个整数n （2<=n<=1,000,000）
第二行n个非负整数，代表排好队后对应的女生的能力值。

### 输出
> 一行，一个整数，即题目中所提到的最大能力值之差。

### 样例输入
{% codeblock lang:cpp %}
3
1 3 0
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
3
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}
#include<bits/stdc++.h>
using namespace std;
const int N=1e6+5;

int main(){
    int Star,n,Max=0,x;//Star:第一个数  x:当前输入数  Max: 数列最大值
    scanf("%d%d",&n,&Star);
    Max=Star;

    for(int i=1;i<n;i++){
        scanf("%d",&x);
        Max=max(Max,x); //更新最大值
    }

    printf("%d\n",Max-min(Star,x));//x现在是最后一个数

    return 0;
}
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***
# G.彩灯游戏
{% tabs G %}
<!-- tab 思路 -->
{% note default %}
证明有点麻烦，比赛的时候肯定不会想着去证明，这种题拿到了可以先暴力输出1-100的答案看一下，用下面这个代码：
```c++
#include<stdio.h>
int main() {
    int a[105]={0};     //a[i]表示编号位i的灯被开关了几次
    for(int i=1;i<=100;i++)
        for(int j=1;j<=i;j++)
            if(i%j==0)      //如果i被j整除，说明它被开关了一次。
                a[i]++;
    int sum=0;
    for(int i=1;i<=100;i++){
        if(a[i]%2)          //如果是奇数就代表最后这个灯是开的
            sum++;
        printf("%d:  %d\n",i,sum);
    }

    return 0;
}
```

观察输出可以发现答案是这样的：
![](http://wx1.sinaimg.cn/mw690/005ZgyPegy1fvxs86bn1qj30rz02vwec.jpg)
然后我们求n的时候落在哪个ans里，由于上面是个数，那么我们对第一行的等差数列求和，即 $S_n= \frac {(3+[3+2 \*(x-1)]) \*n} {2}$
然后让 $S_n=n$，化解之后就是 $\sqrt{n+1}-1=x$，然后答案就是`ans=ceil(sqrt(n+1))-1`,然后发现它等价于`ans=sqrt(n)`，当然两个都能A。
注意long long

{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目链接](http://scpc.openjudge.cn/2018newchong/G/)
## 描述
> 小画学姐是个特别怕黑的弱者，好吧她一直都这么怂，所以胆小的她最喜欢的事情就是收集各种彩灯以备不时之需（并没有暗示礼物的意思）。有一天学校停电了，这些彩灯终于有了用武之地，小画学姐找了N个小伙伴陪她一起点亮这N盏灯，然而你们的小画学姐毒性不浅，她并不想“平凡”地点亮这些收藏，而是要通过一些特殊的规则才可以。

> 首先她把N个小伙伴从1到N依次排序，同时也把她的N盏灯从1到N依次排序。

> 然后每一个小伙伴根据自己的序号对所有是自己序号倍数编号的灯进行操作，操作很简单，若是灯开着就将它关闭，若灯关闭就打开这盏灯，可以肯定的是最初的时候小画学姐的灯全部都是关闭的。

> 那么问题来了，当N个小伙伴都进行操作后，小画学姐一共点亮了多少灯？
### 输入
> 输入只有一行，为一个整数N（$1<=N<=2*10^15$）

### 输出
> 输出小画学姐点亮彩灯的数量。

### 样例输入
{% codeblock lang:cpp %}
4
6
13
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
2
2
3
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}
#include<bits/stdc++.h>
using namespace std;

int main(){
    long long n;
    scanf("%lld",&n);
    printf("%lld\n",(long long)sqrt(n));
    return 0;
}
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***
# H.简单的射箭练习
{% tabs H %}
<!-- tab 思路 -->
{% note default %}
以下是出题人自己写的题解：
假设左学姐射中靶心概率为A，死库水学长射中靶心概率为B，显然答案是
$A+A(1-A)(1-B)+A(1-A)^2(1-B)^2+A(1-A)^3(1-B)^3+...+A(1-A)^{n-1}(1-B)^{n-1}$
显然是个等比数列前n项和：$ \frac {A(1-(1-A)^{(n+1)}(1-B)^{(n+1)})}{1-(1-A)(1-B)}$
，n显然无穷大，那么 $(1-A)^{n+1}(1-B)^{n+1}$ 显然是无限接近于0，因为保留12位小数，这个东西显然可以忽略，所以答案就是$ \frac{A}{1-(1-A)*(1-B)}$
当然你也可以暴力，当等比项$(1-A)^{n+1}(1-B)^{n+1}$小于$10^{-18}$左右的时候，就可以输出了。但是这种做法被我卡掉了。
众所周知，double只能保证14位小数精度，显然精度不够，我们需要使用long double,因为dev奇怪的原因c语言无法输出long double ,所以需要用c++输出，考虑到你们不会，我把怎么使用放在了提示里。

{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目链接](http://scpc.openjudge.cn/2018newchong/H/)
## 描述
> 弓箭手zxj与sksxz在进行一场比赛。他们轮流射击，zxj先手。

> 每次射击，zxj有a/b的概率命中靶心，sksxz有c/d的概率命中靶心。先命中靶心的赢得比赛。

> 求zxj赢得比赛的概率。

### 输入
> 多组输入，每组一行四个整数a,b,c,d

### 输出
> 一个实数表示答案，四舍五入保留12位小数。

### 样例输入
{% codeblock lang:cpp %}
1 2 3 4
2 3 4 5
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
0.571428571429
0.714285714286
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}

{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***

# I.简单易懂的签到题
{% tabs I %}
<!-- tab 思路 -->
{% note default %}
先两重循环求每行的最大值。
再看每个最大值是不是这一列的最小值。
{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目链接](http://scpc.openjudge.cn/2018newchong/I/)
## 描述
> 给定一个5*5的矩阵，每行只有一个最大值，每列只有一个最小值，寻找这个矩阵的鞍点。

> 鞍点指的是矩阵中的一个元素，它是所在行的最大值，并且是所在列的最小值（如果存在多个符合条件的数，则输出第一个即可）。

> 从1开始编号,从上往下从，左往右遍历的第一个

### 输入
> 5行，每行5个数

### 输出
> 一行，三个数，鞍点的行，列，值.

### 样例输入
{% codeblock lang:cpp %}
11 3 5 6 9
12 4 7 8 10
10 5 6 9 11
8  6 4 7 2
15 10 11 20 25
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
4 1 8
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}
//这题代码写得有点丑
#include<bits/stdc++.h>
using namespace std;

int main() {
    int a[5][5],Max[5][3]={0};//Max[i][0]:第i行的最大值 Max[i][1]:第i行最大值的行 Max[i][2]:第i行最大值的列 

    for(int i=0;i<5;i++)
        for(int j=0;j<5;j++){
            scanf("%d",&a[i][j]);
            if(Max[i][0]<a[i][j]){ //更新第i行的最大值
                Max[i][0]=a[i][j]; //值
                Max[i][1]=i;       //行
                Max[i][2]=j;       //列
            }
        }

    for(int i=0;i<5;i++)
        for(int j=0;j<5;j++){
            if(a[j][Max[i][2]]<Max[i][0]) break;//如果当前列有比它小的，说明它不是当前列最小，break
            if(j==4){
                printf("%d %d %d\n",Max[i][1]+1,Max[i][2]+1,Max[i][0]);
                return 0;
            }
        }
    }

    return 0;
}
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***

# J.善良的左学姐
{% tabs J %}
<!-- tab 思路 -->
{% note default %}
贪心
从小到大排序，每次拿最小
{% endnote %}
<!-- endtab -->
<!-- tab 题目描述 -->
 [题目链接](http://scpc.openjudge.cn/2018newchong/J/)
## 描述
> 最近，李学长和肖学长都比较喜欢死库水，今天他们两人因为一条死库水起了争执，坐在一旁的左学姐当然不能坐视不管，于是她为了平息这场战争，决定去再买几条死库水给两位学长，每家商店的死库水价格不都相同，规定每家商店最多买一条死库水，左学姐比较穷，但她又想尽可能多的买几条死库水以博得两位学长的宠幸，所以她想让你帮忙计算她最多能给两位学长买多少条死库水。
### 输入
> 多组测试数据，每组数据第一行输入一个n和m，n表示商店数目，m表示左学姐现在有多少钱，紧接着第二行是a1，a2...ai...an这n个数，ai表示第i个商店的死库水价格;如果n为0，这组数据不做计算，程序结束。（0 < n <= 100 , 0 < ai <= 10,000,000,000）

### 输出
> 每组测试数据输出一行表示左学姐能买到死库水的最大数量。

### 样例输入
{% codeblock lang:cpp %}
5  19
1  3  2  6  9
10  30
3  2  4  5  9  2  12  5  5  1
{% endcodeblock %}

### 样例输出
{% codeblock lang:cpp %}
4
8
{% endcodeblock %}
<!-- endtab -->
<!-- tab 代码 -->
{% codeblock lang:cpp %}
#include<bits/stdc++.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
const int N=1e2+5;
int n,i,ans;long long a[N]={0},m;

int main()
{
    while(~scanf("%d%lld",&n,&m)){
        rep(j,0,n) scanf("%lld",&a[j]);
        sort(a,a+n);                //这里用的自带的快速排序，也可以改成冒泡排序
        ans=i=0;                    //初始化
        while(m>=a[i]){
            ans++;
            m-=a[i++];
        }
        printf("%d\n",ans);
    }
    return 0;
}
{% endcodeblock %}
<!-- endtab -->
{% endtabs %}

***
大家加油，还有一次比赛，努力提升自己吧！