### V2ray服务器的安装

服务器我使用的 [Vultr](https://www.vultr.com/) ，注册充值，会给一些礼金，不过如果像我们这种只开个小服务的，基本用不上，因为 Vultr 是按时收费的，创建了服务器实例就会开始收费，不论这个服务器是否是开机状态，

而给的礼金一个月有效，所以基本占不到便宜。

#### 创建服务器实例

进入控制台，添加一个实例。

![](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/v2ray(1).png?raw=true)

选择 `Cloud Compute` ，服务器位置选择离你近的。

![](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/v2ray(2).png?raw=true)

系统选择 `CentOs` , `Debian` 或者 `Ubuntu` ，后面会使用一键v2ray的脚本，这个脚本支持这三种系统，并且对 `Debian 9` 有加速优化。服务器大小看个人需求，硬盘，CPU，内存，带宽，以及最重要的费用综合考量再决定。前面也提到过说 Vultr 是按时计费的，例如 `$5/mo` 的意思是每月最多5美元，有31天的话，最后一天白送（便宜还是可以占到的）。

![](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/v2ray(3).png?raw=true)

先创建多个实例，点击  `Deploy Now` 。

![](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/v2ray(4).png?raw=true)

然后使用 `ping` 命令测试实例的可用性和速度。

![](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/v2ray(5).png?raw=true)

附上 python 脚本，用于批量测试IP地址可用性，（复制 Products 页面内容保存在同级目录 `ips.txt` 中）。

```python
import re
import os

#读取文本内容
f = open('ips.txt','r')
content = f.read()

#正则提取IP地址
pattern = re.compile(r'\d{1,3}.\d{1,3}.\d{1,3}.\d{1,3}')
result = pattern.findall(content)
print('Extract IPs:')
print(result)

#遍历 ping
enableIps = list()
for ip in result:
    backinfo = os.system('ping -c 1 -w 1 %s'%ip)
    #backinfo 会返回 0 或 1
    #返回0 表示有应答
    #返回1 表示无应答
    #print(backinfo)
    if not backinfo:
        enableIps.append(ip)
        
print('Reply Received:')
print(enableIps)
```

通过右侧的菜单，删除实例，最后得到一个可用并速度稳定并且比较快的服务器实例。

![](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/v2ray(6).png?raw=true)

点击 `Server Details` 查看实例详情，得到 root 用户的密码，通过 `ssh` 命令 （ssh root@123.123.123.123）登录这个服务器，然后进行 v2ray 的安装配置。

![](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/v2ray(7).png?raw=true)

一键v2ray 参考这里 [https://www.nbmao.com/archives/3108](https://www.nbmao.com/archives/3108)

