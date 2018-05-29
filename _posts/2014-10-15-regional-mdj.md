------------------------------------------------------------------------

layout: post\_page\
title: 2014牡丹江regional\
----\
嘛... 现场没A出E还是比较可惜的...\
打完去哈尔滨瞎逛 还去世界名校HIT见了见基友(炸了炸PO(咦?\
简易题解

#### Average Score {#average-score .prob-title}

签到题

#### Building Fire Stations {#building-fire-stations .prob-title}

20万个点的树 找两个点作为消防站
使得每个点到离它最近的消防站的距离的最大值最小

**SOL:**

pre. 二分答案转成判定性问题 假设消防站不在最长链上
那么往最长链上移动不会使解更劣 也就是说 最后一定存在一个解
使得两个消防站都在最长链上\
基于以上性质 假设最大距离为k 那么离最长链两端距离为k的点是必须取的
以这两个点为消防站计算最大距离 就知道k是否可行了

{% highlight cpp linenos %}\
\#include <iostream>\
\#include <cstdio>\
\#include <cstring>\
\#include <new>\
using namespace std;\
const int N = 202020;\
const int M = N\*2;\
struct Edge\
{\
int v;Edge\*next;Edge(){}\
Edge(int V,Edge\*X):v(V),next(X){}\
}XP\[M\],\*E\[N\],\*Pe;\
int n;\
namespace dfs\
{\
int \*dis;\
bool vis\[N\];\
void dfs(int x)\
{\
vis\[x\] = true;\
for(Edge\*i=E\[x\];i;i=i-\>next)\
if (!vis\[i-\>v\])\
{\
dis\[i-\>v\] = dis\[x\]+1;\
dfs(i-\>v);\
}\
}\
void run(int x)\
{\
memset(vis,0,sizeof(vis));\
dis\[x\] = 0;\
dfs(x);\
}\
};\
void ins(int u, int v)\
{\
E\[u\]=new(Pe[]{.underline})Edge(v,E\[u\]);\
}\
int dis\[N\],dl\[N\],dr\[N\], L, R, link\[N\];\
void init()\
{\
Pe = XP;\
memset(E,0,sizeof(E));\
scanf("%d",&n);\
for(int i=1; i\<n; ++i)\
{\
int u,v;\
scanf("%d%d",&u,&v);\
ins(u,v),ins(v,u);\
}\
dfs::dis = dis;\
dfs::run(1);\
L = 1;\
for(int i=1; i\<=n; ++i)\
if (dis\[i\]\>dis\[L\])\
L = i;\
dfs::dis = dl;\
dfs::run(L);\
R = 1;\
for(int i=1; i\<=n; ++i)\
if (dl\[i\]\>dl\[R\])\
R = i;\
dfs::dis = dr;\
dfs::run®;\
for(int i=1; i\<=n; ++i)\
if (dl\[i\] + dr\[i\]  dr\[L\])
      link\[dl\[i\]\] = i;
}
int d1\[N\], d2\[N\];
bool ok(int k)
{
  int p1 = link\[k\];
  int p2 = link\[dr\[L\]-k\];
  dfs::dis = d1;
  dfs::run(p1);
  dfs::dis = d2;
  dfs::run(p2);
  for(int i=1; i\<=n; ++i)
    if (d1\[i\]\>k && d2\[i\]\>k)
      return false;
  return true;
}
void work()
{
  int l = -1, r = dr\[L\];
  while(r-l\>1)
  {
    int mid = (l+r)\>\>1;
    if (ok(mid))
      r = mid;
    else
      l = mid;
  }
  int p1 = link\[r\];
  int p2 = link\[dr\[L\]-r\];
  if (p1\>p2)
    swap(p1, p2);
  if (p2p1)\
{\
p1 = 1;\
if (p2 == p1)\
p2 = 2;\
}\
printf("%d %d %d\\n", r, p1, p2);\
}\
int main()\
{\
\#ifndef ONLINE\_JUDGE\
freopen("in","r",stdin);\
freopen("out","w",stdout);\
\#endif\
int T;\
scanf("%d",&T);\
while(T---)\
{\
init();\
work();\
}\
return 0;\
}\
{% endhighlight %}

#### Domination {#domination .prob-title}

50x50的棋盘 每次随机放一个棋子 直到每行每列都有至少一个棋子 求期望棋子数

**SOL:**

pre. dp啦\> \<\
dp\[i\]\[j\]\[k\]代表i个棋子占据j行k列的概率\
那么在dp\[i\]\[j\]\[k\]这个状态下，放一个棋子，会有四种可能\
----k----------m-k------\
\| \| \|\
\| \| \|\
j 1 j 2 j\
\| \| \|\
\| \| \|\
----k----------m-k------\
\| \| \|\
n n n\
- 3 - 4 -\
j j j\
\| \| \|\
----k----------m-k------\
如果棋子放进1号区域，则没有新增占据的行和列\
2号区域就是多一列\
3号多一行\
4号多一行一列\
往后推即可\
注意1号区域有i个格子已经被占了\
而且\`jn&&km\`这个状态不能再转移了

{% highlight cpp linenos %}\
\#include <iostream>\
\#include <cstring>\
using namespace std;\
const int N = 52;\
double dp\[N\*N\]\[N\]\[N\];\
int main()\
{\
\#ifndef ONLINE\_JUDGE\
freopen("in","r",stdin);\
freopen("out","w",stdout);\
\#endif\
int T;\
cin\>\>T;\
while(T---)\
{\
int n, m;\
cin \>\> n \>\> m;\
memset(dp,0,sizeof(dp));\
dp\[0\]\[0\]\[0\] = 1;\
for(int i=0; i\<n\*m; ++i)\
for(int j=0; j\<=n; ++j)\
for(int k=0; k\<=m; ++k)\
{\
double now = dp\[i\]\[j\]\[k\];\
if (j\<n \|\| k\<m)\
dp\[i+1\]\[j\]\[k\] += now\*(j\*k-i)/(n\*m-i);\
dp\[i+1\]\[j+1\]\[k\] += now\*(n-j)\*k/(n\*m-i);\
dp\[i+1\]\[j\]\[k+1\] += now\*j\*(m-k)/(n\*m-i);\
dp\[i+1\]\[j+1\]\[k+1\] += now**[]{.n-j}**(m-k)/(n\*m-i);\
}\
double ans = 0;\
for(int i=0; i\<=n\*m; ++i)\
ans += i\*dp\[i\]\[n\]\[m\];\
printf("%.12lf\\n", ans);\
}\
return 0;\
}\
{% endhighlight %}

#### Excavator Contest {#excavator-contest .prob-title}

残念...ˊ\_\>ˋ\
N\*N(2\<=N\<=512)的四连通格子，一笔画，要求起点终点在边界上，而且转弯次数不小于N\*(N-1)--1次

**SOL:**

pre. 偶数情况随便一画就出来了ˊ\_\>ˋ\
奇数情况需要归纳，用N-2的情况构造出N的情况\
我采用的构造方法是这样的\
S─┐┌┐┌┐│E\
┌┘│└┘└┘\
└┐E'\
┌┘\
└┐\
┌┘\
└─S'\
注意S'点和E'点 代表N-2的情况的起点和终点\
S点和E点代表N的情况的起点和终点 也就是说
把N-2的情况左转再加上两行两列就构成了N的情况\
归纳假设是N=1时 转弯次数0\>1\*(1-1)--1\
假设N的情况可以 那么加上两行两列共2(N+N-2)个格子\
其中S点E点不是转弯 S'和E'点在合并之后变成了新的转弯\
于是N+2的情况下 转弯次数 \>= N (N-1)~~1 + 2(N+N-2)~~ 2 = (N+2)(N+1)--1\
实现的话 只需要把左上角的位置当作参数 S点从1开始走
E点从N\*N开始倒退即可\
在子问题做完之后把 右下角的N-2矩阵左转即可 代码相当简单

{% highlight cpp linenos %}\
\#include <iostream>\
\#include <cstdio>\
\#include <cstring>\
using namespace std;\
const int N = 600;\
int result\[N\]\[N\];\
void even(int n)\
{\
\#define go(i,j) result\[i\]\[j\]=++cnt\
int cnt = 0;\
for(int i=1; i\<=n\>\>1; ++i)\
{\
if (i&1)\
{\
if (i1)
      {
        go(1,2);
        go(1,1);
        go(2,1);
        go(2,2);
      }
      else
      {
        go(2,i\*2-1);
        go(1,i\*2-1);
        go(1,i\*2);
        go(2,i\*2);
      }
      for(int j=2; j\<=n\>\>1; ++j)
      {
        go(j\*2-1, i\*2);
        go(j\*2-1, i\*2-1);
        go(j\*2, i\*2-1);
        go(j\*2, i\*2);
      }
    }
    else
    {
      for(int j=n\>\>1; j\>=2; \--j)
      {
        go(j\*2, i\*2-1);
        go(j\*2, i\*2);
        go(j\*2-1, i\*2);
        go(j\*2-1, i\*2-1);
      }
      go(2,i\*2-1);
      go(1,i\*2-1);
      go(1,i\*2);
      go(2,i\*2);
    }
  }
  if (!((n\>\>1)&1))
  {
    cnt -= 3;
    go(2, n);
    go(1, n);
    go(1, n-1);
  }
\#undef go
}
void odd(int n, int start, int end, int row, int col)
{
\#define S(i,j) result\[row-1+(i)\]\[col-1+(j)\] = ++start
\#define E(i,j) result\[row-1+(i)\]\[col-1+(j)\] = \--end
  if (n1)\
{\
S (1,1);\
return;\
}\
for(int i=1; i\<=n\>\>1; ++i)\
{\
S (i\*2-1, 1);\
S (i\*2-1, 2);\
S (i\*2, 2);\
S (i\*2, 1);\
}\
S (n, 1);\
S (n, 2);\
for(int j=n\>\>1; j\>=2; ---j)\
{\
E (1, j\*2+1);\
E (2, j\*2+1);\
E (2, j\*2);\
E (1, j\*2);\
}\
E (1, 3);\
E (2, 3);\
odd(n-2, start, end, row+2, col+2);\
//rotate left\
static int tmp\[N\]\[N\];\
for(int i=1; i\<=n-2; ++i)\
for(int j=1; j\<=n-2; ++j)\
tmp\[i\]\[j\] = result\[row+1+i\]\[col+1+j\];\
for(int i=1; i\<=n-2; ++i)\
for(int j=1; j\<=n-2; ++j)\
result\[row+1+i\]\[col+1+j\] = tmp\[j\]\[n-2-i+1\];\
\#undef E\
\#undef S\
}\
int main()\
{\
\#ifndef ONLINE\_JUDGE\
freopen("in","r",stdin);\
freopen("out","w",stdout);\
\#endif\
int T;\
cin \>\> T;\
while(T---)\
{\
int N;\
cin \>\> N;\
if (N&1)\
odd(N, 0, N\*N+1, 1, 1);\
else\
even(N);\
for(int i=1; i\<=N; ++i)\
{\
for(int j=1; j\<=N; ++j)\
printf("%d ", result\[i\]\[j\]);\
printf("\\n");\
}\
}\
return 0;\
}\
{% endhighlight %}

#### Hierarchical Notation {#hierarchical-notation .prob-title}

解析简单json

**SOL:**

pre. ...直接parse就好了 需要内存池免得MLE\
有个trick 就是node里 只存在输入中的左端点和右端点 这样速度快 也不耗内存

{% highlight cpp linenos %}\
\#include <iostream>\
\#include <stack>\
\#include <cstdio>\
\#include <cstring>\
\#include <string>\
\#include <new>\
\#include <map>\
using namespace std;\
const int M = 10100;\
struct Node\
{\
map\<string, Node\*\> c;\
int l,r;\
Node(){}\
Node(int L, int R):l(L),r®{c.clear();}\
}NP\[M\],\*Pe,\*root;\
char inp\[M\*40\],cmd\[M\*40\];\
int next\_\[M\*40\];\
Node\* parse(int l, int r)\
{\
Node \*res = new(Pe[]{.underline})Node(l,r);\
if (inp\[l\]  \'\"\')
    return res;
  for(int i=l+1; i\<r; ++i)
  {
    if (inp\[i\]  ',')\
continue;\
inp\[next\_\[i\]+1\] = 0;\
res-\>c\[inp+i\] = parse(next*[+2, next]{lang="i"}*\[next\_\[i\]+2\]);\
inp\[next\_\[i\]+1\] = ':';\
i = next*\[next*\[i\]+2\]+1;\
}\
return res;\
}\
void init()\
{\
static stack<int> S;\
static stack<char> C;\
Pe = NP;\
int l = strlen(inp);\
bool ins = false;\
for(int i=0; i\<l; ++i)\
switch(inp\[i\])\
{\
case '{':\
if (!ins)\
{\
C.push('{');\
S.push(i);\
}\
break;\
case '}':\
if (!ins)\
{\
C.pop();\
next\_\[S.top()\] = i;\
S.pop();\
}\
break;\
case '"':\
if (ins)\
{\
C.pop();\
next\_\[S.top()\] = i;\
ins = false;\
S.pop();\
}\
else\
{\
C.push('"');\
ins = true;\
S.push(i);\
}\
break;\
}\
root = parse(0,l-1);\
}\
int main()\
{\
\#ifndef ONLINE\_JUDGE\
freopen("in","r",stdin);\
freopen("out","w",stdout);\
\#endif\
int T;\
scanf("%d",&T);\
while(T---)\
{\
scanf("%s",inp);\
init();\
int m;\
scanf("%d",&m);\
while(m---)\
{\
scanf("%s",cmd);\
int last = 0;\
int l = strlen(cmd);\
cmd\[l\] = '.';\
Node \*t = root;\
bool err = false;\
for(int i=1; i\<=l; ++i)\
if (cmd\[i\]  \'.\')
        {
          cmd\[i\] = 0;
          map\<string, Node\*\>::iterator it = t-\>c.find(cmd+last);
          if (it  t-\>c.end())\
{\
err = true;\
break;\
}\
t = it-\>second;\
last = i+1;\
}\
if (err)\
{\
printf("Error!\\n");\
}\
else\
{\
char tmp = inp\[t-\>r+1\];\
inp\[t-\>r+1\] = 0;\
printf("%s\\n", inp+t-\>l);\
inp\[t-\>r+1\] = tmp;\
}\
}\
}\
return 0;\
}\
{% endhighlight %}

#### Information Entropy {#information-entropy .prob-title}

如题... 算信息熵...

{% highlight cpp linenos %}\
\#include <iostream>\
\#include <cstdio>\
\#include <cmath>\
using namespace std;\
int n; char unit\[10\];\
int main()\
{\
\#ifndef ONLINE\_JUDGE\
freopen("in","r",stdin);\
freopen("out","w",stdout);\
\#endif\
int T;\
cin\>\>T;\
while(T---)\
{\
cin\>\>n\>\>unit;\
double H = 0;\
while(n---)\
{\
int P;\
cin\>\>P;\
if (P  0)
        continue;
      double p = P/100.0;
      H -= p\*log(p);
    }
    if (unit\[0\]  'b')\
H /= log(2);\
if (unit\[0\] == 'd')\
H /= log(10);\
printf("%.10lf\\n", H);\
}\
return 0;\
}\
{% endhighlight %}

#### Known Notation {#known-notation .prob-title}

两种操作

-   交换两个字符
-   添加一个字符\
    用最少操作次数将给定字符串变成合法的后缀表达式

**SOL:**

pre. 贪心 只要任意前缀数字必须比操作符多就是个合法的后缀表达式\
那么先插入足够的数字到前面 然后从头往后扫
一旦不合法就把后面的数字交换到前面来

{% highlight cpp linenos %}\
\#include <iostream>\
\#include <cstdio>\
\#include <cstring>\
\#include <cctype>\
using namespace std;\
char s\[1010\];\
int main()\
{\
\#ifndef ONLINE\_JUDGE\
freopen("in","r",stdin);\
freopen("out","w",stdout);\
\#endif\
int T;\
scanf("%d",&T);\
while(T---)\
{\
scanf("%s",s);\
int l = strlen(s);\
int dig = l-1;\
while(s\[dig\]\'\*\' && dig\>=0)
      \--dig;
    int digit = 0;
    int aster = 0;
    for(int i=0; i\<l; ++i)
    {
      if (isdigit(s\[i\]))
        ++digit;
      else
        ++aster;
    }
    if (digit \<= aster)
      digit = aster-digit+1;
    else
      digit = 0;
    aster = 0;
    int ans = digit;
    for(int i=0; i\<l; ++i)
    {
      if (isdigit(s\[i\]))
        ++digit;
      else
        ++aster;
      if (aster \>= digit)
      {
        if (dig \> i)
        {
          ++ans;
          swap(s\[i\], s\[dig\]);
          while(s\[dig\]'\*' && dig\>=0)\
---dig;\
---aster;\
++digit;\
}\
if (aster \>= digit)\
{\
ans += aster - digit + 1;\
digit = aster+1;\
}\
}\
}\
printf("%d\\n", ans);\
}\
return 0;\
}\
{% endhighlight %}
