title: Round Tag Cloud标签云插件实现彩色标签
tags:
  - wordpress
id: 97
categories:
  - 本站建设
date: 2014-03-15 10:17:10
---

本站使用了**wordpress**的[**Round Tag Cloud**](http://suhanto.net/rounded-tag-cloud-widget-wordpress/)标签云插件，但是使用的时候标签只有一种颜色，下面介绍怎么实现彩色标签云。

在主题函数文件functions.php中加入下面的代码：
<pre class="font-size:15 line-height:19 lang:php decode:true ">// 彩色标签云
function colorCloud($text) {
//用正则表达式搜索和替换标签内容
$text = preg_replace_callback('|&lt;a (.+?)&gt;|i', 'colorCloudCallback', $text);
return $text;
}
function colorCloudCallback($matches) {
$text = $matches[1];
$color = dechex(rand(0,16777215));
$pattern = '/style=(\'|\")(.*)(\'|\")/i';
$text = preg_replace($pattern, "style=\"color:#{$color};$2;\"", $text);
return "&lt;a $text&gt;";
}
//添加过滤器
add_filter('wp_tag_cloud', 'colorCloud', 1);</pre>
最后一句是增加过滤器，把代码提交到服务器，就看以看到彩色标签云效果了:

[![彩色](http://yunuschan.com/wp-content/uploads/2014/03/png9)](http://yunuschan.com/wp-content/uploads/2014/03/png9)