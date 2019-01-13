title: OpenGL链接错误，无法解析的外部符号
tags:
  - OpenGL
id: 126
categories:
  - OpenGL
date: 2014-04-02 21:27:25
---

新编译一个OpenGL程序总是出现以下这个链接错误：

unresolved external symbol __imp____glutInitWithExit

加上以下这个宏就行了（比较难记，特此记下）：
<pre class="lang:c++ decode:true ">#define GLUT_DISABLE_ATEXIT_HACK</pre>
&nbsp;