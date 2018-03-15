### 聊聊Android绘图

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### 从需求到API

- 比如，做一个股票趋势图

  首先想到的应该是一条线，一线曲折的线，线就是路径，及对应Android的Path类，简单说一下Path类，它可以画线，可以画图形，还可以做为一条路径，按该路径的走势写字或画图，所有Path有个Direction的内部类，定义了方向。但是上面说的这些能力并不是Path类的，而是通过Path类才能实现的，因为Path类只代表路径。

  Path类指明了路径，还需要一支笔，Paint类。笔是有多种多样的，就以需求来说定义一支笔出来

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

  ![效果图]()

  感觉太空了，加点鲜艳的颜色，给画板加个底色

  ```java
  canvas.drawColor(Color.GREEN);
  ```

  真是惊艳[滑稽]

  ![惊艳的效果图]()

  还差点什么，趋势图嘛，就应该动起来，效果才好，怎么动呢，可以有好多方式，可以每隔一段时间调用一次onDraw方法，要用计时器吗？其实有更好的选择，View类有 invalidate() 方法，当被调用时，什么触发onDraw方法，如果把invalidate()放在onDraw方法中，这就成了一个循环。那么，把path路径的绘制放到onDraw中，每次调用就绘制一段，再调用invalidate()方法，循环下去就成了动图，任务完成。

  回过头来，还要处理一个棘手的问题，上面的相互调用是个死循环，需要在onDraw方法中加入一些限制条件，以控制循环次数，可以选择控制path的Y轴，当Y轴大于自定义的View的最右边界时，停止循环，问题解决。

  为了多聊一个API，这里才用另一种方式

  ```java
  canvas.clipRect(left,top,right,bottom);
  ```

  canvas有好多clipxxx的方法，都是裁剪成xxx的形状，让画板的大小慢慢变大，使path能显示在画板上的区域越来越大，好像趋势图在动，在前进。这个方法要注意的一点是只会影响这个方法调用之后的内容，之前canvas有多大，上面画了什么，都不会受到clip的影响，通过这一点可以把画板的底色设置在调用clip之前，保持底色大小不受裁剪的影响。

  ```java
  @Override
  protected void onDraw(Canvas canvas) {
      super.onDraw(canvas);
      canvas.drawColor(Color.GREEN);

      canvas.clipRect(0,0,right,getBottom());
      canvas.drawPath(path,paint);

      if (right < getRight()){
          right++;
          invalidate();
      }
  }
  ```

  最终效果

  ![最终效果图]()

