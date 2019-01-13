title: 使Win32控制台程序assert()宏弹出窗口
tags:
  - assert
  - C/C++
id: 122
categories:
  - C/C++
  - Debug
date: 2014-03-26 11:31:51
---

写MFC程序时，用assert()还是挺舒服的，出错了能够自动弹出窗口，并且提示出错的文件以及位置。

近日在win32控制台上调试，发现assert()的位置只是一闪而过，根本没机会看到出错的位置（当然按Ctrl + F5启动也可以，但是不是Debug模式）。以下是根据[官方文档](http://support.microsoft.com/kb/111753/zh-cn)的解决方法，需要从新定义你自己的assert()宏，代码如下：
<pre class="lang:c++ decode:true">/* Compile options needed: none
   */ 

   #include &lt;windows.h&gt;
   #include &lt;stdio.h&gt;
   #include &lt;process.h&gt;

   // Prototype for the function called by the assert macro.
   void  ShowAssertion(void * expr, void * filename, unsigned lineno);

   // The following should all be on one line. It wraps around because
   // of its length.
   #define ASSERT(exp) (void)( (exp) || (ShowAssertion(#exp, __FILE__,
   __LINE__), 0) )

   static char AssertString[] = "Expression %s, File %s, Line %dn";

   void  ShowAssertion(void * expr, void * filename, unsigned lineno)
   {
       char buffer[256];

       sprintf( buffer, AssertString, expr, filename, lineno );
       MessageBox( NULL, buffer, "ASSERTION FAILED",
                   MB_OK | MB_ICONHAND | MB_TASKMODAL );

       abort();
   }</pre>
使用大写ASSERT()或者你自己的命名方式，不能使用assert()，不然编译会和系统的assert()冲突。这样就能在Win32控制台提示出错的详细位置。