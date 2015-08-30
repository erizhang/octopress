---
layout: post
title: "How to support LaTeX Chinese editing in TeXstudio under Mac"
date: 2015-08-15 22:01:06 +0800
comments: true
categories: Programming
author: Eric Zhang
---


These days, I'm trying to edit some Chinese text documents edit in LaTeX language. It's not a problem in Windows since Chinese font support, and Windows OS is popular used in Chinese forum. For English supporting is not a problem in LaTeX, but Chinese edit problem I encounter under Mac OS. I'm trying to make the notes to remember the steps here in case of somebody needs it as well.

### Step 1.  Choose good editor and compiler package
As friend recommend, I used [TeXstudio](http://www.texstudio.org "texstudio") for my LaTeX document editing, this tool was quite good when I used under Windows, I suppose it will do great job under Mac OS as well. And Install the [MacTeX](http://www.tug.org/mactex/ "mactex") the LaTeX compiling supporting.

### Step 2. Change the `PDFLaTeX` command to `XeLaTeX`
The first problem I encounter is when I finish a sample document editing, and tried to compile it, TeXstudio will use the default command `PDFLaTeX` to compile it. It works under PC, but failed under Mac system. So the first thing you need to set the default command to `XeLaTeX`. 
In TeXstudio, you can change the option via "Preferences" -> "Build" -> "Default compiler", select the value as `XeLaTeX`. Now if you compile this document, it works fine:
```LaTeX
\documentclass{article}
\title{Hello World}
\author{Mac}
\date{2015, July 4}
\begin{document}
	\maketitle
	This is my first sound.
\end{document}
```
and the result is like this:

![Alt text](/images/2015-08-15-how-to-support-latex-chinese-editing-in-texstudio-under-mac/first-result.png "works well")

### Step 3. Support Chinese words editing
But if we change the English content to Chinese, it doesn't work well, such like this:
```LaTeX
\documentclass{article}
\title{你好，世界}
\author{Mac}
\date{2015, July 4}
\begin{document}
	\maketitle
	这是我呐喊出来的第一句话。
\end{document}
```
Except the English words can be displayed well, everything is mass, unreadable. So we need use `ctex` package to support it. so add the `\usepackage{ctex}` after `\documentclass{article}`, the result is still bad. So I realize the problem might be caused by the fonts supporting. And then I inserted these lines before `\begin{document}`, the document like this:
```LaTeX
\documentclass{article}
\usepackage{ctex}
\title{你好，世界}
\author{Mac}
\setCJKmainfont{Kaiti TC Regular}
\setCJKsansfont{Songti TC Regular}
\setCJKmonofont{Heiti TC Regular}
\date{2015, July 4}
\begin{document}
	\maketitle
	这是我呐喊出来的第一句话。
\end{document}
```
and change the encoding to `UTF-8`

![Alt text](/images/2015-08-15-how-to-support-latex-chinese-editing-in-texstudio-under-mac/encoding.png =500x "select encoding")

then the final result likes this:

![Alt text](/images/2015-08-15-how-to-support-latex-chinese-editing-in-texstudio-under-mac/chinese-result.png =500x "Chinese result")

### Step 4. How do I know the font's name
As you see, I insert these line about the font supporting, but you may ask why I know the Chinese font name. Yes, it's tricky, :p
Firstly, I installed Microsoft Office, and I open the Microsoft Word, you can find the font name from the font dropbox, after you selected a font, the font text box will show its name. See, that's why I know the font name exactly.

![Alt text](/images/2015-08-15-how-to-support-latex-chinese-editing-in-texstudio-under-mac/font-select.png =400x "font select")


### In the eend
Here I give an example of the Chinese words editing, hope it's useful for your reference.
```LaTeX
\documentclass{ctexart}
%\usepackage{ctex}
\usepackage{xeCJK}
\setCJKmainfont{Kaiti TC Regular}
\setCJKsansfont{Songti TC Regular}
\setCJKmonofont{Heiti TC Regular}
 
\begin{document}
 
\tableofcontents
 
\begin{abstract}
这是在文件的开头的介绍文字.本文的主要话题的简短说明.
\end{abstract}
 
\section{ 前言 }
在该第一部分中的一些额外的元素可以被添加。巴贝尔包将采取的翻译服务.
 
\section{关于数学部分}
在本节中的一些数学会使用数学模型含中文字符显示。
 
這是一個傳統的中國文字
 
\end{document}
```

-EOF-
