---
layout: post_page
title: "Michigan Invitational Programming Contest 2011"
---

#### H. --- Two Sets {#h.-two-sets .prob-title}

题意:\
定义全集为\$ S = \\{ x \\mid x\\in\\mathbb{N}, x\\le N \\} \\quad (N\\le
200000) \$\
给两个初始集合A, B 计算表达式 要求支持 交(+) 并(\*) 补(-)\
例如 \$\
N = 10 \\\\\
A = \\{ 1, 5, 10\\} \\\\\
B = \\{ 2, 7, 5, 6 \\} \\\\\
\$\
计算 \$ (A\*B+-(A)\*-(-(B))) \$ 的结果为 \$ \\{ 2, 5, 6, 7 \\} \$

**SOL:**

pre. 元素的属性只有两种 在与不在集合内，将集合压位表示，硬算即可

{% highlight cpp linenos %}\
\#include <iostream>\
\#include <cstdio>\
\#include <cstring>\
\#include <stack>\
\#include <queue>\
\#define see(x) cerr\<\<\#x\<\<\" = \" \<\< (x) \<\< endl\
using namespace std;\
\#define id(x) ((x)\>\>5)\
\#define low(x) (1U\<\<((x)&0x1f))

int n,m,tot,cl;\
bool lower\[300\]\[300\];\
unsigned int num\[2600\]\[6270\];\
char expr\[5010\];

void init()\
{\
memset(lower,0,sizeof(lower));\
memset(num\[1\], 0, sizeof(num\[0\]));\
memset(num\[2\], 0, sizeof(num\[1\]));\
lower\['+'\]\['-'\] = true;\
lower\['+'\]\['\*'\] = true;\
lower\['\*'\]\['-'\] = true;\
cl = id(n-1)+1;\
int l;\
for(int i=1; i\<=2; i[]{.underline})\
{\
scanf("%d",&l);\
while(l---)\
{\
int x;\
scanf("%d",&x);\
x---;\
num\[i\]\[id(x)\] \|= low(x);\
}\
}\
tot = 2;\
scanf("%s",expr);\
}

stack<char> op;\
queue<char> rpn;\
stack<int> ans;

void parse()\
{\
for(int i=0; i\<m; i[]{.underline})\
{\
switch (expr\[i\])\
{\
case 'A':\
case 'B':\
rpn.push(expr\[i\]);\
break;

case '(':\
case '-':\
case '\*':\
case '+':\
while(!op.empty() && lower\[expr\[i\]\]\[op.top()\])\
{\
rpn.push(op.top());\
op.pop();\
}\
op.push(expr\[i\]);\
break;\
case ')':\
while(op.top() != '(')\
{\
rpn.push(op.top());\
op.pop();\
}\
op.pop();\
break;\
}\
}\
while(!op.empty())\
{\
rpn.push(op.top());\
op.pop();\
}\
}

int intersect(int a, int b)\
{\
int c=++tot;\
for(int i=0; i\<cl; i[]{.underline})\
num\[c\]\[i\] = num\[a\]\[i\]&num\[b\]\[i\];\
return c;\
}

int \_union(int a, int b)\
{\
int c=++tot;\
for(int i=0; i\<cl; i[]{.underline})\
num\[c\]\[i\] = num\[a\]\[i\]\|num\[b\]\[i\];\
return c;\
}

int operate(char x, int l, int r)\
{\
if (x=='+')\
return \_union(l,r);\
return intersect(l,r);\
}

int rev(int a)\
{\
int c=++tot;\
for(int i=0; i\<cl; i[]{.underline})\
num\[c\]\[i\] = \~num\[a\]\[i\];\
return c;\
}

void calc()\
{\
while(!rpn.empty())\
{\
char x = rpn.front();\
rpn.pop();\
switch(x)\
{\
case 'A': //1\
case 'B': //2\
ans.push(x-'A'+1);\
break;\
case '\*':\
case '+':\
{\
int rhs = ans.top();\
ans.pop();\
int lhs = ans.top();\
ans.pop();\
ans.push(operate(x, lhs, rhs));\
break;\
}\
case '-':\
int t = ans.top();\
ans.pop();\
ans.push(rev(t));\
}\
}\
}

void work()\
{\
parse();\
calc();\
int A = ans.top();\
int sum=0;\
for(int i=0; i\<n; i[]{.underline})\
if (num\[A\]\[id(i)\]&low(i))\
sum[]{.underline};\
printf("%d", sum);\
for(int i=0; i\<n; i[]{.underline})\
if (num\[A\]\[id(i)\]&low(i))\
printf(\" %d", i+1);\
printf("\\n\");\
}

int main()\
{\
\#ifndef ONLINE\_JUDGE\
freopen("in","r",stdin);\
freopen("out","w",stdout);\
\#endif\
int t=0;\
while (scanf("%d%d",&n,&m)2)
  {
    if (n0 && m==0)\
break;\
init();\
printf("Case %d: ",++t);\
work();\
}\
return 0;\
}\
{% endhighlight %}
