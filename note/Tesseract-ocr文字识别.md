### Tesseract-ocr文字识别

------

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### 安装

[tesseract](https://github.com/tesseract-ocr/tesseract)

[jTessBoxEditor](https://sourceforge.net/projects/vietocr/files/jTessBoxEditor/)

#### 准备

需要收集大量文本图片，然后转换成tiff文件

使用工具 jTessBoxEditor 合并图片，打开 jTessBoxEditor.jar ，`主菜单-Tools-Merge TIFF` ，比如命名为`custom.tif`

生成box文件

```shell
$ tesseract custom.tif custom batch.nochop makebox
```

可以添加参数` -l` 

```shell
$ tesseract custom.tif custom -l eng batch.nochop makebox
```

这个eng的字库文件是tesseract自带的字库，也可以从[这里](https://github.com/tesseract-ocr/tessdata)下载需要的字库，其中的`chi` 开头的是中文字库，`sim` 是简体，`tra` 是繁体，还有带 `vert` ，字面意思是绿色的，相比之下文件大小差很多，但用上面的命令调用会报错，暂时不清楚这个文件是做什么用的，忽略。

下载的字库需要放到tesseract的安装目录，比如Mac的默认安装目录在`/usr/local/Cellar/tesseract/3.05.02/share/tessdata/` ，从这里是可以看到自带的 `eng.traineddata`字库文件的。

上面命令生成了名为`custom.box` 的文件，它存放了识别出来的每个字和它对应的位置信息。

#### 校准

使用工具 jTessBoxEditor 进行校准，切换到 `Box Editor` 标签，`Open` 选择`custom.tif` 文件，对每个文字进行校准，jTessBoxEditor 的操作不懂自行百度。

#### 训练

```shell
$ tesseract  custom.tif custom  nobatch box.train
```

会生成文件 `cuostom.tr` 

```shell
$ unicharset_extractor custom.box
```

会生成文件 `unicharset` 

新建 font_properties 文件，文件格式如下

```
<fontname> <italic> <bold> <fixed> <serif> <fraktur>
```

比如我们的字体名叫`custom` ，后面描述样式 `1` 代表 是，`0` 代表 否，比如是粗体，没有其他样式

```
custom 0 1 0 0 0
```

继续训练和聚类

```shell
$ shapeclustering -F font_properties -U unicharset custom.tr
$ mftraining -F font_properties -U unicharset -O unicharset custom.tr
$ cntraining custom.tr
```

最后总共生成了5个文件，`unicharset` ，`inttemp` ，`pffmtable` ，`shapetable` ，`normproto` 。

修改5个文件的名称，加上前缀`custom.` ，`custom.unicharset` ，`custom.inttemp` ，`custom.pffmtable` ，`custom.shapetable` ，`custom.normproto` 。

合并

```shell
$ combine_tessdata custom.
```

会生成文件 `custom.traineddata`

#### 测试

同样的，我们自己生成的字库，也要先放到 tesseract的安装目录，才能使用。

测试

```shell
$ tesseract test.jpg result -l custom
```

对图片 `test.jpg` 进行识别，使用我们自己训练出来的字库 `custom` ，结果存放在`result` 的文件中

#### 总结

最后我训练出的`custom` 并不理想，还没用官方提供的 `chi` 字库识别率高，话又说回来，毕竟是官方这么多年来训练出的字库，不能同日而语，还有一点，`font_properties` 文件的配置也比较重要。

------

2018/9/22.

*Dean.King*

**Beijing**