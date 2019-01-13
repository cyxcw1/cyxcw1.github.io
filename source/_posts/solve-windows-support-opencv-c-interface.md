title: 解决在Windows对OpenCV C++接口的支持问题
tags:
  - MD
  - MDd
  - MT
  - MTd
  - OpenCV
id: 133
categories:
  - C/C++
  - OpenCV
date: 2014-04-22 22:01:19
---

本人的环境配置为Win7+VS2005+OpenCV2.3，最近被此问题，即OpenCV的C++接口频频出现运行时错误(runtime error)，在实验室的其他机子上也测试过，会出现如下情况：
<pre class="lang:c++ decode:true">//简单的一段代码
Mat image=imread("4_gray.bmp");</pre>
Mat与imread属于OpenCV的C++接口，运行这一段简单的代码时，会跳出内存错误，调试时根据调用堆栈跳到imread函数时，发现参数const string&amp; filename根本无法传入值，显示为&lt;bad ptr&gt;。如果使用OpenCV的C接口，如cvLoadImage等，则完全没有问题。

这样的C++接口问题还表现为，在使用solvePnP、calibrateCamera等C++接口函数时，由于这些函数定义时某些参数类型为InputArry，如果以vector&lt;Point3f&gt;类型（或者其他一些与vector有关类型）传入时，函数内部获取数据时，会出现数据丢失的情况，使用kind()获取类型时也无法正确获取。

<span style="color: #ff0000;">解决方法</span>：由于CMake编译OpenCV2.3源代码时，生成的工程的属性中有其中一项：C/C++-&gt;Code Generation-&gt;Runtime Library中配置的是**<span style="color: #ff0000;">Multi-threaded Debug DLL(MDd)</span>**，但是在现有的新建的OpenCV实验工程中，该项的配置为<span style="color: #ff0000;">Multi-threaded (/MT)</span>，所以会造成运行时错误。只要把该项改成<span style="color: #ff0000;">**Multi-threaded Debug DLL(MDd)**</span>即可：

[![solvec++opencv](http://yunuschan.com/wp-content/uploads/2014/04/solvec-opencv1.jpg)](http://yunuschan.com/wp-content/uploads/2014/04/solvec-opencv1.jpg)

&nbsp;

下面说说这四个选项的不同：

Multi-threaded (/MT)，静态链接方式，链接时会载入libcmt.lib；

Multi-threaded Debug (/MTd)，上者的debug版本，链接时载入libcmtd.lib；

Multi-threaded DLL (/MD)，动态链接方式，使用 msvcrt.lib 创建多线程 DLL；

Multi-threaded Debug DLL (/MDd)，上者的debug版本，使用 msvcrtd.lib创建多线程DLL；

&nbsp;

可以看出，四种不同的编译参数，载入的lib也不同，这样编译出来的dll和lib也会不同，所以OpenCV的实验工程要选择和编译OpenCV源代码时相同的编译参数，也要注意debug版本的工程要选择debug版本的编译参数，release版本的要选择release版本的编译参数。

还要拜谢stackoverflow的神贴及神回复啊，[神贴在此](http://stackoverflow.com/questions/9125817/opencv-imreadfilename-fails-in-debug-mode-when-using-release-libraries)。