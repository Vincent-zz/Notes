基本语法：`\操作[操作的相关值]{对象/值/内容}` 

- `\documentclass[UTF8]{ctexart}`，ctexart中文文章类型 
- 前言：begin之前的内容
  - ``指定宏
  - `\title{标题}`；`\author{作者名}`；`\date{日期}`（`\date{\today}`）
- `\begin{document}`正文开始；`\end{document}`正文结束 
- `\maketitle`在正文中显示前言里设置的标题、作者名、日期
- `\section{章节名}`，`\subsection{子章节名}`，`\subsubsection{孙章节名}`章节名后自带空行；以“1 章节名”（居中）、“1.1 子章节名”、“1.1.1 孙章节名”的形式呈现 
- 换行：双回车 
- `\texbf{content}`，bold font；`\textit{content}`，italic；`\underline{content}`
