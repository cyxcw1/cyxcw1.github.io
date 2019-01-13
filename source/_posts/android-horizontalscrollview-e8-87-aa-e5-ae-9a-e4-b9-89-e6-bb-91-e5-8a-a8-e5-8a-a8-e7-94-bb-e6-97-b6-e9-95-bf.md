title: Android HorizontalScrollView自定义滑动动画时长
tags:
  - HorizontalScrollView
id: 220
categories:
  - Android
date: 2015-06-27 14:28:04
---

在使用**HorizontalScrollView**时，需要控制其滑动速度，但是发现并不能自定义该控件滑动时的时长，而_SmoothScrollTo(...)_也满足不了需求，源代码里把滑动_duration_写死了_250ms_。下面介绍自定义**HorizontalScrollView**滑动动画时长的方法。

HorizontalScrollView的滑动是通过其**OverScroller**类型的私有成员**mScroller**的`startScroll(int startX, int startY, int dx, int dy, int duration)`实现的，而当`startScroll(...)`被调用时，**duration**被写死为**DEFAULT_DURATION=250**。

我们要做的就是，自定义一个`HorizontalScrollView`，并通过反射的方法获取**mScroller**，当你需要执行滑动时，执行_mScroller.startScroll(...)_即可。

看看代码：

<pre class="lang:java decode:true">private OverScroller myScroller;     

private void init()
{
    try
    {
        Class parent = this.getClass();
        do
        {
            parent = parent.getSuperclass();
        } while (!parent.getName().equals("android.widget.HorizontalScrollView"));

        Log.i("Scroller", "class: " + parent.getName());
        Field field = parent.getDeclaredField("mScroller");
        field.setAccessible(true);
        myScroller = (OverScroller) field.get(this);

    } catch (NoSuchFieldException e)
    {
        e.printStackTrace();
    } catch (IllegalArgumentException e)
    {
        e.printStackTrace();
    } catch (IllegalAccessException e)
    {
        e.printStackTrace();
    }
}

public void customSmoothScrollBy(int dx, int dy)
{
    myScroller.startScroll(scrollX, getScrollY(), dx, 0, 500);
    invalidate();
}</pre>

上述代码把_duration_设为500ms，用户可以根据自己的需求，设定自己需要的滑动时长。