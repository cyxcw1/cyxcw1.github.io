title: Visual Studio+VAssistX自动添加注释
tags:
  - VAssistX
  - Visual Studio
id: 120
categories:
  - 工具
date: 2014-03-22 16:30:14
---

## 1\. 增加函数头注释

右击函数名，然后依次点击<span style="color: #ff0000;">“Refacto”–&gt;“Document Method”</span>，这个时候函数头注释就会蹦出来，不过这个注释的格式是默认的，想修改注释格式，可以通过以下方法。

点击 <span style="color: #ff0000;">“VAssistX”–&gt;“Visual VAssistX Options”然后选择Suggestions,再点击“Edit VA Snippets”</span>。在打开的窗口中选择<span style="color: #ff0000;">Refactor Document Method</span>，在这就可以更改你的显示样式了。可以参照默认的注释格式来定制自己的注释：
<pre class="lang:c++ decode:true">//************************************
// Method:    $SymbolName$
// FullName:  $SymbolContext$
// Access:    $SymbolVirtual$$SymbolPrivileges$$SymbolStatic$
// Returns:   $SymbolType$
// Qualifier: $MethodQualifier$
// Parameter: $MethodArg$
//************************************</pre>

##  2\. 增加文件头注释

要想在文件头添加注释，需要把鼠标光标定位到VS编辑器的第一行，点击 <span style="color: #ff0000;">“VAssistX”–&gt;“Insert VA Snippet...”—&gt;“File Header Detail”</span>，即可增加文件头注释。默认的注释格式如下，可以通过点击<span style="color: #ff0000;">“VAssistX”–&gt;“Visual VAssistX Options”—&gt;“Advanced”—&gt;“Suggestions”—&gt;“Edit VA Snippets”</span>，选择你相应语言的<span style="color: #ff0000;">“File Header Detail”</span>修改。
<pre class="lang:c++ decode:true ">/********************************************************************
	created:	$DATE$
	created:	$DAY$:$MONTH$:$YEAR$   $HOUR$:$MINUTE$
	filename: 	$FILE$
	file path:	$FILE_PATH$
	file base:	$FILE_BASE$
	file ext:	$FILE_EXT$
	author:		$Author$

	purpose:	$end$
*********************************************************************/</pre>
&nbsp;

&nbsp;

&nbsp;