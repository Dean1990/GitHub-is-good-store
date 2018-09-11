#### 入坑Tensorflow

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

*主要参考文章：*

*https://blog.csdn.net/qq_36810544/article/details/79149424*

*https://blog.csdn.net/chenmaolin88/article/details/79357263*

*https://blog.csdn.net/Leon_yy/article/details/81053282*

##### 安装

可喜可贺的是[TensorFlow的官网](https://tensorflow.google.cn/)，是可以正常访问的。

安装tensorflow，直接参考官方教程，[链接](https://tensorflow.google.cn/install/)。



还需要安装labelImg做图片标记

安装labelImg

```shell
pip install labelImg
```

运行labelImg

```shell
labelImg
```

运行时可能报错

提示`No module named PyQt4` 

安装PyQt4

```shell
conda install pyqt=4
```

提示`No module named lxml` 

安装lxml

```shell
pip install lxml
```

以此类推，如果遇到相似错误提示 `No module named xxx` ,就使用`pip install xxx` ，如果不对，可以百度“xxx的安装命令”，比如PyQt4就是这一类。对于这一类，我打算持续更新一个列表，[链接](http://deanlib.com/index.php/archives/22/)。



https://github.com/tensorflow/models/issues/2025



http://www.cnblogs.com/coolqiyu/p/9440475.html