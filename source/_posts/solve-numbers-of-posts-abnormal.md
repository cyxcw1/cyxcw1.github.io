title: terrifico主题文章归档与分类目录文章数量显示异常
tags:
  - terrifico
  - wordpress
  - 调试
id: 55
categories:
  - 本站建设
date: 2014-03-09 15:09:36
---

或者是因为SAE版本的wordpress框架的问题，terrifico主题的文章归档侧边栏与分类目录侧边栏显示文章数量的时候，会出现以下这种情况：

[![QQ截图20140305103833](http://yunuschan.com/wp-content/uploads/2014/03/QQ截图201403051038331-206x300.png)](http://yunuschan.com/wp-content/uploads/2014/03/QQ截图201403051038331.png)

&nbsp;

&nbsp;

如图，文章的数量与分类并没有在同一行显示。还是利用chrome的**审查元素**，不过这时是查看elements右边的**styles模块**，这个模块会显示css信息。在elements找到文章归档或者分类目录所对应的代码区域，styles则会定位到相应的该区域所对应的css文件:

[![归档](http://yusion-wordpress.stor.sinaapp.com/uploads/2014/03/归档-300x39.png)](http://yunuschan.com/wp-content/uploads/2014/03/png5)

&nbsp;

这里定位到sidebar.css。可以改变styles模块里相应的参数来调试。这里发现改变display参数能影响到上述的文章数量显示不正常现象。经过测试，<span style="color: #ff0000;">display：inline-block</span>可以修正上述现象。

定位到sidebar.css里的第81行，修正display参数，上传的SAE，一切正常：

[![本站](http://yunuschan.com/wp-content/uploads/2014/03/png6)](http://yunuschan.com/wp-content/uploads/2014/03/png6)

&nbsp;

&nbsp;