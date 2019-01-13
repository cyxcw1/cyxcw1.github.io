title: 解决XeLaTex不能使用Minion Pro的斜体
tags:
  - LaTex
  - 字体
id: 39
categories:
  - LaTex
date: 2014-03-06 22:53:09
---

最近写简历要用到Minion Pro字体，编译完后发现无法使用这个字体的斜体，而fontspec包也不能自动转化斜体，包括粗体也无法正常显示。以下是我的解决方案。

要想正常使用斜体的话，在setsansfont的时候加多一个参数即可：
<pre class="lang:tex decode:true">setsansfont[SlantedFont=* Italic]{Minion Pro}</pre>
这样就能做到手动指定Italic样式的字体为斜体字。同理，如果想正常显示粗体字，也要手动指定：
<pre class="lang:tex decode:true">setsansfont[SlantedFont=* Italic, BoldFont=* Bold]{Minion Pro}</pre>
以上是设置非衬线字体，同样，设置衬线字体时（setmainfont）也可以用相同的伎俩来达到使用粗体斜体的目的。

设置好后的Minion Pro斜体效果如图：

[![QQ截图20140306225055](http://yunuschan.com/wp-content/uploads/2014/03/QQ截图201403062250551-300x140.png)](http://yunuschan.com/wp-content/uploads/2014/03/QQ截图201403062250551.png)

&nbsp;

&nbsp;