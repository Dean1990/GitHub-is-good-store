### Linux 开机启动配置 rc.local 文件运行 python 脚本的 ImportError:No module named xxx

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

最近写了个商品价格定时查询的小程序，部署到树莓派上做开机启动。

在使用python命令运行时一切正常，当写到 `rc.local` 文件中时遇到这个问题。

首先要查看日志，通过在 `rc.local` 文件头将：

```shell
#!/bin/sh
```

修改为

```shell
#!/bin/sh -x
```

日志输出到 `var/log/boot.log` 文件中

通过查看 `boot.log` 文件发现报错 ImportError 

这个问题就有点诡异了，后来通过查询，得知原因所在，真得是很难想到

**“用户”**

因为 `rc.local` 是由 root 用户运行的，而我一直是在一个叫 pi 的用户下安装环境，部署程序，所以在开机启动时，`rc.local` 中执行时在 root 这个用户下查找 python 的环境，包引用，所以出现了 ImprtError:No module named xxx 。

#### 解决

将：

```python
/usr/bin/python3 /home/pi/xxxxxxx.py
```

修改为

```python
sudo -H -u pi /usr/bin/python3 /home/pi/xxxxxxx.py
```

使用指定的用户运行脚本



----

   2019/3/4.

   *Dean.King*

   **Beijing**