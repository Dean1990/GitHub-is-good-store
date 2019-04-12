### 关于对Android App Bundle 一些疑问的总结

关于 App Bundle 在刚刚出来的时候，看过一点点介绍，所以对它的理解就是分包，所以这几天要用到，学习时就有很多疑问，也走了弯路。

#### 动态交付与主apk怎么联系的

看到官方文档说，App Bundle 技术几乎不用开发者做什么多余的工作，就可以打包成 `.aab` 文件，在安装时，就会以不同语言，分辨率以及 abi 的结合，生成新的 apk，我们先把这个过程叫 `split` ， 然后安装到手机上，那这里的 Dynamic Feature 是怎么运作的呢？

###### *说明一点：这里按当前要安装应用的手机生成设备需要的 apk 的工作是 google play 做的，开发者不用关心，但后边会提到开源工具 bundletool，了解一下 google play 的工作*

接上文，其实，这里是两个概念，`split` 指的是工程中的 `res` ，`libs` 等文件夹下的内容提取 apk ，而 Dynamic Feature 是通过 AS 的`File->New->New Module...->Dynamic Feature Module` 创建的，它是 `Module` ，所以它基本是个完整的工程，可以有 `src` ，`res` ，`assets` 文件目录等，自然，这些文件夹下边能放什么就不用多说了（资产啊，功能块啊）。并且，这个 `Module` 同样也支持 `split` 。

回到我们的问题，当开发者不做任何修改直接通过 AS 的 `Build->Build Bundles` 去打包时，会把工程，以及 `Module` 全部打包，在 `split` 时，Dynamic Feature Module 与主 apk 没有任何联系 ，提取的也只有 主工程的 语言，分辨率，abi 。

###### *说明一点：这里的 `split` 过程中是否要 分裂某一项是可控的，在 app 的 build.gradle 中，通过 enableSplit 属性控制 ，直接上代码*

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

#### 怎样让动态交付与主apk产生联系

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

这是需要开发者写代码，这才体现出动态

*这里我就困惑好久，我一直想 google play 咋做到这么智能，怎么知道开发者在 Module 里边放了什么，所以才有上边的疑问，'是怎么联系的？'  ，原来是我天真了*

#### Bundletool 测试我们的包

[bundletool 是开源的](<https://github.com/google/bundletool>)，google play 其实用的也是它

用AS 打包生成 `.aab` 文件后，就可以上传到 google play 上发布了，如果我们要测试这个包打的对不对，就要用 Bundletool 工具

Bundletool 下载下来的是一个 `.jar` 文件，java 运行的命令自然是下面这样，后面可接参数

```shell
java -jar bundletool-all-0.9.0.jar
```

首先，由 `.aab` 文件生成 `.apks` 文件

```shell
java -jar ~/Downloads/bundletool-all-0.9.0.jar build-apks --bundle=app.aab --output=app.apks --ks=test_key --ks-pass=pass:12345678 --ks-key-alias=key0 --key-pass=pass:12345678
```

###### *这里要强调输入密码需要加前缀 `pass:`* 

先得到设备信息，保存为 json 文件

```shell
java -jar ~/Downloads/bundletool-all-0.9.0.jar get-device-spec --output=tcl.json --adb=~/Dev/android-sdk/platform-tools/adb
```

然后通过在 `.apks` 文件中抽取设备需要的内容，保存在一个文件夹中，这个文件夹需要提前创建好

```shell
java -jar ~/Downloads/bundletool-all-0.9.0.jar extract-apks --apks=app.apks --output-dir=save_apks --device-spec=tcl.json
```

成功提示

```shell
The APKs have been extracted in the directory: save_apks
```

查看该文件夹，可以看到 `base-master.apk` 主apk，还有有关语言的apk，有关分辨率的apk，如果工程中有 abi 的话，也会有相关 apk ，当然，前提是前面提到的 `build.gradle` 中 enableSplit = true

这里没有看到任何与 Module 相关的apk ，也体现出 Dynamic Feature Module 与主apk没有联系，建立联系需要通过代码实现，动态的从 google play 上下载需要的内容。

Bundletool 还有个安装命令，想必是结合了前面提到的 获取设备信息，提取需要的内容，再安装的一条龙服务吧，这个并没有深入研究，毕竟这些事原本也不是开发者要干的，把 `.aab` 包交给 google play 就好了

```shell
java -jar bundletool-all-0.9.0.jar install-apks --apks=app.apks
```



split 的过程

![split](https://img-blog.csdn.net/20181018223835228?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x0eW0yMDE0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

参考内容：

[Add support for Dynamic Delivery](<https://developer.android.com/studio/projects/dynamic-delivery>)

[Download modules with the Play Core Library](<https://developer.android.com/guide/app-bundle/playcore>)

[bundletool](<https://developer.android.com/studio/command-line/bundletool>)



