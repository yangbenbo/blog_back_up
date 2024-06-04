---
title: Latex
date: 2024-04-19 14:55:46
categories:
- Software
tags:
- Latex
mathjax: true
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

- [简介](#简介)
  - [vscode 配置Latex环境](#vscode-配置latex环境)
    - [安装LaTeX工具](#安装latex工具)
    - [安装LaTeX插件](#安装latex插件)
    - [配置LaTeX环境](#配置latex环境)
  - [伪代码](#伪代码)
    - [algorithm2e](#algorithm2e4)
    - [基本语法](#基本语法)

<!-- /code_chunk_output -->


# 简介
LaTeX (/ˈlɑːtɛk/ LAH-tek or /ˈleɪtɛk/ LAY-tek)[^1]是一种用于排版文档的软件系统. 很多论文排版使用, 这里主要介绍伪代码

## vscode 配置Latex环境
vscode配置好环境后可能遇到公式引用出错, 只需要再`build`就可以正常引用
### 安装LaTeX工具
安装TeX发行版, 这里选择的MiKTeX
### 安装LaTeX插件
安装LaTeX插件：在VSCode中安装插件如"LaTeX Workshop"
### 配置LaTeX环境
这里参考文章[Visual Studio Code (vscode)配置LaTeX](https://zhuanlan.zhihu.com/p/166523064)
打开vscode配置(`ctrl+,`),增加即可
```
//------------------------------LaTeX 配置----------------------------------
// 设置是否自动编译
"latex-workshop.latex.autoBuild.run": "never",
// 右键菜单
"latex-workshop.showContextMenu": true,
// 从使用的包中自动补全命令和环境
"latex-workshop.intellisense.package.enabled": true,
// 编译出错时设置是否弹出气泡设置
"latex-workshop.message.error.show": false,
"latex-workshop.message.warning.show": false,
// 编译工具和命令
"latex-workshop.latex.tools": [
    {
        "name": "xelatex",
        "command": "xelatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "pdflatex",
        "command": "pdflatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "-outdir=%OUTDIR%",
            "%DOCFILE%"
        ]
    },
    {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
            "%DOCFILE%"
        ]
    }
],
// 用于配置编译链
"latex-workshop.latex.recipes": [
    {
        "name": "XeLaTeX",
        "tools": [
            "xelatex"
        ]
    },
    {
        "name": "PDFLaTeX",
        "tools": [
            "pdflatex"
        ]
    },
    {
        "name": "BibTeX",
        "tools": [
            "bibtex"
        ]
    },
    {
        "name": "LaTeXmk",
        "tools": [
            "latexmk"
        ]
    },
    {
        "name": "xelatex -> bibtex -> xelatex*2",
        "tools": [
            "xelatex",
            "bibtex",
            "xelatex",
            "xelatex"
        ]
    },
    {
        "name": "pdflatex -> bibtex -> pdflatex*2",
        "tools": [
            "pdflatex",
            "bibtex",
            "pdflatex",
            "pdflatex"
        ]
    }
],
// 文件清理。此属性必须是字符串数组
"latex-workshop.latex.clean.fileTypes": [
    "*.aux",
    "*.bbl",
    "*.blg",
    "*.idx",
    "*.ind",
    "*.lof",
    "*.lot",
    "*.out",
    "*.toc",
    "*.acn",
    "*.acr",
    "*.alg",
    "*.glg",
    "*.glo",
    "*.gls",
    "*.ist",
    "*.fls",
    "*.log",
    "*.fdb_latexmk"
],
// 设置为onFaild 在构建失败后清除辅助文件
"latex-workshop.latex.autoClean.run": "onFailed",
// 使用上次的recipe编译组合
"latex-workshop.latex.recipe.default": "lastUsed",
// 用于反向同步的内部查看器的键绑定。ctrl/cmd +点击(默认)或双击
"latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",
// 使用 SumatraPDF 预览编译好的PDF文件
// 设置VScode内部查看生成的pdf文件
"latex-workshop.view.pdf.viewer": "external",
// PDF查看器用于在\ref上的[View on PDF]链接
"latex-workshop.view.pdf.ref.viewer": "auto",
// 使用外部查看器时要执行的命令。此功能不受官方支持。
"latex-workshop.view.pdf.external.viewer.command": "F:/SumatraPDF/SumatraPDF.exe", // 注意修改路径
// 使用外部查看器时，latex-workshop.view.pdf.external.view .command的参数。此功能不受官方支持。%PDF%是用于生成PDF文件的绝对路径的占位符。
"latex-workshop.view.pdf.external.viewer.args": [
    "%PDF%"
],
// 将synctex转发到外部查看器时要执行的命令。此功能不受官方支持。
"latex-workshop.view.pdf.external.synctex.command": "F:/SumatraPDF/SumatraPDF.exe", // 注意修改路径
// latex-workshop.view.pdf.external.synctex的参数。当同步到外部查看器时。%LINE%是行号，%PDF%是生成PDF文件的绝对路径的占位符，%TEX%是触发syncTeX的扩展名为.tex的LaTeX文件路径。
"latex-workshop.view.pdf.external.synctex.args": [
    "-forward-search",
    "%TEX%",
    "%LINE%",
    "-reuse-instance",
    "-inverse-search",
    "\"F:/Microsoft VS Code/Code.exe\" \"F:/Microsoft VS Code/resources/app/out/cli.js\" -r -g \"%f:%l\"", // 注意修改路径
    "%PDF%"
]
```

## 伪代码
Latex伪代码一般会接触到的包有algorithm、algorithmic、algorithmicx、algorithm2e这四种包[^2]

- algorithm用于给伪代码提供一个浮动体环境，防止其换页或其他因素导致的内容中断，从而跨页显示。
- algorithmic用于编辑伪代码的内容，一些for、while、if等语句通过该包中的命令进行编写。
- algorithmicx可以看作algorithmic的升级版，提供了一些自定义命令
- algorithm2e则是独立于algorithmic和algorithmicx的另一套伪代码环境，两套环境语法、排版上均不相同;自定义功能更多

这里主要介绍algorithm2e, 可以使用网页overleaf[^3]在线高效快速编译

### algorithm2e[^4]
#### 选项
算法引入
```
\usepackage[options ]{algorithm2e}
```
具体选项很多
- ruled 是让标题显示在上面, 否则算法的标题在下面
- linesnumbered 显示行号

### 基本语法
|                       |                            |
| --------------------- | -------------------------- |
| 代码                  | 含义                       |
| \\;                    | 在行末添加分号，并自动换行 |
| \caption{}            | 插入标题                   |
| \KwData               | 效果为：“Data:输入信息”    |
| \Kwln                 | 效果为：“ln:输入信息”      |
| \KwOut                | 效果为：“Out:输出信息”     |
| \KwResult             | 效果为：“Result:输出信息”  |
| \For {条件}           |                            |
| \If {条件}            |                            |
| \While {条件}         |                            |
| \tcc                  | /*注释*/                   |
| \tcp                  | //注释                     |
| \elf {条件}{肯定语句} |                            |

**if-then-else macros**
- \If, \Else, \ElseIf都是会以end结尾
- \uIf, \uElse, \uElseIf, 是不以end结尾的块级元素
- \lIf, \lElse, \lElseIf 是不以end为结尾的行内元素
在If-else结构中，\eIf 自带else（即 if 和 else 共用一个 end），而只是用 \If 和 \Else 的话则会多出一个end给Else。

示例如下

```
\documentclass[11pt,a4paper]{article}
\usepackage[margin=3cm]{geometry}
\usepackage[ruled,linesnumbered]{algorithm2e}
\begin{document}


%% H: algorithms cannot be cut
\begin{algorithm}[H]
\caption{An algorithm test} \label{alg:test}
%% This is needed if you want to add comments in
%% your algorithm with \Comment
\SetKwComment{Comment}{/* }{ */}

\KwIn{$x,n$}
\KwOut{$y = x^n$}
$y \gets 1$\;
$X \gets x$\;
$N \gets n$\;
\While{$N \neq 0$}
{
    \uIf{$N$ is even}
    {
        $X \gets X \times X$\;
        $N \gets \frac{N}{2} $ \Comment*[r]{This is a comment}
    }
    \ElseIf{$N$ is odd}
    {
        $y \gets y \times X$\;
        $N \gets N - 1$\;
    }  
}
\end{algorithm}


test ref Algorithm~\ref{alg:test}

\end{document}
```

算法标签`\label{alg:test}`, 方便在文章中引用, 引用`Algorithm~\ref{alg:test}`


[^1]: https://en.wikipedia.org/wiki/LaTeX
[^2]: https://en.wikibooks.org/wiki/LaTeX/Algorithms
[^3]: https://www.overleaf.com/project
[^4]: https://mirror.ox.ac.uk/sites/ctan.org/macros/latex/contrib/algorithm2e/doc/algorithm2e.pdf