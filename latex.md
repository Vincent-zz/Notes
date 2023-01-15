基本语法：`\操作[操作的相关值]{对象/值/内容}` 

- `\documentclass[UTF8]{ctexart}`，ctexart中文文章类型 
- 前言：begin之前的内容
  - `usepackage{o}`指定宏
  - `\title{标题}`；`\author{作者名}`；`\date{日期}`（`\date{\today}`）
- `\begin{document}`正文开始；`\end{document}`正文结束 
- `\maketitle`在正文中显示前言里设置的标题、作者名、日期
- `\section{章节名}`，`\subsection{子章节名}`，`\subsubsection{孙章节名}`章节名后自带空行；以“1 章节名”（居中）、“1.1 子章节名”、“1.1.1 孙章节名”的形式呈现 
- 换行：双回车 
- `\texbf{content}`，bold font；`\textit{content}`，italic；`\underline{content}`
- 插入图片：宏`graphicx`
  - `includegraphics[参数]{图片文件名}`
  - figure环境及一些操作，例：
```
\begin{figure}[htbp]
\centering
\includegraphics[width = 0.5\textwidth]{图片}
\caption{图片标题}
\end{figure}
``` 

- 列表、itemize环境（item以`·`开头），例：
```
\begin{itemize}
\item content1
\item content2
\item content3
\end{itemize}
```
- 列表、enumerate环境（item以数字开头），例：
```
\begin{enumerate}
\item content1
\item content2
\item content3
\end{enumerate}
```
- 公式
  - `$content$`
  - 单独成段：`equation`环境