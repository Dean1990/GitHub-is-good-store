### 聊聊Android绘图

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### 从需求到API

例如，做一个股票趋势图

首先想到的应该是一条线，一条曲折的线，线就是路径，及对应Android的Path类，简单说一下Path类，它可以画线，可以画图形，还可以做为一条路径，按该路径的走势写字或画图，所以Path有个Direction的内部类，定义了方向。但是上面说的这些能力并不是Path类的，而是通过Path类才能实现的，因为Path类只代表路径。

Path类指明了路径，还需要一支笔，Paint类。笔是有多种多样的，就按需求来说定义一支笔出来

```java
Paint paint = new Paint();
paint.setStyle(Paint.Style.STROKE);//设置画笔的样式为STROKE(描边)，样式还有FILL(填充)和FILL_AND_STROKE(填充+描边)
paint.setColor(Color.RED);//设置画笔颜色
paint.setStrokeWidth(3);//设置画笔粗细
```

有画笔还是不够的，还需要画板才能作画，大多数情况下，我们是不需要自己创建画板的，因为大多数情况下，使用Android绘图都是在自定义View的时候，继承View重写的onDraw方法的入参就是一个现成的Canvas（画板）

```java
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
}
```

现在就差数据了，有了数据才能定义出路径来，有了路径，画笔才能在画板上按路径STROKE(描边)。

造数据，可以用个随机数来生成，在Y轴上一个范围内随机，在X轴上按一定刻度前进，应该就是想像中的趋势图了[完美]

```java
Path path = new Path();
Random random = new Random();
for (int i = 0;i < 50;i++) {

    int x = i * 20;
    int y = random.nextInt(300);
    if (i == 0)
        path.moveTo(x,y);
    else
        path.lineTo(x,y);
}
```

然后在画板上画出来

```java
canvas.drawPath(path,paint);
```

效果图

![效果图](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/stock_view_1.png?raw=true)

感觉太空了，加点鲜艳的颜色，给画板加个底色

```java
canvas.drawColor(Color.GREEN);
```

真是惊艳[滑稽]

![惊艳的效果图](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/stock_view_2.png?raw=true)

还差点什么，作为趋势图，就应该动起来，效果才好，怎么动呢，可以有好多方式，可以每隔一段时间调用一次onDraw方法，要用计时器吗？其实有更好的选择，View类有 invalidate() 方法，当被调用时，会触发onDraw方法，如果把invalidate()放在onDraw方法中，这就成了一个循环。那么，把path路径的绘制放到onDraw中，每次调用就绘制一段，再调用invalidate()方法，循环下去就成了动图，搞定。

回过头来，还要处理一个棘手的问题，上面的相互调用是个死循环，需要在onDraw方法中加入一些限制条件，以控制循环次数，有什么做条件呢？可以选择控制path的Y轴，当Y轴大于自定义的View的最右边界时，停止循环，问题解决。

但是，为了多聊一个API，这里采用另一种方式

```java
canvas.clipRect(left,top,right,bottom);
```

canvas有好多clipxxx的方法，都是裁剪成xxx的形状，让画板的大小慢慢变大，使path能显示在画板上的区域越来越大，好像趋势图在动，在前进。这个方法要注意的一点是裁剪只会影响这个方法调用之后的内容，之前canvas有多大，上面画了什么，都不会受到clip的影响，通过这一点可以把画板的底色设置在调用clip之前，保持底色大小不受裁剪的影响。

最终效果

![最终效果图](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/stock_view_3.gif?raw=true)

附上代码

```java
public class StockView extends View {
    
    Path path;
    Paint paint;
    int right;
    private int defaultSize = 400;
    
    public StockView(Context context){
        super(context);
    }
    public StockView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);

        path = new Path();
        Random random = new Random();
        for (int i = 0;i < 50;i++) {

            int x = i * 20;
            int y = random.nextInt(300);
            if (i == 0)
                path.moveTo(x,y);
            else
                path.lineTo(x,y);
        }

        paint = new Paint();
        paint.setStyle(Paint.Style.STROKE);
        paint.setColor(Color.RED);
        paint.setStrokeWidth(3);

    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);

        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);

        if (heightMode == MeasureSpec.AT_MOST){
            setMeasuredDimension(widthSize,defaultSize);
        }

    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        canvas.drawColor(Color.GREEN);

        canvas.clipRect(0,0,right,getBottom());
        canvas.drawPath(path,paint);

        if (right < getRight()){
            right+=2;
            invalidate();
        }
    }

}
```



#### 不是股票趋势图

画完才发现，这不是股票图吧，应该是那种吧，那种一个条，两头尖的，应该叫K线图。

