title: sublime text固定在ubuntu任务栏
tags:
  - sublime
  - ubuntu
id: 24
categories:
  - 工具
date: 2014-02-17 20:32:19
---

在ubuntu13.10下载了sublime text 2，发现无法固定在ubuntu的任务栏，在网上找了各种方法，特此记录。

建立一个文件sublime.desktop，复制以下代码进去：
<pre class="lang:sh decode:true">[Desktop Entry]
Name=Sublime Text 2
Comment=Sublime Text 2
Exec=/home/chan/"Sublime Text 2"/sublime_text
Icon=/home/chan/Sublime Text 2/Icon/48x48/sublime_text.png
Terminal=false
Type=Application
Categories=Application;Development;
StartupNotify=true</pre>
接着把这个文件复制到系统的application中：
<pre class="lang:sh decode:true">sudo cp sublime.desktop /usr/share/applications/</pre>
建立链接，这样就可以在命令行中调用：
<pre class="lang:sh decode:true">ln -s /usr/local/lib/Sublime Text 2/sublime_text /usr/bin/sublime</pre>
其中，/usr/local/lib/Sublime Text 2/ 为你sublime的解压路径。这样你就可以在命令行以sublime命令调用sublime text编辑器了。