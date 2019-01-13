title: mac下配置goagent 3.0
tags:
  - Chrome
  - goagent
  - mac
id: 160
categories:
  - 工具
date: 2015-01-19 22:53:50
---

离开学校，也就离开教育网，6v也关了，离开了竟也不觉得很可惜。回家后用使用移动的宽带上网，google果然被墙了，还有stackoverflow内某些链接也被墙，比如说某些图片刷不出来。虽然google有host（我不会告诉你有这个找[谷歌host](http://serve.netsh.org/pub/gethosts.php)的好网站的），但是其他被墙的链接一一找host也是忒麻烦。于是使用goagent+chrome（插件SwitchySharp）的组合。

mac系统版本：OS X Yosemite 10.10.1

goagent版本：goagent 3.0

配置的步骤和网上文章描述的大体相似，这里只简要提一下，并重点描述配置这个版本配置过程中遇到的问题。

**一、goagent配置**

1\. 要使用goagent，你需要一个谷歌账号，点击下面的链接注册：

[https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

2\. 注册[GAE](https://appengine.google.com/)（Google App Engine）并获得一个APPID，步骤我就不详细说了。

3\. 下载goagent程序，现在的goagent挂载在[github上](https://github.com/goagent/goagent)了。

4\. 步骤3下载下来后应该得到一个压缩包，解压后，进入local文件夹，改写你的proxy.ini文件的以下部分中的"appid"以及"password"：
<pre class="theme:monokai lang:c++ decode:true">[gae]
enable = 1
appid = [你的APPID]
password = [你的密码]
path = /_gh/
mode = https
ipv6 = 0
sslversion = TLSv1
window = 7
cachesock = 1
headfirst = 1
keepalive = 0
obfuscate = 0
validate = 0
transport = 0
options =
regions =</pre>
5\. 由于“步骤6”上传你的个人信息的时候需要开启代理，所以本步骤中在你的local文件夹下用命令行执行下述代码先开启goagent：
<pre class="theme:monokai lang:sh decode:true">python proxy.py</pre>
6\. 进入到goagent的server文件夹上传你的个人信息。这里特别说明下，很多网站上说使用
<pre class="theme:monokai lang:sh decode:true ">python uploader.zip</pre>
来上传个人信息，但是我并没有找到[uploader.zip]这个文件。我使用下述代码上传：
<pre class="theme:monokai lang:sh decode:true">python uploader.py</pre>
上传过程中需要输入你的APPID、google账号以及密码，这里特别注意下你的google账号是不是开启了两步验证。如果开启了两步验证，而你还是输入你的google账号的密码，会出现：
<pre class="theme:monokai lang:sh decode:true">AttributeError: can't set attribute</pre>
的情况。这时，有两种解决办法：

(1) 关闭谷歌账号的两步验证。可到[这个页面](https://accounts.google.com/b/0/SmsAuthSettings#devices)关闭两步验证；

(2) 使用专用应用密码上传信息。通过下列方式可以查看专用密码：进入[你的谷歌账户](https://myaccount.google.com/)，在[登陆]模块找到[两步验证]，点击进去后找到[专用应用密码]--[管理专用应用密码]，按照提示生成你应用的专用密码。在“步骤6”上传信息输入密码时，就输入你的专用应用密码，你的个人信息即可上传成功。

7\. 由于“步骤5”开启了goagent，这时你需要把“步骤5”开启的goagent关闭，再进入到local文件夹，双击goagent-osx.command，就可以开启你的goagent代理。如果你想开机自动启动goagent，可以把goagent-osx.command拽托到docker固定，右键docker上的该图标点击：选项--登陆时打开，就可以实现goagent的开机启动。

二、chrome的配置

chrome的SwitchySharp插件配合goagent能让你的翻墙非常灵活，什么墙你想翻就翻，不想翻（如果你有Host）就撞，好吧这其实是过滤功能。

1\. 首先，给你的chrome下载[SwitchySharp插件](https://chrome.google.com/webstore/detail/proxy-switchysharp/dpplabbmogkhghncfbfdeeokoefdjegm?hl=zh-CN)（现在已经有了新版本SwitchySharp Omega，但是我没有使用，配置应该差不多）。

2\. 导入goagent的配置。进入该插件的[选项]，进入[导入导出]选项卡，点击[从文件中恢复]，在弹出的框中选择你的[goagent文件夹]/local/SwitchyOptions.bak文件。

3\. 配置你的goagent的过滤规则。进入[切换规则]选项卡，编辑你的过滤规则，以我的为例：

[![C343FCE6-592E-4D26-8A51-49E6263BD029](http://yunuschan.com/wp-content/uploads/2015/01/C343FCE6-592E-4D26-8A51-49E6263BD0291.jpg)](http://yunuschan.com/wp-content/uploads/2015/01/C343FCE6-592E-4D26-8A51-49E6263BD0291.jpg)

&nbsp;

URL模式支持正则表达式，增添起来很方便。 编辑完后记得[保存]。

4\. 这时点击你chrome上的SwitchySharp图标，选择[自动切换模式]，chrome则会根据你的过滤规则调用goagent翻墙。当然，若你想全程使用goagent翻墙，可以选择[GoAgent]情景模式翻墙。若不想翻墙了就选择[直接连接]。

5\. 大功告成了？反正我没有，我发现我不能成功登陆https服务器，chrome总提示“不是私密链接”。上网查明才发现是“证书”的问题。需要往chrome导入goagent的证书。步骤如下：

点击[设置]--[显示高级设置]--找到HTTPS/SSL下的[管理证书]并点击，会弹出一个框：

[![0E1B0BC0-3D8E-454C-9745-8F0D0F1E13C8](http://yunuschan.com/wp-content/uploads/2015/01/0E1B0BC0-3D8E-454C-9745-8F0D0F1E13C81.jpg)](http://yunuschan.com/wp-content/uploads/2015/01/0E1B0BC0-3D8E-454C-9745-8F0D0F1E13C81.jpg)

&nbsp;

“种类”框下选中“证书”，按[shift+command+i]组合键，在弹出的框选择[你个goagent文件夹]/local/CA.crt 并确定，就可以导入你的goagent证书。双击你的goagent证书，按照下图设置所有项目为[始终信任]：

[![EFC7E60B-914A-4873-8CD0-96AD87C92B27](http://yunuschan.com/wp-content/uploads/2015/01/EFC7E60B-914A-4873-8CD0-96AD87C92B271.jpg)](http://yunuschan.com/wp-content/uploads/2015/01/EFC7E60B-914A-4873-8CD0-96AD87C92B271.jpg)

&nbsp;

最后重启你的chrome。尝试上https服务器的网站，如香港谷歌，已经可以正常链接了。