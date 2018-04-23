### [Android] requestLayout() , layout(),invalidate() , postInvalidate() 之间的区别

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### requestLayout()

```java
View.requestLayout();
```

调用该方法，控件会重新执行 `onMeasure()`，`onLayout()` 和 `onDraw()` 这三个方法。

#### layout()

```java
View.layout(int l, int t, int r, int b)
```

调用该方法会执行 `onLayout()` 方法，但 Android 5.0之后的代码有所不同，因为引入了 `NestedScrolling`，所以该调用也可能会执行 `onMeasure()` 。

```java
if ((mPrivateFlags3 & PFLAG3_MEASURE_NEEDED_BEFORE_LAYOUT) != 0) {
    onMeasure(mOldWidthMeasureSpec, mOldHeightMeasureSpec);
    mPrivateFlags3 &= ~PFLAG3_MEASURE_NEEDED_BEFORE_LAYOUT;
}
```

#### invalidate()

```java
View.invalidate()
```

调用该方法，控件会重新执行 `onDraw()` 。

#### postInvalidate()

```java
View.postInvalidate();
```

调用该方法，与 `invalidate()` 方法效果一样，区别在于 `invalidate()` 方法是在UI线程中调用，`postInvalidate()` 是在非UI线程中调用。

