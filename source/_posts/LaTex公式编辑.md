---
title: LaTex公式编辑
date: 2023-07-11 06:37:51
categories:
- Blog
tags:
---

想要在文档中快速敲出好看的公式，[LaTex](https://www.latex-project.org/)必不可少。LaTeX系统是一种可以处理排版和渲染的标记语言，一种基于TEX的排版系统，由美国计算机科学家莱斯利·兰伯特在20世纪80年代初期开发，非常适用于生成高印刷质量的科技和数学、物理文档。

## 基本使用
- `$`: 左对齐公式
- `$$`: 居中对齐公式
- `_`、`^`: 下标，上标

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
4. [LaTeX数学公式-详细教程](https://blog.csdn.net/NSJim/article/details/109066847)


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


