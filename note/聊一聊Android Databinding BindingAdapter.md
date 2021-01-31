### 聊一聊Android Databinding BindingAdapter

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

怎么用，自不必多说，教程蛮多的，聊聊他的坑。

#### 命名

如果对 `Java Web 的 Spring 框架` 有了解，自然会很情切这种写法

```kotlin
@BindingAdapter(value = ["imageUrl", "placeHolder", "error", "useCache", "isCircle"], requireAll = false)
@JvmStatic
fun loadImage(
    imageView: ImageView,
    url: String?,
    holderDrawable: Drawable?,
    errorDrawable: Drawable?,
    cache: Boolean = true,
    circle: Boolean = false
) {}
```

这里要说的就是 **方法命名无特殊要求**，就是很随意，`fun a()` `fun b()` 都可以，但是形参个数必须比 @BindingAdapter的 `value`  中的参数多一个，而且多的这个必须在第一个（后边再说这个），其他形参与 注解中参数**位置要一一对应，但名称不要求必须相同**，例如上面代码中的 "imageUrl" 与 "url" 位置对应，但名称不同。

#### 匹配

框架是通过扫描整个工程，找到有 @BindingAdapter 注解的方法，通过参数，匹配对应的代码，再进行相应的操作，这个过程和 `Spring框架` 很像。其中在注解中可以通过 `requireAll` 来控制是否要完全匹配。`requireAll = true` 的话，`value` 中的每个参数都要赋值，才能匹配成功，`requireAll = true` 为默认值。

举例：

```xml
<ImageView
	android:layout_width="53dp"
	android:layout_height="53dp"
	imageUrl="@{Constant.serviceUrlForXML + config.user.avatar}"
	placeHolder="@{@drawable/toux}"
	error="@{@drawable/toux}"
	isCircle="@{true}"/>
```

如果在 `requireAll = true` 的情况下，上面提到的 `loadImage()` 方法将不能与这个 `ImageView` 匹配，因为这个 `ImageView` 没有 `useCache` 。

*这里要注意， 在没有设置 `requireAll = false` 时，一定要注意 布局文件（xml）设置这些属性时的正确性，由于@{} 中是可以写代码的，一定要注意代码运行的返回值，例如`imageUrl` 参数接收 `String` 类型，而@{} 中代码返回结果不是 `String` 类型，这个时间又设置的完全匹配，可能就会报错，"不能找到对应的set方法，巴拉巴拉..."，都是从 databinding自动生成的文件中报出来的，让你看的云里雾里~*

#### 值

刚开始接触 @BindingAdapter 的时候，感觉这个东西挺好用，在自定义 View 的时候，自定义一些属性，不用去写声明文件，也不用在 View 初始化的时候去 获取每一个属性。直接在自定义View 中写一个 `public` 的setXXX方法，在注解的方法中第一个形参就是这个 View ，都实例化好了，拿来直接用，调用setXXX方法，真是方便。 

但用着用着发现并不是这样，使用 @BindingAdapter 注解出来的属性只能使用 @{} 方式赋值，例如上面 布局代码中的 `isCircle` 赋值写成了 `@{true}`，如果下面这样写：

```xml
isCircle="true"
```

这样写是无效的，在 `loadImage()` 方法中，形参 `circle` 是拿不到值的。

看这写法，使用双引号引起，字符串能不能拿到值呢？答案是拿不到，即使形参类型是 `String` 。

所以这里与上面的 *注意* 中提到的问题一样，需要注意赋值的正确性。

#### 第一个参数

第一个参数是 View 本身，支持子类型，而且他起到了编译时的验证作用。例如上面 `loadImage()` 方法第一个形参是 `ImageView` ，那么，在布局文件（xml）中所有的 <ImageView> 标签，以及继承 `ImageView` 子View的标签都可以使用这个方法注解@BindingAdapter中的属性（"imageUrl", "placeHolder", "error", "useCache", "isCircle"），如：

```xml
<androidx.appcompat.widget.AppCompatImageView
	android:scaleType="fitXY"
	useCache="@{false}"
	imageUrl="@{Constant.serviceUrlForXML + data.snapshot}"
	android:layout_width="match_parent"
	android:layout_height="match_parent"/>
```

但如果给 其他标签 使用这些属性（假如 其他标签 没有定义与以上相同参数名的@BindingAdapter），在编译时就会报错。

#### 注解样例

给出一个ImageView 结合 Glide 加载图片的例子

```kotlin
@BindingAdapter(value = ["imageUrl", "placeHolder", "error", "useCache", "isCircle","url"], requireAll = false)
    @JvmStatic
    fun loadImage(
        imageView: ImageView,
        url: String?,
        holderDrawable: Drawable?,
        errorDrawable: Drawable?,
        cache: Boolean = true,
        circle: Boolean = false,
    ) {
        val options = RequestOptions()
        if (!cache) {
            options.skipMemoryCache(false).diskCacheStrategy(DiskCacheStrategy.NONE)
        }
        if (circle) {
            options.transform(CircleCrop())
        }
        if (holderDrawable == null) {
            options.placeholder(R.drawable.default_img)
        } else {
            options.placeholder(holderDrawable)
        }
        if (errorDrawable == null) {
            options.error(R.drawable.default_img)
        } else {
            options.error(errorDrawable)
        }
        Glide.with(imageView.context)
            .load(url).apply(options).into(imageView)
    }
```

在布局文件中的使用，查看前面内容。


----

2020/12/17.

*Dean.King*

**Beijing**

