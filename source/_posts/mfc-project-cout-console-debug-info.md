title: MFC工程输出控制台调试信息
tags:
  - C/C++
  - MFC
id: 129
categories:
  - C/C++
  - MFC
date: 2014-04-06 16:24:23
---

MFC工程的调试方法有很多种，传统的断点、TRACE、MessageBox，也有人习惯了使用cout/printf等控制台工程下常用的调试方法，这些方法在MFC工程中不能够直接使用，需要使用如下代码作流重定向：
<pre class="lang:c++ decode:true ">/* 重定向 */
AllocConsole();
freopen("CON", "w", stdout);

/* 输出你的调试信息 */

/* 恢复流 */
FreeConsole();</pre>
&nbsp;