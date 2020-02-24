### [Python SQLObject] UnicodeEncodeError: 'charmap' codec can't encode characters in position 96-99: character maps to <undefined>

使用 SQLObject ，插入中文内容报错

```shell 
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/sqlobject/mysql/mysqlconnection.py", line 248, in _executeRetry
    return cursor.execute(query)
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/MySQLdb/cursors.py", line 191, in execute
    query = query.encode(db.encoding)
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/encodings/cp1252.py", line 12, in encode
    return codecs.charmap_encode(input,errors,encoding_table)
UnicodeEncodeError: 'charmap' codec can't encode characters in position 96-99: character maps to <undefined>
```

我使用Python3 开发，所以开始的时候，尝试使用 str 与 bytes 的 `decode('utf-8')` 与 `encode('utf-8')` 做编码转换，没有任何效果。

``` python
import sys

print(sys.stdout.encoding)
print(sys.getdefaultencoding())
print(sys.getfilesystemencoding())
```

查看系统编码都是 `utf-8` 

无果后，把问题转移到错误日志上，发现编码调用的是 `encodings/cp1252.py` ，难道要 “encode cp1252 ” ？，试了一下还是不行，继续看日志，就扯到 `MySQLdb` 了，`query = query.encode(db.encoding)` ，看到这个，明白了，原来是数据库要编码 sql 语句。

追了一个 `db.encoding` ，发现这个变量是从 SQLObject 的创建数据库连接的方法传进来的

``` python
class MySQLConnection(DBAPI):
  ...
	def __init__(self, db, user, password='', host='localhost', port=0, **kw):
    ...
    if "charset" in kw:
    	self.dbEncoding = self.kw["charset"] = kw.pop("charset")
    else:
    	self.dbEncoding = None
```

设置一下试试

``` python
from sqlobject.mysql import builder

conn = builder()(user='root', password='123456', host='localhost', db='db1', port=3306, charset='utf-8')
# 这是个错误版本，请不要复制
```

又报错了！

``` shell
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/sqlobject/mysql/mysqlconnection.py", line 187, in makeConnection
    raise dberrors.OperationalError(ErrorMessage(e, conninfo))
sqlobject.dberrors.OperationalError: Can't initialize character set utf-8 (path: /usr/local/mysql/share/charsets/); used connection string: host=localhost, port=3306, db=db1, user=root
```

他不认识 `utf-8` ，为什么呢，要大写？最后发现正确的是 `utf8` 

``` python
from sqlobject.mysql import builder

conn = builder()(user='root', password='123456', host='localhost', db='db1', port=3306, charset='utf8')
```

测试代码，插入数据，一切OK。

