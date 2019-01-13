title: 安卓笔记
date: 2016-08-27 21:35:44
tags:
  - ScrollView
  - Draw
categories:
  - Android
---

### 一个有用的表格库
这个库名叫[HelloCharts](https://github.com/lecho/hellocharts-android), 在曲线变化时能实现**动画效果**，还有强大的`Viewport`功能。

此外，使用得比较多的功能比较强大的还有[MPAndroidChart](https://github.com/PhilJay/MPAndroidChart)、[AChartEngine](http://www.achartengine.org/)等。

### 绘画颜色渐变的曲线
只要往`Paint`设置一个`Shader`即可，代码如下：

``` java
protected void onDraw(Canvas canvas)
{
    super.onDraw(canvas);
    Paint p = new Paint();
    // start at 0,0 and go to 0,max to use a vertical
    // gradient the full height of the screen.
    p.setShader(new LinearGradient(0, 0, 0, getHeight(), Color.BLACK, Color.WHITE, Shader.TileMode.MIRROR));
    canvas.drawPaint(p);
}
```
其中`Shader.TileMode`有三种`CLAMP`、`REPEAT`、`MIRROR`，具体可以查看安卓文档。

### 解决`ScrollView`与`HorizontalScrollView`间的滑动冲突
典型的应用场景如一个`HorizontalScrollView`包含于一个`ScrollView`时，如果手指在`HorizontalScrollView`上横向滑动，此间若在Y轴上稍有偏差，父View`ScrollView`则会毫不留情的截取滑动事件，子View的横向滑动就懵逼了。

感谢[SO大神的回答](http://stackoverflow.com/questions/2646028/android-horizontalscrollview-within-scrollview-touch-handling)，方法是重写`ScrollView`的`onInterceptTouchEvent`方法：

``` java
public class CustomScrollView extends ScrollView {
    private GestureDetector mGestureDetector;
    
    public CustomScrollView(Context context, AttributeSet attrs) {
        super(context, attrs);
        mGestureDetector = new GestureDetector(context, new YScrollDetector());
        setFadingEdgeLength(0);
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        return super.onInterceptTouchEvent(ev) && mGestureDetector.onTouchEvent(ev);
    }

    // Return false if we're scrolling in the x direction  
    class YScrollDetector extends SimpleOnGestureListener {
        @Override
        public boolean onScroll(MotionEvent e1, MotionEvent e2, float distanceX, float distanceY) {             
            return Math.abs(distanceY) > Math.abs(distanceX);
        }
    }
}
```

    

