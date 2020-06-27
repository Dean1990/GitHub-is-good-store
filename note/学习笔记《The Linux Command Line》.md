学习笔记《The Linux Command Line》

- 如果提示符的最后一个字符是“#”，而不是“$”，那么这个终端会话就是超级用户权限

- 后台有6个**虚拟终端**在运行，可通过按下 `Ctrl+Alt+F1` 到 `Ctrl+Alt+F6` 访问，要切换虚拟终端要按下 `Alt+F1` 到 `Alt+F6` ，返回图形桌面需要按 `Alt+F7` 
- 类 Unix 操作系统，比如 Linux，总是**只有一个单一的文件系统树**，不管有多少个磁盘或者存储设备连接到计算机上
- 每个用户都有他自己的家目录，当用户以普通用户身份操控系统时，家目录是**唯一有写权限的目录**
- 符号 `/` 表示根目录，符号 `~` 表示家目录，符号 `.` 表示当前工作目录，符号 `..` 表示当前工作目录的父目录，在输入相对路径时，`./` 是可以省略的
- 以 `.` 字符开头的文件名是隐藏文件，需要使用 `ls -a` 命令才能列出
- 文件名与命令是大小写敏感的
- Linux 没有 “文件扩展名” 的概念
- Linux 支持长文件名，文件名可以包含空格和标点符号（仅限使用 `.` `-` `_` ），但是不建议使用空格，因为在终端输入时，空格需要做特殊处理

#### 命令

大多数命令的格式

```shell
command -options arguments
```

命令名（command）经常带有一个或多个用来更正命令行为的选项（-options），选项（-options）后面可能会带有一个或多个参数（arguments），这些参数（arguments）是命令作用的对象

由一个 `-` 构成的选项叫短选项，由两个 `-` 构成的选项叫长选项

**date**	显示系统当前时间和日期

```shell
dean@DESKTOP-MIIC8NT:~$ date
Thu Apr  9 23:11:37 CST 2020
```

**cal**	默认显示当前月份的日历

```shell
dean@DESKTOP-MIIC8NT:~$ cal
     April 2020
Su Mo Tu We Th Fr Sa
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28 29 30

```

**df**	查看磁盘剩余空间的数量

```shell
dean@DESKTOP-MIIC8NT:~$ df
Filesystem      1K-blocks      Used Available Use% Mounted on
rootfs          233768956 107476800 126292156  46% /
none            233768956 107476800 126292156  46% /dev
none            233768956 107476800 126292156  46% /run
none            233768956 107476800 126292156  46% /run/lock
none            233768956 107476800 126292156  46% /run/shm
none            233768956 107476800 126292156  46% /run/user
cgroup          233768956 107476800 126292156  46% /sys/fs/cgroup
C:\             233768956 107476800 126292156  46% /mnt/c
D:\             104862716  47925616  56937100  46% /mnt/d
E:\             104862716  52404456  52458260  50% /mnt/e
F:\            1048575996 141180616 907395380  14% /mnt/f
G:\             278654304 204726324  73927980  74% /
```

**free**	显示空闲内存的数量

```shell
dean@DESKTOP-MIIC8NT:~$ free
              total        used        free      shared  buff/cache   available
Mem:       16726964     6021524    10476088       17720      229352    10571708
Swap:      29221116        1284    29219832
```

**exit**	结束终端会话

```shell
dean@DESKTOP-MIIC8NT:~$ exit
```

**pwd** (print working directory)	显示当前工作目录

```shell
dean@DESKTOP-MIIC8NT:~$ pwd
/home/dean
```

**ls**	列出一个目录包含的文件及子目录，默认为当前工作目录，可指定其他目录，甚至是多个目录

```shell
dean@DESKTOP-MIIC8NT:~$ ls
WORKING_DIRECTORY  bin
dean@DESKTOP-MIIC8NT:~$ ls /usr
bin  games  include  lib  local  sbin  share  src
dean@DESKTOP-MIIC8NT:~$ ls /var /usr
/usr:
bin  games  include  lib  local  sbin  share  src

/var:
backups  cache  crash  lib  local  lock  log  mail  opt  run  snap  spool  tmp
```

**cd**	更改当前工作目录，支持绝对路径和相对路径

```shell
dean@DESKTOP-MIIC8NT:~/bin$ cd ../WORKING_DIRECTORY/
dean@DESKTOP-MIIC8NT:~/WORKING_DIRECTORY$ 
```

*cd 快捷键*

| cd           | 更改工作目录到用户的家目录                        |
| ------------ | ------------------------------------------------- |
| cd -         | 更改工作目录到之前的目录                          |
| cd ~xiaoming | 更改工作目录到指定的用户（比如 xiaoming）的家目录 |