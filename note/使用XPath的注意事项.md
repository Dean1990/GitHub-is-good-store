###XPath

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### 默认情况下，根节点不做为查找对象

```html
<h2><span id="方块"></span><span class="mw-headline" id=".E6.96.B9.E5.9D.97">方块</span></h2>
```

如果想查找 `h2` 下所有的 `span` ，使用 "h2/span" 是拿不到数据的，应该写成 "span" （这不是唯一答案，以下同）。

如果我们就是需要查找 `h2` ，比如，想判断当前根节点是不是 `h2` ，可以写成 "self::h2"。

所以，在上面提到的提取 `span` 还可以这样写 "self::h2/span"

#### `//` 与 `.//` 要区分开

```html
<body>
  <h2><span id="方块"></span><span class="mw-headline" id=".E6.96.B9.E5.9D.97">方块</span></h2>
	<h3><span id="方块"></span><span class="mw-headline" id=".E6.96.B9.E5.9D.97">方块</span></h3>
</body>
```

假如当前查找到的节点是 `h2` ，命称为 aa 

- `aa.xpath("//span") ` 查找的是**整个文档**中所有的 `span` ，以上文档为例，便会找出4个 `span`
- `aa.xpath(".//span")` 查找的是**当前 aa** 下所有的`span` ，以上文档为例，会找到2个 `span`

#### 查找时列表下标是从1开始的，并非0

```html
<h2><span id="方块"></span><span class="mw-headline" id=".E6.96.B9.E5.9D.97">方块</span></h2>
```

查找第一个 `span` 标签，写成 "span[1]"


----

2020/12/13.

*Dean.King*

**Beijing**