### 关于对Android App Bundle 一些疑问的总结

关于 App Bundle 在刚刚出来的时候，看过一点点介绍，所以对它的理解就是分包，所以这几天要学习的时候就有很多疑问，也走了弯路。

#### 动态交付(Dynamic delivery)与主apk怎么联系的

看到官方文档说，App Bundle 技术几乎不用开发者做什么多余的工作，就可以打包成 `.aab` 文件，在安装时，就会以不同语言，分辨率以及ABI的结合生成新的apk，我们先把这个过程叫 `split` ， 然后安装到手机上，那这里的 Dynamic Feature 是怎么运作的呢？

###### *说明一点：这里按当前要安装应用的手机生成设备需要的apk的工作是 google play 做的，开发者不用关心，但后边会提到开源工具 bundletool，模拟 google play 的工作*

接上文，其实，这里是两个概念，`split` 指的是工程中的 `res` ，`libs` 等文件夹下的内容提取apk ，而 Dynamic Feature 是通过 AS 的`File->New->New Module...->Dynamic Feature Module` 创建的，它是 `Module` ，所以它基本是个完整的工程，可以有 `src` ，`res` ，`assets` 文件目录等，自然，这些文件夹下边能放什么就不用我多说了（资产啊，功能块啊，都可以放）。并且，这个 `Module` 同样也支持 `split` 。

回到我们的问题，当开发者不做任何修改直接通过 AS 的 `Build->Build Bundles` 去打包时，会把工程，以及 `Module` 全部打包，在在 `split` 时，Dynamic Feature Module 与主 apk 没有任何联系 ，提取的也只有 主工程的 语言，分辨率，ABI。

###### *说明一点：这里的 `split` 过程中是否要 分裂某一项是可控的，在 app 的 build.gradle 中，通过 enableSplit 属性控制 ，一直上代码*

```groovy
android {
		...
    // When building Android App Bundles, the splits block is ignored.
    splits {...}
  
  	// Instead, use the bundle block to control which types of configuration APKs
    // you want your app bundle to support.
    bundle {
        language {
            // Specifies that the app bundle should not support
            // configuration APKs for language resources. These
            // resources are instead packaged with each base and
            // dynamic feature APK.
            enableSplit = false
        }
        density {
            // This property is set to true by default.
            enableSplit = true
        }
        abi {
            // This property is set to true by default.
            enableSplit = true
        }
    }
}
```

#### 怎样让动态交付(Dynamic delivery)与主apk产生联系

这里就要提到 `Play Core Library` 这个库

引用

```groovy
dependencies {
    // This dependency is downloaded from the Google’s Maven repository.
    // So, make sure you also include that repository in your project's build.gradle file.
    implementation 'com.google.android.play:core:1.4.1'
    ...
}
```

怎么用点这里 [Play Core Library](<https://developer.android.com/guide/app-bundle/playcore>)

这是需要开发者写代码的，这才体现出动态

*这里我就困惑好久，我一直想 google play 咋做到这么智能，怎么知道 New 出来的 Module 里边放了什么，所以我才有上边的困惑，'是怎么联系的？'  ，原来是我天真了*

#### Bundletool 测试我们的包

[bundletool 是开源的](<https://github.com/google/bundletool>)，google play 其实用的也是它



参考内容：

[Add support for Dynamic Delivery](<https://developer.android.com/studio/projects/dynamic-delivery>)

[Download modules with the Play Core Library](<https://developer.android.com/guide/app-bundle/playcore>)

[bundletool](<https://developer.android.com/studio/command-line/bundletool>)