研究一下，这个名叫蜡烛的东西，需要4个值，开盘，收盘等…姑且用一个数组表示吧，1234。

数据特点：

- 开盘和收盘不会高于最高
- 开盘和收盘不会低于最低
- 收盘高于开盘时为红色
- 开盘高于收盘时为绿色

根据这些特点，可以造出正确的数据

```java
private float[] createData() {
    double open = Math.random() * defaultSize;
    double close = Math.random() * defaultSize;
    return new float[]{(float) open, (float) close, 
                       (float) (Math.max(open, close) + Math.random() * (defaultSize - Math.max(open, close))), 
                       (float) (Math.min(open, close) - Math.random() * (Math.min(open, close)))};
}
```

制造一个蜡烛，并画到画板上

```java
private int kWidth = 18;//蜡烛的宽度
// data {开盘，收盘，最高，最低}
private void createK(Canvas canvas, int x, float[] data) {

    Path path = new Path();
    path.moveTo(x, transformYScale(data[2]));
    float tempMaxY = transformYScale(Math.max(data[0], data[1]));
    float tempMinY = transformYScale(Math.min(data[0], data[1]));
    path.lineTo(x, tempMaxY);
    path.addRect(x - kWidth / 2, tempMaxY, x + kWidth / 2, tempMinY, Path.Direction.CCW);
    path.moveTo(x, tempMinY);
    path.lineTo(x, transformYScale(data[3]));
    //改变颜色
    if (data[0] > data[1])
        paint.setColor(Color.GREEN);
    else
        paint.setColor(Color.RED);
    canvas.drawPath(path, paint);
}

//转换Y轴
private float transformYScale(float y) {
    return defaultSize - y;
}
```

由于Android的坐标轴是从左上角为(0,0)点，Y轴和笛卡尔坐标系相反，所以就有了transformYScale函数用于转换。

这个方式实现的蜡烛不能使用之前用到过的invalidate()方法，因为这次的path并不是一个整体的，当再次触发onDraw方式时，画板重置，之前的path绘制就被清除了，所以可以采用循环来处理这个问题。

为什么不用一个整体的path？因为画笔（paint）的颜色在不断的变化。

由于需要画一个矩形，所以画笔（paint）的样式需要设置成填充+描边

```java
paint.setStyle(Paint.Style.FILL_AND_STROKE);
```

最终效果

![最终效果](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/k_line_view_1.png?raw=true)

不够惊艳，加个黑色背景色，效果翻倍[开心]

![惊艳效果](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/k_line_view_2.png?raw=true)

确实很惊艳，也很惊讶，这股票趋势，能吓死人了吧[滑稽]。

附上代码

```java
public class KLineView extends View {

    Paint paint;
    private int defaultSize = 400;
    private int kWidth = 18;
    private int x = 50;

    public KLineView(Context context) {
        super(context);
    }

    public KLineView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);

        paint = new Paint();
        paint.setStyle(Paint.Style.FILL_AND_STROKE);
        paint.setStrokeWidth(2);

    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);

        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);

        if (heightMode == MeasureSpec.AT_MOST) {
            setMeasuredDimension(widthSize, defaultSize);
        }

    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        canvas.drawColor(Color.BLACK);

        while (x < getRight()) {
            createK(canvas, x, createData());
            x += 50;
        }

    }

    private float[] createData() {
        double open = Math.random() * defaultSize;
        double close = Math.random() * defaultSize;
        return new float[]{(float) open, (float) close,
                (float) (Math.max(open, close) + Math.random() * (defaultSize - Math.max(open, close))),
                (float) (Math.min(open, close) - Math.random() * (Math.min(open, close)))};
    }


    // data {开盘，收盘，最高，最低}
    private void createK(Canvas canvas, int x, float[] data) {

        Path path = new Path();
        path.moveTo(x, transformYScale(data[2]));
        float tempMaxY = transformYScale(Math.max(data[0], data[1]));
        float tempMinY = transformYScale(Math.min(data[0], data[1]));
        path.lineTo(x, tempMaxY);
        path.addRect(x - kWidth / 2, tempMaxY, x + kWidth / 2, tempMinY, Path.Direction.CCW);
        path.moveTo(x, tempMinY);
        path.lineTo(x, transformYScale(data[3]));
        if (data[0] > data[1])
            paint.setColor(Color.GREEN);
        else
            paint.setColor(Color.RED);
        canvas.drawPath(path, paint);
    }

    private float transformYScale(float y) {
        return defaultSize - y;
    }
}
```



------

2018/3/15.

*Dean.King*

**Beijing**

