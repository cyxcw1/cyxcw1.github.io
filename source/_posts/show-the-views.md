title: 在文章标题下面显示文章的浏览数（基于terrifico主题）
tags:
  - terrifico
  - wordpress
id: 49
categories:
  - 本站建设
date: 2014-03-09 14:45:26
---

需求是要在标题下面的这个栏目显示阅读数，主题默认的时候无法显示，如下图：

[![QQ截图20140309140102](http://yunuschan.com/wp-content/uploads/2014/03/QQ截图201403091401021-300x16.png)](http://yunuschan.com/wp-content/uploads/2014/03/QQ截图201403091401021.png)

&nbsp;

用谷歌浏览器右键--&gt;审查元素，通过“icon-calender”找到这个栏目的文件：meta.php。在该文件&lt;/div&gt;前面加上一行即可：
<pre class="lang:php decode:true ">&lt;span&gt;&lt;i class="icon-comments-alt"&gt;&lt;/i&gt;&lt;?php if(function_exists('the_views')) { the_views(); } ?&gt;&lt;/span&gt;</pre>
用SVN上传到新浪服务器之后，显示效果如下：

[![QQ截图20140309141551](http://yunuschan.com/wp-content/uploads/2014/03/QQ截图201403091415511-300x16.png)](http://yunuschan.com/wp-content/uploads/2014/03/QQ截图201403091415511.png)

&nbsp;

如图，使用的是评论的图标（目前自己不会制作好的图标，囧，文章浏览数量也惨不忍睹，现在是挂在sinaapp上。继续努力）