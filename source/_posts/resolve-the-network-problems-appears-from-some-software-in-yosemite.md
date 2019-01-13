title: 解决Yosemite下的多个软件同时出现的网络问题
tags:
  - DNS
  - wifi
  - yosemite
id: 177
categories:
  - OS X
date: 2015-02-11 23:07:43
---

这是我在威锋网发的一个[帖子](http://bbs.feng.com/read-htm-tid-8868836.html)，里面描述了最近在Yosemite（10.10.2）系统下遇到的网络问题。其实在妹子家的时候，这些症状已经出现，由于当时是用「手机wifi共享」上网，出现这些症状时并没有在意，直到回家后发现连上了正常的网络，发现这些症状仍然困扰着我，于是开始满世界找解困之法。

简要描述下症状：

*   「App Store」无法连接到服务器；
*   「网易云音乐」无法正常连接、登录、播放；
*   「印象笔记」无法正常同步、登录、更新；
*   连「战网客户端」也无法刷出前端的广告页面。
等等，还有很多情况，比如说iTunes的、GoAgent的等。但是能**以各种姿势正常浏览各种网页**。

尝试过的解决方法：

*   更换DNS。行不通，最终使用默认DNS。
*   刷新DNS缓存。依然不起作用。
*   重新安装各软件。还是不行。
*   清理各个软件的缓存以及浏览器的缓存。就要放弃治疗了。
还好没尝试「重装系统」的方法，虽然这似乎是一种万灵药，但是重装之后又要重装各种软件，时间损失太大了。

偶然间发现Yosemite版本有个wifi bug，虽然不确定我的系统的症状属于不属于这个Bug的范畴，抱着试一试的态度，最后网络居然神奇地恢复了。

## 解决方法

参照[这个网址](http://www.waerfa.com/os-x-yosemite-wi-fi-trouble-shooting)里的第二部分「删除 Mac OS X 的无线设置相关的 plist 文件」操作，即：

1.  关闭wifi；
2.  在Finder按“shift+cmmd+G”进入/Library/Preferences/SystemConfiguration/文件夹；
3.  如果存在下列plist文件，则删除（删除前记得先备份）：

*   com.apple.airport.preferences.plist
*   com.apple.network.identification.plist
*   com.apple.network.eapolclient.configuration.plist
*   com.apple.wifi.message-tracer.plist
*   NetworkInterfaces.plist
*   preferences.plist
4\.  最后重启系统，开启wifi。

就这样，删除wifi的配置后，系统会自动生成，也就回到了最初的wifi配置。问题得到了解决。