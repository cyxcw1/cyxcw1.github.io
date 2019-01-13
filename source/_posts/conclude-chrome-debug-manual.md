title: Chrome调试方法总结
tags:
  - Chrome
  - 调试
id: 66
categories:
  - Debug
  - 本站建设
date: 2014-03-09 15:53:49
---

最近在折腾terrifico主题的时候，总结了一下网站调试的方法：

1.  最简单的和一般的程序调试差不多，修改源文件<span style="color: #ff0000;">输出信息</span>，比如php的是echo "Yusion Hello World";
2.  Chrome的审查元素功能很强大，用<span style="color: #ff0000;">element模块</span>可以定位你所要调试的区域；
3.  Chrome的<span style="color: #ff0000;">Styles模块</span>，显示的是.css样式文件，可以通过修改里面的参数来调试；
4.  Chrome的<span style="color: #ff0000;">Console模块</span>，可以监视网页各种事件所输出的信息，网页的一些错误可以在这里捕捉；
如下面两个图：

[![归档](http://yusion-wordpress.stor.sinaapp.com/uploads/2014/03/归档-300x39.png "element和Styles模块")](http://yunuschan.com/wp-content/uploads/2014/03/png5 "element和Styles模块")

图1

[![console](http://yunuschan.com/wp-content/uploads/2014/03/console1-300x41.png "console模块")](http://yunuschan.com/wp-content/uploads/2014/03/console1.png)

图2