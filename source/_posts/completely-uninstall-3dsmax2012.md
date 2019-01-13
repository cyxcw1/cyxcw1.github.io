title: 完全卸载3ds Max 2012
tags:
  - 3ds Max
id: 137
categories:
  - 工具
date: 2014-04-23 20:26:05
---

最近的一次重装系统后，第一次重装3ds Max 2012，发现有一些组件安装失败造成了整个安装失败，于是选择了再次重装，发现之前安装失败的组件，本次提示安装成功，于是本次只安装了简单的composite组件，造成本次安装完后程序无法运行。到控制面板也找不到3ds Max的主要卸载程序，只找到了composite的卸载程序，还有几个Library，卸载了重装了还是不行。

按照网上的方法，即删除文件夹清空有关注册表信息，还是不行，于是发现了自己的<span style="color: #ff0000;">卸载方法</span>。

打开安装解压缩文件夹，打开Setup32.ini（我的系统是32位），发现有以下这段代码：
<pre class="lang:c++ decode:true ">[SETUP]
SETUP_CODE={A7100F83-91BE-47d0-8B0C-F457428976F4}
TITLE=Autodesk® 3ds Max 2012
LOG=%tmp%3ds Max 2012 Setup.log
EVALMSI=SetupReseval.msi
PRIMARY_MSI=x86max3dsMax2012.msi</pre>
于是到x86max文件夹中打开<span style="color: #ff0000;">3dsMax2012.msi</span>，点击“Remove”。上述完成后再到控制面板把Autodesk相关的东西卸载，这时再重新安装3ds Max 2012，即可安装成功。