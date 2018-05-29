------------------------------------------------------------------------

layout: post\_page\
title: 剩余系中的递推关系\
----\
线性递推关系的计算有个很经典的办法
就是构造递推矩阵将递推转化为矩阵的连乘 用快速幂的思想可以极大地加快计算\
前几天做了ZJU2014校赛的题 F题是求Fibonacci数列的k次幂的前n项和
这里的k有100,000 所以普通的矩阵方法已经不适用了\
接下来的思路可以应用到各种可以求得通项递推关系中 这里仅举Fibonacci的特例

Fibonacci数列的定义是 \$F\_n = F\_{n-1} + F\_{n-2}\$\
我们的目标是求 \$ \\left(\\sum\\limits\_{i=1}\^n F\_n\^k\\right) \\bmod
m\$\
用解特征方程的办法可以得到通项的闭式\
\\\[\
\\begin{equation}\
F\_n =
\\frac{1}{\\sqrt{5}}\\left\[\\left(\\frac{1+\\sqrt{5}}{2}\\right)\^n~~\\left(\\frac{1~~\\sqrt{5}}{2}\\right)\^n\\right\]\
\\label{eq:Fib}\
\\end{equation}\
\\\]用跟逆元类似的思想 我们可以计算一个等价于\$\\sqrt{5}\$的数x\
如果这里5是m的二次剩余 则存在x\
使得 \$ x\^2 \\equiv 5 \\pmod m \$\
把 \$ \\sqrt{5} \\equiv x \\pmod m \$ 代入 \$\\eqref{eq:Fib}\$\
得\
\\\[\
\\begin{equation}\
F\_n \\equiv \\frac{1}{x}(a\^n-b\^n) \\pmod m\
\\label{eq:FibMod}\
\\end{equation}\
\\\]其中\$ a \\equiv \\frac{1+x}{2} ,\\ b \\equiv \\frac{1-x}{2} \\pmod
m\$\
这样就能很轻松的利用整数快速幂计算\$F\_n\$了\
接下来考虑k次幂,展开\$\\eqref{eq:FibMod}\$可得\
\\\[\
\\begin{equation}\
F\_n\^k \\equiv \\frac{1}{x\^k}\
\\sum\\limits\_{i=0}\^k {k\\choose i} (--1)\^{k-i}a\^{nk}b\^{n(k-i)}
\\pmod m\
\\label{eq:FibK}\
\\end{equation}\
\\\]\
对于任何\$F\_n\$, \$\\eqref{eq:FibK}\$中 \$\\Sigma\$里的系数都是相同的\
在\$F\_n\^k\$的求和式中, 合并相同的系数,
就构成了\$(k+1)\$个\$n\$项的等比数列,
这些等比数列利用求和公式可以在\$\\mathcal{O}(\\log{n})\$时间内算出,总复杂度\$\\mathcal{O}(k\\log{n})\$\
于是此题得到完美解决
