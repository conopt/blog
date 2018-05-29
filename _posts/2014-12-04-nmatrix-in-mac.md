------------------------------------------------------------------------

layout: post\_page\
title: nmatrix在Mac OS X上的编译&安装\
----

nmatrix是SciRuby的重要组成部分, 包含了功能完善的高性能的线性代数运算库,
但是我在编译安装nmatrix的过程中, 遇到了一大堆问题.\
(不得不说, Ruby社区还是太不成熟了, 相比之下, Python虽然丑了点,
但是有无数的脑残粉贡献代码, 给这个鸡肋的语言添加了不少可用性)

我的系统是Mac OS X 10.10 Yosemite, 不过早期版本的Mac也一样适用.

\- 在一切开始之前, 你必须具备了基本的开发环境 (Xcode)

\- 首先是常规的安装\
\`\`\`sh\
sudo gem install nmatrix\
\`\`\`\
不出意外, 执行这句话会得到一个类似NoMethodError的错误,
否则也没有本文存在的必要了.\
google一番之后发现了github上的[这个issue](https://github.com/SciRuby/nmatrix/pull/166)

稍微解释一下: 由于Mac上默认的g+[实际上就是clang]{.underline}+,
虽然这俩货声称兼容, 但是---version的输出明显不一样.\
同时, 它非常傻逼地利用了\`g[]{.underline}
---version\`的输出来获得版本号.\
为毛这货需要版本号呢? 在extconf.rb里,
我发现它利用版本号判断编译器对C++11的支持程度,
若是不支持就告诉你没法编译.

**这是件非常傻逼的事情!** 一个库还操心它自己能不能被编译??
