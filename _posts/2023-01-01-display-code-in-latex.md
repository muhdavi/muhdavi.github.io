---
title: Display code in LaTeX
author: muhdavi
date: 2023-01-01 10:10:00 +0700
categories: [Blogging, Tutorial]
tags: [LaTeX]
render_with_liquid: false
---

This tutorial get from [Dr. Jörg Lenhard Blog](https://joerglenhard.wordpress.com/2011/03/10/display-xml-bpel-wf-code-in-latex/).

In my master thesis, I have quite a number of code listings. To make them easy to read and understand, I was looking for nice way for formatting and colouring the code. To this date, the no. 1 LaTeX package for environments to display code is the [listings](https://www.ctan.org/tex-archive/macros/latex/contrib/listings/) package. It provides a nice and highly configurable environment for displaying code and natively supports many different programming languages.

For these reasons, I also use it here. However, the code I need to display is of two XML-based orchestration languages, BPEL and Windows Workflow.

I am not so happy with the preliminary support for XML in listings and ended up configuring a listings style that provides a nicely structured environment for both of the languages:

```latex
\definecolor{lightgray}{rgb}{.9,.9,.9}
\definecolor{darkgray}{rgb}{.4,.4,.4}
\definecolor{forestGreen}{RGB}{34,139,34}
\definecolor{orangeRed}{RGB}{255,69,0}
\lstdefinestyle{workflowStyle}{
  language=XML,
  alsolanguage=bpel,
  alsolanguage=xaml,
  %Formatting
  basicstyle=\scriptsize,
  sensitive=true,
  showstringspaces=false,
  numbers=left,
  numberstyle=\tiny,
  tabsize=4,
  numbersep=3pt,
  extendedchars=true,
  xleftmargin=2em,
  lineskip=1pt,
  breaklines,
  captionpos=t,
  %Coloring
  backgroundcolor=\color{lightgray},
  morekeywords={BooleanExpression},
  alsoletter={:,,/,?},
  morestring=[b]{"},
  morecomment=[s]{&lt;!--}{--&gt;},keywordstyle=\color{forestGreen},
  identifierstyle=\color{blue}\ttfamily,
  stringstyle=\color{orangeRed}\ttfamily,
  commentstyle=\color{forestGreen}\ttfamily
}
```

The coloring more or less resembles the coloring of the Netbeans IDE. As you can see, there is quite a number of configurations and I won’t explain them in detail here. Just look them up in the documentation of the listings package or play with them to get a grasp of what they do.

Now, to color the attributes of the XML elements of the two languages differently, I defined these attributes as keywords of the two languages and overwrote the default XML configuration:

```latex
\lstdefinelanguage{bpel}{
  morekeywords={name,linkName,isolated,parallel,partnerLink,operation,portType,inputVariable,createInstance,
  variable,element,location,importType,partnerLinkType,myRole,messageType,properties,level,outputVariable,
  xmlns,version,encoding}
}
\lstdefinelanguage{xaml}{
  morekeywords={TypeArguments,Name,Default,DisplayName,OperationName,ServiceContractName,Key,AddressUri,
  CanCreateInstance, LogName, Message, MessageNumber, Expression,CorrelationHandle,Request}
}
\lstdefinelanguage{xml}{
  basicstyle=\small,
  sensitive=false,
}
```

Having these definitions in place, all you need is to use the style or define yourself a new environment:

```latex
\lstnewenvironment{workflow-code}[2]{
  \lstset{caption=#1,label=#2,style=workflowStyle}
}{}
```

An example code fragment in my thesis (demonstrating a trace extension of the OpenESB BPEL Service Engine) looks like this:

```latex
\begin{workflow-code}{caption}{label}
<assign name ="LogActivity">
    <trace>
        <log level="info" location="onComplete">
            <from variable="logMessage"/>
        </log>
    </trace>
    <!--copy a meaningful message to variable logMessage-->
</assign>
\end{workflow-code}
```

![Desktop View](/assets/posts/20230101/assign.png){: width="972" height="589" }
_Result from Example Code_

Full code

```latex
\documentclass{article}

\usepackage{color}
\usepackage{listings}

\definecolor{lightgray}{rgb}{.9,.9,.9}
\definecolor{darkgray}{rgb}{.4,.4,.4}
\definecolor{forestGreen}{RGB}{34,139,34}
\definecolor{orangeRed}{RGB}{255,69,0}

\lstdefinelanguage{bpel}{
  morekeywords={name,linkName,isolated,parallel,partnerLink,operation,portType,inputVariable,createInstance,
  variable,element,location,importType,partnerLinkType,myRole,messageType,properties,level,outputVariable,
  xmlns,version,encoding}
}
\lstdefinelanguage{xaml}{
  morekeywords={TypeArguments,Name,Default,DisplayName,OperationName,ServiceContractName,Key,AddressUri,
  CanCreateInstance, LogName, Message, MessageNumber, Expression,CorrelationHandle,Request}
}

\lstdefinelanguage{xml}{
  basicstyle=\small,
  sensitive=false,
}

\lstdefinestyle{workflowStyle}{
  language=XML,
  alsolanguage=bpel,
  alsolanguage=xaml,
  %Formatting
  basicstyle=\scriptsize,
  sensitive=true,
  showstringspaces=false,
  numbers=left,
  numberstyle=\tiny,
  tabsize=4,
  numbersep=3pt,
  extendedchars=true,
  xleftmargin=2em,
  lineskip=1pt,
  breaklines,
  captionpos=t,
  %Coloring
  backgroundcolor=\color{lightgray},
  morekeywords={BooleanExpression},
  alsoletter={:,<,>,/,?},
  morestring=[b]{"},
  morecomment=[s]{<!--}{-->},keywordstyle=\color{forestGreen},
  identifierstyle=\color{blue}\ttfamily,
  stringstyle=\color{orangeRed}\ttfamily,
  commentstyle=\color{forestGreen}\ttfamily
}

\lstnewenvironment{workflow-code}[2]{
\lstset{caption=#1,label=#2,style=workflowStyle}
}{}

\begin{document}

\begin{workflow-code}{caption}{label}
<assign name ="LogActivity">
    <trace>
        <log level="info" location="onComplete">
            <from variable="logMessage"/>
        </log>
    </trace>
    <!--copy a meaningful message to variable logMessage-->
</assign>
\end{workflow-code}

\end{document}
```
