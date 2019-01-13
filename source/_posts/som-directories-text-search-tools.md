title: 几款文件夹内文本搜索工具
tags:
  - ack
  - EditPlus
  - grep
  - sublime
  - Visual Studio
id: 102
categories:
  - 工具
date: 2014-03-15 11:19:49
---

## 1\. Visual Studio

Visual Studio中有非常方便的文本搜索功能，能够轻松定位到一个工程中一个函数或者单单是一个字符串的位置。当然，如果你并没有建立一个工程，而单单想阅读他人的代码，Visual Studio也提供非常方便的文本搜索功能。

Edit-&gt;Find and Replace-&gt;Find in Files（或者快捷键Ctrl+Shift+F）即可打开这个功能，可以根据自己的需求指定搜索的字符串、文件夹以及文件类型（能使用正则表达式）进行搜索：

[![vs](http://yunuschan.com/wp-content/uploads/2014/03/vs1.png)](http://yunuschan.com/wp-content/uploads/2014/03/vs1.png)

&nbsp;

## 2\. EditPlus

EditPlus也提供文本搜索的功能，点击“搜索”-&gt;“在文件中查找”，搜索的方法和效果基本和上面介绍的Visual Studio相同，记得把“包含子文件夹”的勾打上：

[![ep](http://yunuschan.com/wp-content/uploads/2014/03/ep1.png)](http://yunuschan.com/wp-content/uploads/2014/03/ep1.png)

&nbsp;

## 3\. Sublime Text

Sublime Text中也提供非常方便的文本搜索功能，而且还能很直观地进行预览，点击Find-&gt;Find in Files就可以打开次功能：

[![subl](http://yunuschan.com/wp-content/uploads/2014/03/subl1.png)](http://yunuschan.com/wp-content/uploads/2014/03/subl1.png)

其中...按钮也能提供非常灵活的选项。搜索出来的文本效果如图：

[![st](http://yunuschan.com/wp-content/uploads/2014/03/st1.png)](http://yunuschan.com/wp-content/uploads/2014/03/st1.png)

双击相应的高亮文本或者该文本所在的行，能够跳到相应的文件。

## 4\. Linux中的grep工具

虽然Linux中也有Sublime Text，但是感觉grep工具更加强大。使用grep的基本方式：
<pre class="lang:sh decode:true">grep -r text files</pre>
&nbsp;

-r表示递归搜索，grep不仅能搜索相应的文本，而且如果文件的名称包含该“text”字符串，也能显示出来，而且能非常方便得使用Linux中的管道功能：

[![grep使用](http://yunuschan.com/wp-content/uploads/2014/03/grep使用1.png)](http://yunuschan.com/wp-content/uploads/2014/03/grep使用1.png)

&nbsp;

## 5\. Linux中的ack工具

这个工具用得不是很熟，基本的用法和grep差不多。详见极客范中的[这篇文章](http://www.geekfan.net/6881/)，其中还包括了很多高级用法。