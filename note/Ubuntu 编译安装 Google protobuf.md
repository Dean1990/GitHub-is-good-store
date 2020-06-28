### Ubuntu 编译安装 Google protobuf

------

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### 安装依赖包

``` shell
$ sudo apt-get install autoconf automake libtool curl make g++ unzip
```

#### 下载 protobuf 源代码

[GitHub项目地址](https://github.com/protocolbuffers/protobuf)

``` shell
$ git clone https://github.com/protocolbuffers/protobuf.git
```

#### 编译 protobuf 

切到 protobuf 目录下，然后执行编译命令

``` shell
  $ cd protobuf
  $ ./autogen.sh
  $ ./configure
  $ make
  $ make check
  $ sudo make install
  $ sudo ldconfig # 刷新
```

测试是否安装成功

``` shell
$ protoc --version
```

然后安装对应语言的版本，例如 python 版

这里说明一下，我之前就卡在这里了，直接找到 python 目录下的 readme 文件 ，但只提供了以下命令：

``` shell
 $ cd  python
 $ python setup.py build
 $ python setup.py test
 $ python setup.py install
```

运行 build 的时候就出错了，后来才知道，这是要分两步的，先要编译 protobuf ，然后在安装需要的语言的版本，如果编译 protobuf 后再运行上面的命令，应该不会有问题，最后测试一下是否安装成功

``` shell
$ python
>>> import google.protobuf
```

> 注意：有些命令会报没有权限的错误，使用 `sudo` 时注意 python 的指向，python2 or python3?

---

2020/6/27.

*Dean.King*

**Beijing**

