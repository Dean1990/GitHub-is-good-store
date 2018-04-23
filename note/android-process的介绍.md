### android:process的介绍

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### 使用范围

使用在Manifest文件中的`<application>` ，`<activity>` ，`<service>` ，`<receiver>` 和 `<provider>` 标签中，默认情况（没有显示定义）进程名就是包名，如果在 `<application>` 标签中定义将影响全局，但在四大组件中定义的进程名优先于 `<application>` 。

#### 命名规则

- 以 `:` 开头的进程属于当前应用的私有进程

  ```xml
  <activity android:name=".SecondActivity"
            android:process=":aaa"
            />
  ```

  完整进程名 `包名:aaa`，但不允许手动补全包名 `android:process="com.deanlib:aaa"` 这样写是错的。必须以 `:` 开头，后面内容可以由字母，点，下划线和数字组成，不能以下划线或者数字开头，并且在点`.`后面不能接下划线或数字。

- 不以 `:` 开头的进程属于全局进程

  ```xml
  <activity android:name=".ThreeActivity"
            android:process="com.deanlib.abc"
            />
  ```

  规则与`:`后面内容的规则一样，再加一条，至少包含一个点`.` 。

上面提到私有和全局，怎么体现出来呢？

私有进程，其他应用的组件不可以和它运行在同一个进程中，这也很好理解，由Android的包名不可重复和上面的命名规则就可以看出，我们定义不出来在同一个设备上运行的两个应用而且进程名一样的情况；

全局进程是允许数据共享的，但需要满足两点要求，相同的ShareUID和相同的包签名。



----

   2018/4/10.

   *Dean.King*

   **Beijing**

