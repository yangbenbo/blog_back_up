---
title: LaTex公式编辑
date: 2023-07-11 06:37:51
categories:
- Blog
tags:
mathjax: true
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

- [基本使用](#基本使用)
  - [公式编号](#公式编号)
  - [注释](#注释)
- [引用](#引用)
- [附录](#附录)

<!-- /code_chunk_output -->

想要在文档中快速敲出好看的公式，[LaTex](https://www.latex-project.org/)必不可少。LaTeX系统是一种可以处理排版和渲染的标记语言，一种基于TEX的排版系统，由美国计算机科学家莱斯利·兰伯特在20世纪80年代初期开发，非常适用于生成高印刷质量的科技和数学、物理文档。

简单公式可以参考下面规则,对于复杂公式可以直接使用[在线latex](https://www.latexlive.com/), 或者使用mathtype编辑后复制,对应公式识别可以使用[mathpix](https://mathpix.com/)
## 基本使用
- 常用字符

        `$`: 左对齐公式
        `$$`: 居中对齐公式
        `_`、`^`: 下标，上标
        `\`: 转义字符
        `\\`: 换行
                $$
                f(x)=2x+1 \\
                =2+1 \\
                =3
                $$
- 对齐
    `begin{aligned}`对齐，`&`为对齐位置，一般在`=`前面

        $$
        \begin{aligned}
        f(x)&=2x+1 \\
        &=2+1 \\
        &=3
        \end{aligned}
        $$
    $$
    \begin{aligned}
    f(x)&=2x+1 \\
    &=2+1 \\
    &=3
    \end{aligned}
    $$

- 字体
    具体根据需要查询[latex在线help](https://www.latexlive.com/help)

    比如拉式变换用到的花体$\mathcal{L}$

- 空格
    `quad`: 空1格
    `qquad`: 空2格

- 绝对值范数
    `||`, `||||`, 绝对值和范数

- 括号
    `(), [], |`为符号本身，大括号用`\{\}`，需要转义(上下标集合用了花括号)
    长括号`$\left(表达式\right)$`

- 分式
    `\frac{分子}{分母}`, 简单可使用`\frac ab`, 对应$\frac ab$，复杂可以使用`分子 \over 分母`, $a \over b$

- 根
    \sqrt [根指数] {被开方数}
    缺省根指数时为2

- 对数
    \log_{对数底数}{表达式}

    表达式的大括号可省略
- 省略号
    数学公式中常见的省略号有两种，`\ldots` 表示与文本底线对齐的横向省略号$\ldots…$，`\cdots` 表示与文本中线对齐的横向省略号$\cdots⋯$

- 最值
    `\max_{下标表达式}{最值表达式}`表示最大值，`\min_{下标表达式}{最值表达式}`表达最小值

- 累加累乘
    使用 `\sum_{下标表达式}^{上标表达式}{累加表达式}`来输入一个累加。
    与之类似，使用 `\prod \bigcup \bigcap`来分别输入累乘、并集和交集。
    此类符号在行内显示时上下标表达式将会移至右上角和右下角。

        $$\sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} R$$
    对应
    $$\sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} R$$

- 矢量
    使用 `\vec{矢量}`来自动产生一个矢量。`$\vec{a}$`对应$\vec{a}$

- 极限
    `\lim_{变量 \to 表达式} 表达式`, 比如`\lim_{n \to +\infty} \frac{1}{n(n+1)}`
    对应

    $$\lim_{n \to +\infty} \frac{1}{n(n+1)}$$

- 导数
    ${\rm d}x$或${\text d}x$或$\text{d}x$

- 偏导
    `${\partial y} \over {\partial x}$`对应${\partial y} \over {\partial x}$

- 梯度
    `$\nabla f(x)$`对应$\nabla f(x)$
- 积分
    \int_积分下限^积分上限 {被积表达式}, `\int_0^1 {x^2} \,{\rm d}x`对应
    $$\int_0^1 {x^2} \,{\rm d}x$$

### 公式编号
- 自动编号

        $$
        \begin{equation}
        a^2+b^2=c^2
        \end{equation}
        $$

    只需要在开始增加`\begin{equation}`，结束增加`\end{equation}`
    $$
    \begin{equation}
    a^2+b^2=c^2
    \end{equation}
    $$
- 手动编号

        $$
        x+y=z
        \tag{3}
        $$

    对应只需要增加`\tag{编号}`，可以自定义修改公式编号，推荐自动编号
    $$
    x+y=z
    \tag{3}
    $$

### 注释
`%`单行注释, 主要方便自己对公式的维护和阅读
$$
% test comment
x+y=z
\tag{3}
$$

## 引用
1. [在线文档](https://www.latexlive.com/help)
2. [LaTex在线编辑器](https://www.latexlive.com/)
3. [LaTeX详细教程+技巧总结](https://blog.csdn.net/NSJim/article/details/109066847)
4. [LaTeX数学公式-详细教程](https://blog.csdn.net/NSJim/article/details/109045914)


## 附录
常用希腊字母对照

| 序号 |     小写      |    LaTeX    |          读音          | 序号 |    大写     |   LaTeX   |    读音     |
| :--: | :-----------: | :---------: | :--------------------: | :--: | :---------: | :-------: | :---------: |
|  1   |   $\alpha$    |   \alpha    |        /ˈælfə/         |  22  |  $\sigma$   |  \sigma   |  /ˈsɪɡmə/   |
|  2   |    $\beta$    |    \beta    | /ˈbiːtə/, US: /ˈbeɪtə/ |  23  | $\varsigma$ | \varsigma |  /ˈsɪɡmə/   |
|  3   |   $\gamma$    |   \gamma    |        /ˈɡæmə/         |  24  |   $\tau$    |   \tau    | /taʊ, tɔː/  |
|  4   |   $\delta$    |   \delta    |        /ˈdɛltə/        |  25  | $\upsilon$  | \upsilon  | /ˈʌpsɪlɒn/  |
|  5   |  $\epsilon$   |  \epsilon   |       /ˈɛpsɪlɒn/       |  26  |   $\phi$    |   \phi    |    /faɪ/    |
|  6   | $\varepsilon$ | \varepsilon |       /ˈɛpsɪlɒn/       |  27  |  $\varphi$  |  \varphi  |    /faɪ/    |
|  7   |    $\zeta$    |    \zeta    |        /ˈzeɪtə/        |  28  |   $\chi$    |   \chi    |    /kaɪ/    |
|  8   |    $\eta$     |    \eta     |        /ˈeɪtə/         |  29  |   $\psi$    |   \psi    |   /psaɪ/    |
|  9   |   $\theta$    |   \theta    |        /ˈθiːtə/        |  30  |  $\omega$   |  \omega   | /oʊˈmeɪɡə/  |
|  10  |  $\vartheta$  |  \vartheta  |        /ˈθiːtə/        |  31  |  $\Gamma$   |  \Gamma   |   /ˈɡæmə/   |
|  11  |    $\iota$    |    \iota    |       /aɪˈoʊtə/        |  32  |  $\Delta$   |  \Delta   |  /ˈdɛltə/   |
|  12  |   $\kappa$    |   \kappa    |        /ˈkæpə/         |  33  |  $\Theta$   |  \Theta   |  /ˈθiːtə/   |
|  13  |   $\lambda$   |   \lambda   |        /ˈlæmdə/        |  34  |  $\Lambda$  |  \Lambda  |  /ˈlæmdə/   |
|  14  |     $\mu$     |     \mu     |         /mjuː/         |  35  |    $\Xi$    |    \Xi    | /zaɪ, ksaɪ/ |
|  15  |     $\nu$     |     \nu     |         /njuː/         |  36  |    $\Pi$    |    \Pi    |    /paɪ/    |
|  16  |     $\xi$     |     \xi     |      /zaɪ, ksaɪ/       |  37  |  $\Sigma$   |  \Sigma   |  /ˈsɪɡmə/   |
|  17  |      $o$      |      o      |       /ˈɒmɪkrɒn/       |  38  | $\Upsilon$  | \Upsilon  | /ˈʌpsɪlɒn/  |
|  18  |     $\pi$     |     \pi     |         /paɪ/          |  39  |   $\Phi$    |   \Phi    |    /faɪ/    |
|  19  |   $\varpi$    |   \varpi    |         /paɪ/          |  40  |   $\Psi$    |   \Psi    |   /psaɪ/    |
|  20  |    $\rho$     |    \rho     |         /roʊ/          |  41  |  $\Omega$   |  \Omega   | /oʊˈmeɪɡə/  |
|  21  |   $\varrho$   |   \varrho   |         /roʊ/          |      |             |           |             |


