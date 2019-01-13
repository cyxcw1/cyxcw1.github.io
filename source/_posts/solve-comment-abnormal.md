title: 解决SAE上的terrifico主题评论不正常
tags:
  - terrifico
  - wordpress
id: 60
categories:
  - 本站建设
date: 2014-03-09 15:34:23
---

现象1：点击reply，只看到滚动条在动，无法正常显示评论框体。

解决：**Chrome审查元素--&gt;Console模块**，观察到点击reply时，提示addComment没有定义。在element里看到reply绑定的click函数为<span style="color: #ff0000;">addComment</span>。搜索文件，该函数已经在javascript文件comment-reply.js文件里定义，并在framework-functions.php文件里面导入该JS：
<pre class="lang:php decode:true">if ( is_singular() &amp;&amp; comments_open() &amp;&amp; get_option( 'thread_comments' ) ){
	wp_enqueue_script( 'comment-reply' );
}</pre>
应该是这一堆判断条件没有通过，导致JS文件无法导入。网上也有人反映<span style="color: #ff0000;">is_singual()</span>函数判断有bug。于是删除了这些判断条件，直接导入JS文件：
<pre class="lang:php decode:true">wp_enqueue_script( 'comment-reply' );</pre>
把修改后的文件上传到服务器，一切正常：

[![评论正常](http://yusion-wordpress.stor.sinaapp.com/uploads/2014/03/评论正常-300x257.png)](http://yunuschan.com/wp-content/uploads/2014/03/png7)

&nbsp;

&nbsp;

现象2：在有评论（评论数量不为0）的文章里，无法显示评论框体。

解决：查看comment.php文件，已经有了显示评论窗体的代码：
<pre class="lang:php decode:true">&lt;?php comment_form(); ?&gt;</pre>
胡乱调试下，发现加个<span style="color: #ff0000;">echo</span>能把问题解决：
<pre class="lang:php decode:true">&lt;?php 
	echo "Hi man";	
	comment_form(); 
?&gt;</pre>
虽然看起来有点囧，为嘛上面会蹦出来一个Hi man?（本来想echo "n"，或者一个笑脸":)"，很不幸不能解决问题）

[![有评论](http://yusion-wordpress.stor.sinaapp.com/uploads/2014/03/有评论-286x300.png)](http://yunuschan.com/wp-content/uploads/2014/03/png8)