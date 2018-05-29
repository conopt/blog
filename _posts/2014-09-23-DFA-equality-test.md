---
layout: post_page
title: "2014 ACM/ICPC Asia Regional Guangzhou Online - Equality Test"
---

[题目地址](http://acm.hdu.edu.cn/showproblem.php?pid=5026)\
一个赤裸裸的DFA等价判定问题\
首先要解决的是把题中的NRE转换成等价的DFA\
这里采用了经典的做法，先把NRE转成eNFA，由于数据里的NRE比较短，生成的eNFA不会超过60个状态（其实不超过30个），所以subset可以用一个long
long来压缩，于是可以很简单地利用subset construction转成DFA\
在得到了两个DFA之后，构造一个新的DFA\
新的状态集合\$ Q\' = Q\_1\\times Q\_2 \$\
对于\$q\' = (q\_1, q\_2)\$\
\$\\delta(q\',a) = (\\delta(q\_1, a), \\delta(q\_2, a))\$\
新的可接受状态集合\$F'=(F\_1\\times\\overline{F\_2})\\cup(F\_2\\times\\overline{F\_1})\$\
实际上 \$F\_1\\times\\overline{F\_2}\$ 代表了DFA2包含DFA1，反之亦然。\
如果新的状态机为空(无法到达任何一个\$F'\$内的状态)，那么原先的两个自动机等价\
代码好长\_(;3J \<)\_\
NOTE: 值得一提的是，数据中似乎有单独一个\*的情况，导致我WA了三天。。

{% highlight cpp linenos %}

\#include <iostream>\
\#include <stack>\
\#include <cstdio>\
\#include <cstring>\
\#include <string>\
\#include <vector>\
\#include <queue>\
\#include <set>\
\#include <map>\
\#define see(x) cerr\<\<\#x\<\<\" = \" \<\< (x) \<\< endl\
using namespace std;

typedef pair\<int, int\> PII;\
\#define mp make\_pair\
struct eNFA\
{\
static const int N = 200;\
static const int eps = 10;\
static const int S = eps+1;\
bool G\[N\]\[S\]\[N\];\
int size;\
int par\[N\], del\[N\];\
long long clos\[N\];\
bool vis\[N\];\
PII fa;

void cdfs(int s)\
{\
if (vis\[s\]) return;\
vis\[s\] = true;\
for(int t=1; t\<=size; ++t)\
if (G\[s\]\[eps\]\[t\])\
cdfs(t);\
}\
long long closure(int s)\
{\
if (clos\[s\]  0)
    {
      memset(vis,0,sizeof(vis));
      cdfs(s);
      for(int i=1; i\<=size; ++i)
        if (vis\[i\])
          clos\[s\] \|= 1LL\<\<i;
    }
    return clos\[s\];
  }
  long long where(int s, int e)
  {
    long long ans = 0;
    for(int i=1; i\<=size; ++i)
      if (G\[s\]\[e\]\[i\])
        ans \|= closure(i);
    return ans;
  }
  bool contain\_final(long long s)
  {
    return ((s\>\>fa.second)&1)1;\
}\
PII concat(PII l, PII r)\
{\
G\[l.second\]\[eps\]\[r.first\] = true;\
return mp(l.first, r.second);\
}\
PII star(PII a)\
{\
int s = ++size;\
int t = ++size;\
G\[s\]\[eps\]\[a.first\] = true;\
G\[s\]\[eps\]\[t\] = true;\
G\[a.second\]\[eps\]\[a.first\] = true;\
G\[a.second\]\[eps\]\[t\] = true;\
return mp(s,t);\
}\
PII merge(vector<PII> macs)\
{\
int s = ++size;\
int t = ++size;\
for(int i=0; i\<macs.size(); ++i)\
{\
G\[s\]\[eps\]\[macs\[i\].first\] = true;\
G\[macs\[i\].second\]\[eps\]\[t\] = true;\
}\
return mp(s,t);\
}\
int nstar\[N\];\
PII parse(char\*s, int l, int r)\
{\
if (l\>r)\
{\
++size;\
return mp(size,size);\
}\
if (lr)
    {
      if (s\[l\]  '\*')\
{\
return parse(s,1,0); //去掉这句就WA了。。日！\
}\
PII fa = mp(size+1, size+2);\
G\[size+1\]\[s\[l\]-'0'\]\[size+2\] = true;\
size += 2;\
return fa;\
}\
if (s\[l\]  \'(\' && s\[r\]  ')' && par\[l\]r)
      return parse(s, l+1, r-1);
    if (del\[l\] \< r)
    {
      vector\<PII\> macs;
      while(del\[l\] \< r)
      {
        macs.push\_back(parse(s, l, del\[l\]-1));
        l = del\[l\]+1;
      }
      if (l\<=r)
        macs.push\_back(parse(s, l, r));
      return merge(macs);
    }
    if (s\[par\[l\]+1\]  '\*')\
{\
if (par\[l\]+1  r)
        return star(parse(s, l, par\[l\]));
      return concat(parse(s, l, par\[l\]+1), parse(s, par\[l\]+2, r));
    }
    return concat(parse(s, l, par\[l\]), parse(s, par\[l\]+1, r));
  }
  void parse(char\*s)
  {
    memset(clos,0,sizeof(clos));
    memset(G,0,sizeof(G));
    size = 0;
    int l = strlen(s);
    del\[l\] = par\[l\] = nstar\[l\] = l;
    stack\<int\> left;
    for(int i=0; i\<l; ++i)
    {
      if (s\[i\]  '(')\
{\
left.push(i);\
continue;\
}\
if (s\[i\] == ')')\
{\
par\[left.top()\] = i;\
left.pop();\
continue;\
}\
par\[i\] = i;\
}\
for(int i=l-1; i\>=0; ---i)\
switch(s\[i\])\
{\
case '\|':\
del\[i\] = i;\
nstar\[i\] = nstar\[i+1\];\
break;\
case '(':\
del\[i\] = del\[par\[i\]+1\];\
nstar\[i\] = nstar\[par\[i\]+1\];\
break;\
case '\*':\
del\[i\] = del\[i+1\];\
nstar\[i\] = i;\
break;\
default:\
del\[i\] = del\[i+1\];\
nstar\[i\] = nstar\[i+1\];\
}\
fa = parse(s, 0, l-1);\
}\
};

struct DFA\
{\
static const int N = 500;\
static const int S = 10;\
int G\[N\]\[S\];\
int size, start;\
bool accept\[N\];\
DFA()\
{\
start = size = 0;\
memset(G,0,sizeof(G));\
memset(accept,0,sizeof(accept));\
}\
bool qvis\[N\];\
queue<int> Q;\
bool empty()\
{\
while(!Q.empty())\
Q.pop();\
memset(qvis,0,sizeof(qvis));\
Q.push(start);\
qvis\[start\] = true;\
qvis\[0\] = true;\
while(!Q.empty())\
{\
int now = Q.front();\
Q.pop();\
if (accept\[now\])\
return false;\
for(int e=0; e\<S; ++e)\
if (!qvis\[G\[now\]\[e\]\])\
{\
Q.push(G\[now\]\[e\]);\
qvis\[G\[now\]\[e\]\]=true;\
}\
}\
return true;\
}\
\#define label(i,j) ((((i)\*(m+1))+j))\
DFA (const DFA&dfa1, const DFA&dfa2)\
{\
memset(G,0,sizeof(G));\
memset(accept,0,sizeof(accept));\
int n = dfa1.size;\
int m = dfa2.size;\
for(int i=0; i\<=n; ++i)\
for(int j=0; j\<=m; ++j)\
{\
//printf(\"%d =\> \[%d, %d\]\\n\", label(i,j), i,j);
        for(int e=0; e\<S; ++e)
        {
          int ni = dfa1.G\[i\]\[e\];
          int nj = dfa2.G\[j\]\[e\];
          G\[label(i,j)\]\[e\] = label(ni,nj);
        }
        bool f1 = dfa1.accept\[i\];
        bool f2 = dfa2.accept\[j\];
        if (f1 && f2) continue;
        if (f1 \|\| f2)
          accept\[label(i,j)\] = true;
      }
    size = label(n,m);
    start = label(dfa1.start, dfa2.start);
  }
\#undef label
  map\<long long, int\> label;
  void subset\_dfs(long long s, eNFA&enfa)
  {
    if (label.find(s)  label.end())\
label.insert(mp(s, ++size));\
else\
return;\
for(int e=0; e\<enfa.eps; ++e)\
{\
long long t = 0;\
long long j=s;\
for(int i=0; j\>0; ++i, j\>\>=1) if (j&1)\
t \|= enfa.where(i, e);\
if (t != 0)\
{\
subset\_dfs(t, enfa);\
G\[label\[s\]\]\[e\] = label\[t\];\
}\
}\
}\
void construct(eNFA&enfa)\
{\
start = size = 0;\
memset(G,0,sizeof(G));\
memset(accept,0,sizeof(accept));\
label.clear();\
subset\_dfs(enfa.closure(enfa.fa.first), enfa);\
start = label\[enfa.closure(enfa.fa.first)\];\
for(map\<long long, int\>::iterator it=label.begin(); it!=label.end();
++it)\
if (enfa.contain\_final(it-\>first))\
accept\[it-\>second\] = true;\
}\
};

char line\[100\];\
DFA dfa1, dfa2, dfa3;\
eNFA enfa;\
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
scanf("%s",line);\
enfa.parse(line);\
dfa1.construct(enfa);\
scanf("%s",line);\
enfa.parse(line);\
dfa2.construct(enfa);\
dfa3 = DFA (dfa1, dfa2);\
printf("%s\\n", dfa3.empty()?[YES]("NO"));\
}\
return 0;\
}

{% endhighlight %}
