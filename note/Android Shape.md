### Android Shape

------

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### 规定形状

Shape可以自定义四类图形，rectangle（矩形），oval（椭圆形），line（线），ring（圆环）;

由<shape>标签的 android:shape 属性控制图形，默认是rectangle（矩形） 

```xml
<shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:dither=["true" | "false"]
    android:tint="color"
    android:tintMode=["screen" | "add" | "multiply" | "src_atop" | "src_in" | "src_over"]
    android:visible=["true" | "false"]
	android:shape=["rectangle" | "oval" | "line" | "ring"]>
</shape>
```

| 属性               | 说明                                       |
| ---------------- | ---------------------------------------- |
| android:dither   | 当位图的像素配置与屏幕不同时（例如：ARGB 8888 位图和 RGB 565 屏幕）启用位图的抖动，值为“false”时则停用抖动，默认值为 true。 |
| android:tint     | 着色                                       |
| android:tintMode | 着色模式                                     |
| android:visible  | 未知                                       |
| android:shape    | 设置形状                                     |

##### android:shape="line"

- 只能画水平线；
- 线的高度是通过stroke的android:width属性设置的；
- 如果是虚线，引用虚线的view必须添加属性android:layerType，值设为"software"，否则显示不了虚线；
- <stroke>标签的 android:width 必须小于 <size>标签的 android:width，<size>标签的 android:height 必须大于 <stroke>标签的 android:width，否则无法显示。

##### android:shape="ring"

```xml
<shape
    xmlns:android="http://schemas.android.com/apk/res/android"
	android:shape="ring"
    android:thickness="dimen"
    android:thicknessRatio="integer"
    android:innerRadius="dimen"
    android:innerRadiusRatio="integer"
    android:useLevel=["true" | "false"]>
</shape>
```

注：本文在代码块中出现的"dimen"，"integer"和"color"均表示Android资源文件<resources>标签的子标签<dimen>，<integer>和<color>所能定义的内容，重要说明<integer>，不只代表整型，还可以定义浮点型。

| 属性                       | 说明                                       |
| ------------------------ | ---------------------------------------- |
| android:thickness        | 环的厚度，优先级大于android:thicknessRatio         |
| android:thicknessRatio   | 环的厚度相对于环的宽度的比例，默认为9， 厚度是宽度的1/9           |
| android:innerRadius      | 内环半径，优先级大于android:innerRadiusRatio       |
| android:innerRadiusRatio | 内环半径相对于环的宽度的比例， 默认为3， 内环半径为环的宽度的1/3      |
| android:useLevel         | 如果当做是LevelListDrawable使用时值为true，否则为false，该值为true时会影响shape单独使用时的显示效果。 |

#### 修饰

<shape>标签有6个子标签，分别为 <size>（尺寸），<solid>（内部），<stroke>（边缘），<corners>（角），<gradient>（渐变），<padding>（内边距），用来修饰图形。

##### <size>

只有控件宽高设置成wrap_content时，该宽高才起作用，只起到最小宽高值的作用

```xml
<size
      android:width="dimen"
      android:height="dimen" />
```
| 属性             | 说明    |
| -------------- | ----- |
| android:width  | 图形的宽度 |
| android:height | 图形的高度 |

##### <solid>

当设置<gradient>标签时，该标签失效，当android:shape="line"时，该标签也失效

```xml
<solid
      android:color="color" />
```
| 属性            | 说明      |
| ------------- | ------- |
| android:color | 内部填充的颜色 |

##### <stroke> 

不设置该标签表示图形没有边线

```xml
<stroke
      android:width="dimen"
      android:color="color"
      android:dashWidth="dimen"
      android:dashGap="dimen" />
```
| 属性                | 说明                                  |
| ----------------- | ----------------------------------- |
| android:width     | 边线的宽度                               |
| android:color     | 边线的颜色                               |
| android:dashWidth | 虚线每段的长度，只有设置了android:dashGap才能显示出效果 |
| android:dashGap   | 虚线空隙的宽度                             |

##### <corners>

只有 android:shape="rectangle" 时有效

```xml
<corners
      android:radius="dimen"
      android:topLeftRadius="dimen"
      android:topRightRadius="dimen"
      android:bottomLeftRadius="dimen"
      android:bottomRightRadius="dimen" />
```
| 属性                        | 说明               |
| ------------------------- | ---------------- |
| android:radius            | 圆角大小，优先级小于下面四个属性 |
| android:topLeftRadius     | 左上角大小            |
| android:topRightRadius    | 右上角大小            |
| android:bottomLeftRadius  | 左下角大小            |
| android:bottomRightRadius | 右下角大小            |

##### <gradient>

```xml
<gradient
      android:angle="integer"
      android:centerX="integer"
      android:centerY="integer"
      android:startColor="color"
      android:centerColor="color"
      android:endColor="color"
      android:gradientRadius="dimen"
      android:type=["linear" | "radial" | "sweep"]
      android:useLevel=["true" | "false"] />
```
| 属性                     | 说明                                       |
| ---------------------- | ---------------------------------------- |
| android:angle          | 渐变角度， 取值[0-360]，0为从左到右，逆时针旋转             |
| android:centerX        | 渐变中心的X轴位置，百分值，取值[0-1.0f]，当android:type="linear"时，需要设置android:centerColor属性才能显示出效果 |
| android:centerY        | 渐变中心的Y轴位置，百分值，取值[0-1.0f]，当android:type="linear"时，需要设置android:centerColor属性才能显示出效果 |
| android:startColor     | 渐变起始颜色                                   |
| android:centerColor    | 渐变中间颜色                                   |
| android:endColor       | 渐变结束颜色                                   |
| android:gradientRadius | 渐变半径，只有当android:type="radial"时有效         |
| android:type           | 渐变类型，可选类型 linear（线性，默认值）， radial（ 放射，必须设置android:gradientRadius属性才能显示效果）， sweep（ 扫描） |
| android:useLevel       | 如果当做是LevelListDrawable使用时值为true，否则为false，该值为true时会影响shape单独使用时的显示效果。 |

##### <padding>

```xml
<padding
      android:left="dimen"
      android:top="dimen"
      android:right="dimen"
      android:bottom="dimen" />
```
| 属性             | 说明   |
| -------------- | ---- |
| android:left   | 左内边距 |
| android:top    | 上内边距 |
| android:right  | 右内边距 |
| android:bottom | 下内边距 |

大概就是这些。

------

2018/1/31.

*Dean.King*

**Beijing**