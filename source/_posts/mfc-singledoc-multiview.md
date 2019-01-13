title: MFC实现单文档多视图
tags:
  - C/C++
  - MFC
  - 界面设计
id: 26
categories:
  - MFC
date: 2014-02-22 23:23:36
---

搜集了一下网上的有关MFC单文档多视图的资料，大部分作者的代码实现的是两个视图在不同的时间显示并切换（参考资料http://www.cnblogs.com/cy163/archive/2011/01/09/1931451.html），以及使用CSplitterWnd来实现同时显示两个视图。

我目前的需求是：结合以上两种情况，能分别显示视图1与视图2，也能两个同时显示。以下是程序的设计。

**主要思想**是：分别建立一个CSplitterWnd的对象以及视图1以及视图2的两个对象，用户要求单独显示的时候就根据需求显示视图1或者视图2的对象，要同时显示两个视图的话就显示CSplitterWnd的对象。这个思想也可以扩展到多视图的显示。

**步骤1**：在MainFrm里增加这三个成员。
<pre class="lang:c++ decode:true">	CSplitterWnd	m_SplitWnd;
	CsplittestView	*m_SView;
	CSecView		*m_SecView;</pre>
其中CsplittestView是系统自动生成的View，CSecView是自己写的继承于CsplittestView的View，当然也可以用其他的如Edit之类的。

**步骤2**：重载MainFrm里的OnCreateClient函数。
<pre class="lang:c++ decode:true">BOOL CMainFrame::OnCreateClient(LPCREATESTRUCT lpcs, CCreateContext* pContext)
{
	// TODO: Add your specialized code here and/or call the base class

	/* 建立Splitter对象并隐藏，这个可以根据用户的需求而定，如果窗体第一次
	 * 出现时要显示的是双视图，可以ShowWindow(SW_SHOW)
	 */
	m_SplitWnd.CreateStatic(this,1,2);
	m_SplitWnd.CreateView(0,0,RUNTIME_CLASS(CsplittestView),CSize(400,30),pContext);
	m_SplitWnd.CreateView(0,1,RUNTIME_CLASS(CSecView),CSize(30,30),pContext);
	m_SplitWnd.ShowWindow(SW_HIDE);

	/* 建立视图1，并显示。注意需要显示的视图的ID号必须是AFX_IDW_PANE_FIRST */
	CRect rect;
	m_SView = new CsplittestView();
	m_SView-&gt;Create(NULL, NULL, WS_CHILD, rect, this, AFX_IDW_PANE_FIRST, pContext);
	m_SView-&gt;ShowWindow(SW_SHOW);

	/* 建立视图2并隐藏 */
	CRect rect2;
	m_SecView = new CSecView();
	m_SecView-&gt;Create(NULL, NULL, WS_CHILD, rect2, this, AFX_IDW_PANE_FIRST + 2, pContext);
	m_SecView-&gt;ShowWindow(SW_HIDE);

	//return CFrameWnd::OnCreateClient(lpcs, pContext);
	return TRUE;
}</pre>
**步骤3**：建立众视图之后，就可以增加切换视图的响应函数。
<pre class="lang:c++ decode:true">/* 切换到视图1的响应函数 */
void CMainFrame::OnEditSwitchtoone()
{
	// TODO: Add your command handler code here
	/* 注意调整ID号 */
	m_SecView-&gt;SetDlgCtrlID(AFX_IDW_PANE_FIRST + 1);
	m_SView-&gt;SetDlgCtrlID(AFX_IDW_PANE_FIRST);

	/* 显示 */
	m_SView-&gt;ShowWindow(TRUE);
	m_SecView-&gt;ShowWindow(FALSE);
	m_SplitWnd.ShowWindow(FALSE);

	/* 刷新 */
	this-&gt;RecalcLayout();
}</pre>
&nbsp;
<pre class="lang:c++ decode:true">/* 切换到视图2的响应，同视图1 */
void CMainFrame::OnEditSwitchtotwo()
{
	// TODO: Add your command handler code here

	m_SView-&gt;SetDlgCtrlID(AFX_IDW_PANE_FIRST + 1);
	m_SecView-&gt;SetDlgCtrlID(AFX_IDW_PANE_FIRST);

	m_SecView-&gt;ShowWindow(TRUE);
	m_SView-&gt;ShowWindow(FALSE);
	m_SplitWnd.ShowWindow(FALSE);

	this-&gt;RecalcLayout();
}</pre>
&nbsp;
<pre class="lang:c++ decode:true">/* 同时显示两个视图的函数 */
void CMainFrame::OnEditShowall()
{
	// TODO: Add your command handler code here

	/* 要切换回视图1才能显示，目前还不知道问题出在什么地方 */
	OnEditSwitchtoone();
	m_SView-&gt;SetDlgCtrlID(AFX_IDW_PANE_FIRST + 1);
	m_SecView-&gt;SetDlgCtrlID(AFX_IDW_PANE_FIRST + 2);

	m_SView-&gt;ShowWindow(FALSE);
	m_SView-&gt;ShowWindow(FALSE);
	m_SplitWnd.ShowWindow(TRUE);

	this-&gt;RecalcLayout();
}</pre>
**实验**：在两个View中填写OnDraw函数。
<pre class="lang:c++ decode:true">void CsplittestView::OnDraw(CDC* pDC)
{
	CsplittestDoc* pDoc = GetDocument();
	ASSERT_VALID(pDoc);
	if (!pDoc)
		return;

	// TODO: add draw code for native data here
	pDC-&gt;TextOut(400,300,"First View");
	pDC-&gt;TextOut(400,320,pDoc-&gt;GetTitle());
}</pre>
&nbsp;
<pre class="lang:c++ decode:true">void CSecView::OnDraw(CDC* pDC)
{
	CDocument* pDoc = GetDocument();
	// TODO: add draw code here

	pDC-&gt;TextOut(400,300,"Second View");
	pDC-&gt;TextOut(400,320,pDoc-&gt;GetTitle());
}</pre>
点击各种视图切换后的效果如下。

视图1：

[![QQ截图20140222232034](http://yunuschan.com/wp-content/uploads/2014/02/QQ截图201402222320341-300x183.png)](http://yunuschan.com/wp-content/uploads/2014/02/QQ截图201402222320341.png)

&nbsp;

视图2：

[![QQ截图20140222232048](http://yunuschan.com/wp-content/uploads/2014/02/QQ截图201402222320481-300x183.png)](http://yunuschan.com/wp-content/uploads/2014/02/QQ截图201402222320481.png)

双视图：

[![QQ截图20140222232059](http://yunuschan.com/wp-content/uploads/2014/02/QQ截图201402222320591-300x183.png)](http://yunuschan.com/wp-content/uploads/2014/02/QQ截图201402222320591.png)