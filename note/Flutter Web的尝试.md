### Flutter Web的尝试

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### 版本

由于众所周知的原因，更新 Flutter 的 SDK 变的异常艰难，最后找到了 flutter 中文站，下载最新版本替换旧版，完成更新。去创建项目，发现 VSCode 中没有网上说的 “Flutter: New Web Project” 选项，于是先创建了一个 “Flutter: New Project” ，果然，项目中没有 “web” 目录，由于就各种查资料，有人说，需要切换版本，需要切换到 `beta` 版本中，原来 Flutter web 一直都没有正式发布，不过新闻在去年都没有提到 Flutter web ，现在还没有正式版也是奇怪。

切换版本

```shell
flutter channel beta
```

我的天！又是异常艰难。

后来实在忍不了了，我按下了 `ctrl + c` ，我为什么要中断呢，因为我突然想到，去看看 flutter 中文站有没有 `beta` 版本可以下载，果然有。老方法，再替换一次。

[https://flutter.cn/docs/development/tools/sdk/releases](https://flutter.cn/docs/development/tools/sdk/releases)

这时候我们就有了 Flutter 的  `beta` 版本

#### 步入正轨

下一步就是开启对 web 的支持 

```shell
flutter config --enable-web
```

然后，去打开 VSCode的 ，满心期待的去创建项目

快捷键 `command + shift + p` 调出输入框，输入 “Flutter” ，选项中还是没有 “Flutter: New Web Project”，由于，我又“礼貌”的创建了一个 “Flutter: New Project” 项目

噢！居然有 `web` 目录！

然后就开始干活吧，运行命令是这样的：

```shell
flutter run -d chrome
```

温馨提示，上面这条命令需要在项目根目录下输入。

还有就是电脑里得安装了 chrome浏览器的话，-d 后面的参数才用 `chrome`

可以先查一下电脑中可用设备

```shell
flutter devices
```

根据实际情况填写参数。

#### 总结

当前阶段（2020-12-4）Flutter的 web 功能还没有发布正式版本，所以使用 web 功能需要使用 `beta` 版本

- Flutter beta 版本下载地址，进入页面找 `Beta channel` 列表

  [https://flutter.cn/docs/development/tools/sdk/releases](https://flutter.cn/docs/development/tools/sdk/releases)

- 最好给 Flutter SDK 配置到系统的环境变量中，配置方法（略）

- 开启 Flutter 对 web 的支持

  ```shell
  flutter config --enable-web
  ```

- 创建项目，使用命令或者IDE都可，不必纠结在创建过程中没有没 “web” 字样，如果上面几步正确，创建的项目中就会有 web 目录

- 如果项目中有 web 目录，就可以在项目根目录下直接执行命令，查看效果

  ```shell
  flutter run -d chrome
  ```

  完



----

   2020/12/4.

   *Dean.King*

   **Beijing**

