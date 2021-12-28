---
layout: post_page
title: "gcd行列式"
---

TAOCP第二卷中，出现了一个非常牛逼的习题，我觉得跳跃性接近组合数学，还结合了线性代数和数论，结论也非常漂亮。\
描述如下：

> 大小为\$n\$的矩阵\$A\$, 满足\$A\_{i,j}=gcd(i,j)\$, 试计算\$\\det A\$

**SOL:**

\\( 直接给出结论： \\\\\
构造 A=LU , 其中L为下三角矩阵，U为上三角矩阵 \\\\\
则\\det A=\\det L\\cdot\\det U=\\prod\\limits\_{i=1}\^n
L\_{i,i}U\_{i,i}\\\\\
\\)\
\\(\
L\_{i,j} =\
\\left\\{\
\\begin{array}{ll}\
1 & \\mbox{if \$j\\mid i\$};\\\\\
0 & \\mbox{if \$j\\nmid i\$}.\
\\end{array}\
\\right. \\\\\
U\_{i,j} =\
\\left\\{\
\\begin{array}{ll}\
\\phi(i) & \\mbox{if \$i\\mid j\$}; & \\phi(n) 表示欧拉函数 \\\\\
0 & \\mbox{if \$i\\nmid j\$}.\
\\end{array}\
\\right. \\\\\
\\)\
\\(\
这时候\\\\\
\\begin{eqnarray}\
LU\_{i,j}&=&\\sum\\limits\_{k=1}\^n L\_{i,k}U\_{k,j}\\\\\
&=&\\sum\\limits\_{k\\mid gcd(i,j)} \\phi(k)\\\\\
&=&gcd(i,j) \\ \\ (Moebius反演)\
\\end{eqnarray} \\\\\
\\)\
\\(\
这说明 \\\\\
\\begin{eqnarray}\
\\det A &=& \\det L \\cdot \\det U \\\\\
&=& 1 \\cdot \\prod\\limits\_{i=1}\^n U\_{i,i} \\\\\
&=& \\prod\\limits\_{i=1}\^n \\phi(i) \\\\\
\\end{eqnarray}\
\\)\
\\(\
现在再回头看看这个结论\\\\\
实际上LU的构造过程就是将A用初等变换消成上三角U的过程\\\\\
最后的变换矩阵就是L\
\\)

应用：\
[SEERC 2008 - GCD Determinant](http://poj.org/problem?id=3910)
