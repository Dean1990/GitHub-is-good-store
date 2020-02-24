#### [Android] Error:The SourceSet 'instrumentTest' is not recognized by the Android Gradle Plugin. Perhaps you misspelled something?

报错文件 build.gradle

```groovy
sourceSets {
    main {
        manifest.srcFile 'AndroidManifest.xml'
        java.srcDirs = ['src']
        resources.srcDirs = ['src']
        aidl.srcDirs = ['src']
        renderscript.srcDirs = ['src']
        res.srcDirs = ['res']
        assets.srcDirs = ['assets']
        jniLibs.srcDirs = ['libs']
    }
    // Move the tests to tests/java, tests/res, etc...
    instrumentTest.setRoot('tests')
    // Move the build types to build-types/<type>
    // For instance, build-types/debug/java, build-types/debug/AndroidManifest.xml, ...
    // This moves them out of them default location under src/<type>/... which would
    // conflict with src/ being used by the main source set.
    // Adding new build types or product flavors should be accompanied
    // by a similar customization.
    debug.setRoot('build-types/debug')
    release.setRoot('build-types/release')
}
```

由于Gradle升级，`instrumentTest`已被弃用，**将`instrumentTest.setRoot('tests')`修改为`androidTest.setRoot('tests')`** 。[stackoverflow](https://stackoverflow.com/questions/49511200/the-sourceset-instrumenttest-is-not-recognized-by-the-android-gradle-plugin)

----

#### [Android] Caused by: org.gradle.process.internal.ExecException: A problem occurred starting process 'command 'xxx/android-sdk/ndk-bundle/toolchains/mips64el-linux-android-4.9/prebuilt/darwin-x86_64/bin/mips64el-linux-android-strip''

升级NDK后，老项目在运行或者打包时就会报此错误，开始的时候从网上找到一种解决办法，修改`local.properties`文件中的 ndk.dir

```properties
ndk.dir=/Users/dean/Dev/android-sdk/ndk-bundle
#修改成
ndk.dir=/Users/dean/Dev/android-sdk/ndk-10
```

就是把 bundle 换成一个数字，确实是可行的，但`local.properties`这个文件是自动生成的，每次重新打开工程时都要修改，很是麻烦。

看报错内容，是说`mips64el-linux-android-strip`这个东西执行不了，于是，顺藤摸瓜根据那个路径找找这个东西，当找到`darwin-x86_64`这一层文件夹时，就没有了，文件夹中只有一个文本`NOTICE-MIPS64`，打开来看

```
This mips64el-linux-android-4.9 directory exists to make the NDK compatible with the Android
SDK's Gradle plugin, version 3.0.1 and earlier, which expects the NDK
to have a MIPS64 toolchain directory.
```

这就都明白了，为什么新项目的`local.properties` ndk.dir与老的一样，而只有老项目才会报错。

两个解决方法

- 升级Gradle plugin，修改build.gradle(Project)

  ```groovy
  buildscript {
      repositories {
          jcenter()
      }
      dependencies {
          classpath 'com.android.tools.build:gradle:2.3.3'
  		//修改到高于或等于3.0.1的版本
      }
  }
  ```

- **修改NDK的本地路径 菜单栏 File - Project Structure - SDK Location - Android NDK location，修改成一个低版本的NDK，**我的NDK是在升级到17.0.4640043 rc1后出现的这个问题。默认情况下，NDK使用的是android-sdk下的ndk-bundle这个文件下的NDK，但正是这个版本被升级了，所以需要从网上去找一个低版本的NDK，因为我之前搞Cocos需要用NDK-10，直接指定过来一试，问题解决。

**Tip** 不建议使用第一种方式，想起同事经常说的一句话

> 你见过代码有保质期吗？Android studio的代码就有[哭笑]

其实说的就是 Gradle ，当你打开一个老项目，AS提示你，Gradle Update的时候，你一定要有充足的勇气和充足的时间，说多了全是眼泪...

---

#### [Android] java.lang.NoClassDefFoundError

这个问题是一个大类，我只说我遇到的情况

首先，遇到这个问题，可以先`Clean Project`，如果没用，请继续往下看 

这里有一份详细的[分析文档](https://blog.csdn.net/jamesjxin/article/details/46606307)，但并不适应于我遇到这种情况，因为这是在Android上才能遇到的情况

问题就出在`分包`上，什么是分包？

当方法数超过65535时，就会需要`分包`，代码打包成多个DEX文件。

为什么是65535？

> Dalvik Executable 规范将可在单个 DEX 文件内可引用的方法总数限制在 65,536，其中包括 Android 框架方法、库方法以及您自己代码中的方法。在计算机科学领域内，术语[*千（简称 K）*](https://en.wikipedia.org/wiki/Kilo-)表示 1024（或 2^10）。由于 65,536 等于 64 X 1024，因此这一限制也称为“64K 引用限制”。

代码量不够还真遇不到这个错误[苦笑]

首先会报这样一个错误

```
com.android.build.api.transform.TransformException: com.android.ide.common.process.ProcessException: java.util.concurrent.ExecutionException: com.android.ide.common.process.ProcessException: Return code 2 for dex process
```

提示信息

> The number of method references in a .dex file cannot exceed 64K.
> Learn how to resolve this issue at https://developer.android.com/tools/building/multidex.html	

**配置 multiDexEnabled 开启分包**

```groovy
defaultConfig {
    applicationId "com.deanlib.application"
    minSdkVersion 15
    targetSdkVersion 25
    multiDexEnabled = true//开启分包
    versionCode 16
    versionName "1.0_beta16"
    testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
}
```

开启分包后，运行工程就有可能遇这个` java.lang.NoClassDefFoundError`问题，当然也有可能遇不到，这个很好理解，当第一个DEX包中运行的代码调用到其他包中的类或者方法时，才会报这个错误，而且还有一点，就是在Andrid 5.0以下才能遇到这个问题，因为Android 5.0以上使用了ART机制	，支持从 APK 文件加载多个 DEX 文件运行。

解决方法：**使用 MultiDexApplication 替换 Application**

> Android 构建工具会根据需要构建主 DEX 文件 (`classes.dex`) 和辅助 DEX 文件（`classes2.dex` 和 `classes3.dex` 等）。然后，构建系统会将所有 DEX 文件打包到您的 APK 中。

> 运行时，Dalvik 可执行文件分包 API 使用特殊的类加载器来搜索适用于您的方法的所有 DEX 文件（而不是仅在主 classes.dex 文件中搜索）。

[配置方法数超过 64K 的应用](https://developer.android.com/studio/build/multidex.html)

---

#### [Android]关于WebView图片不显示的一种可能

做WebView图片列表展示时，遇到偶尔有图片不能显示的问题，经过多种测试，图片压缩，存放在本地，切换网络等种种方法，都没有找到问题的原因。拿到img标签中的图片链接，使用浏览器是可以正常显示的。一筹莫展之际，想到了抓包，得到的结果是403，403就是没有权限，为什么没有权限呢？

没有权限，很容易让我想到了图片链接中后缀的两个参数，大概是这样：`https://cdn.xxx.com/abcxxxx.jpg?sign=XKD0DFL45KDfdkSKD%2bdksld&xxx=xxxx`，如果去掉后边的参数，放到浏览器中也是会报403的错误，问题就出在这两个参数。于是，把图片列表中的每个链接都拿出来，进行比较，图片有的可以显示，有的不能显示，发现不能显示图片的链接中都带有`%2b`，这是个转意字符，其实就是`+`加号，通过查询，资料很少，stackoverflow上有人提到了[这个问题](https://stackoverflow.com/questions/43623260/react-native-webview-image-with-2b-char)。

解决方案：抱歉，我还没有找到好的解决方法。

有人在服务器端通过在生成sign时，做检查，如果有`%2b`就重新生成一个，这并不能算一种解决方案，我认为。

后来的图片列表展示功能，因为使用WebView还有其他问题，改用了ListView，这也不能算解决方案～

----

https://blog.csdn.net/u013009899/article/details/79953933

删缓存