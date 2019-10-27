###  [转载防封]最好用的 V2Ray 一键安装脚本 & 管理脚本

支持 V2Ray 多数传输协议，WebSocket + TLS，Shadowsocks，动态端口，集成 BBR 和锐速优化等。

#### 前言

V2Ray 是一个于 Shadowsocks 之后非常好用的代理软件，但是由于 V2Ray 的配置略复杂，GUI 客户端不完善，所以 V2Ray 并没有像 Shadowsocks 在科学上网人群之中那么流行。
不过我想，像我这种小小白萌新，更需要的是一个好用的一键安装脚本……
所以，此脚本是为了方便像我这种小小白萌新更加容易去使用 V2Ray，配置 V2Ray。希望对你有帮助 ^_^

 

#### 功能特点

1. 支持 V2Ray 多数传输协议
2. 支持 WebSocket + TLS
3. 支持 动态端口 (WebSocket + TLS 除外)
4. 支持 屏蔽广告
5. 支持 配置 Shadowsocks
6. 支持 下载客户端配置文件 (不用 Xshell 也可以下载)
7. 客户端配置文件同时支持 SOCKS 和 HTTP
8. 支持 生成 V2Ray 配置二维码链接 (仅适用部分客户端)
9. 支持 生成 V2Ray 配置信息链接
10. 支持 生成 Shadowsocks 配置二维码链接
11. 支持修改 V2Ray 传输协议
12. 支持修改 V2Ray 端口
13. 支持修改 动态端口
14. 支持修改 用户ID
15. 支持修改 TLS 域名
16. 支持修改 Shadowsocks 端口
17. 支持修改 Shadowsocks 密码
18. 支持修改 Shadowsocks 加密协议
19. 自动启用 BBR 优化 (如果内核支持)
20. 集成可选安装 BBR (by teddysun.com)
21. 集成可选安装 锐速 (by moeclub.org)
22. 一键 查看运行状态 / 查看配置信息 / 启动 / 停止 / 重启 / 更新 / 卸载 / 等等…
23. 人性化向导 & 纯净安装 & 卸载彻底

哈哈哈..我故意要写够 23 条的。说着当然，脚本肯定都会有如上所说的功能。

#### 安装或卸载

要求：Ubuntu 14+ / Debian 7+ / CentOS 7+ 系统的小鸡鸡
推荐使用 Debian 9 系统，脚本会自动启用 BBR 优化。
使用 root 用户输入下面命令安装或卸载

```
bash <(curl -s -L https://git.io/v2ray.sh)
```

> 如果提示 curl: command not found ，那是因为你的小鸡没装 Curl
> ubuntu/debian 系统安装 Curl 方法: `apt-get update -y && apt-get install curl -y`
> centos 系统安装 Curl 方法: `yum update -y && yum install curl -y`
> 安装好 curl 之后就能安装脚本了

注意事项：如果你是 CentOS 7 系统，此脚本会关闭 firewalld 并且使用 iptables
备注：安装完成后，输入 `v2ray` 即可管理 V2Ray
如果提示你的系统不支持此脚本，那么请尝试更换系统

下面是此脚本的一些截图

安装选项

![安装过程](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/1v2ray(1).png?raw=true)

配置 Shadowsocks，可以不配置，也可以用

![Shadowsocks 配置](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/1v2ray(2).png?raw=true)

安装完成

![V2Ray 安装完成](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/1v2ray(3).png?raw=true)

管理面板

![V2Ray 管理面板](https://github.com/Dean1990/GitHub-is-good-store/blob/master/image/v2ray_install/1v2ray(4).png?raw=true)



#### 快速管理

`v2ray info` 查看 V2Ray 配置信息
`v2ray config` 修改 V2Ray 配置
`v2ray link` 生成 V2Ray 配置文件链接
`v2ray infolink` 生成 V2Ray 配置信息链接
`v2ray qr` 生成 V2Ray 配置二维码链接
`v2ray ss` 修改 Shadowsocks 配置
`v2ray ssinfo` 查看 Shadowsocks 配置信息
`v2ray ssqr` 生成 Shadowsocks 配置二维码链接
`v2ray status` 查看 V2Ray 运行状态
`v2ray start` 启动 V2Ray
`v2ray stop` 停止 V2Ray
`v2ray restart` 重启 V2Ray
`v2ray log` 查看 V2Ray 运行日志
`v2ray update` 更新 V2Ray
`v2ray update.sh` 更新 V2Ray 管理脚本
`v2ray uninstall` 卸载 V2Ray

#### 备注

V2Ray 客户端配置文件解压密码为 `233blog.com`，SOCKS 监听端口为 `2333`， HTTP 监听端口为 `6666`
可能某些 V2Ray 客户端的选项或描述略有不同，但事实上，此脚本显示的 V2Ray 配置信息已经足够详细，由于客户端的不同，请对号入座。

#### 反馈问题

Telegram 群组：[ https://t.me/blog233](https://t.me/blog233)
Github 反馈：[ https://github.com/233boy/v2ray/issues](https://github.com/233boy/v2ray/issues)
任何有关于 V2Ray 的问题，请自行到 V2Ray 官方反馈。
**目前不支持 V2Ray 多用户…不支持 Shadowsocks 多用户。。不支持 SSR。。**
**使用国际大厂的小鸡鸡，请自行在安全组 (防火墙) 开放端口和 UDP 协议 (如果你要使用含有 mKCP 的传输协议)**

#### 补充

## TCP 阻断

如果你觉得你的小鸡出现了这种情况，那么可以尝试使用 UDP 协议相关的 mKCP
当然，用了我的脚本那是很简单的啦，直接输入 `v2ray config` 然后选择修改 V2Ray 传输协议
之后再选择 mKCP 相关的就行咯
备注：使用 mKCP 或许还可以提高速度，但由于 UDP 的原因也许会被运营商 Qos，这是无解的。

## 优化 V2Ray

由于本人的脚本在 Debian9 系统会自动开启 BBR 优化加速了，所以不需要再安装 BBR 优化了，
如果你还是觉得网络比较慢的话，你可以尝试使用含有 mKCP 的传输协议，这个 mKCP 的东东，简单一点说就像 Kcptun 一样加速，并且还能进行伪装 (可选)，但是由于 mKCP 是使用 UDP 协议的，也许运营商会限速得更加厉害，网络变得更加慢。但不管怎么样，你都可以随时尝试一下。
服务器输入 `v2ray config` 回车，然后选择 修改 V2Ray 传输协议，再接着选择 mKCP 相关的传输协议即可
如果你是使用其他商家的 VPS 并且是按照此教程流程来安装 V2Ray 的话，那么你可以输入 `v2ray bbr` 回车，然后选择安装 BBR 或者 锐速 来优化 V2Ray
只是还想再啰嗦一下，如果你是使用国际大厂的 VPS，并且是按照此教程流程来安装 V2Ray 的话，请自行在安全组 (防火墙) 开放端口和 UDP 协议 (如果你要使用含有 mKCP 的传输协议)

## WebSocket + TLS

实现 WebSocket + TLS 超级无敌简单，前提是要拥有一个能正常解析的域名 (并且知道怎么解析域名)
服务器输入 `v2ray config` 回车，然后选择 修改 V2Ray 传输协议，再选择 WebSocket + TLS，即是输入 4，接着输入你的域名，然后我都懒得说了，脚本都那么简单明了，我还瞎BB干嘛…
哈哈…可能有不少人在折腾 V2Ray 实现 WS + TLS 的时候真的是搞到很蛋痛咯，有些人的教程可能说得不是很清楚，或者是直接忽略小白萌新这些亲爱的用户，嗯，小白们好好加油吧，请尽量多学一些基础知识，别总是做伸手党，对于毫无交集的陌生人，人家并没有任何义务要帮你的啊
偷偷跟你说…使用 WebSocket + TLS 会有断流的问题
多说一句，不要被某些人带节奏，WS + TLS 并不是 V2Ray 的神级配置，该墙还是会墙，墙你不需要理由
备注一下啦，这里我没写怎么教你注册域名啦，怎么解析域名啦，如果你真的想要使用 WebSocket + TLS，那就 自己谷歌摸索一下，其实好简单的啦！
我本人并没有在使用 WS + TLS (WebSocket + TLS)，我用 TCP，就是用一键脚本全程回车的那种懒人

## HTTP/2

实现 HTTP/2 (h2) 也超级无敌简单，和 WebSocket + TLS 一样，也就是只要一个域名就够
服务器输入 `v2ray config` 回车，然后选择 修改 V2Ray 传输协议，再选择 HTTP/2，即是输入 16，然后………看上面的 WebSocket + TLS 的相关。
备注一下，HTTP/2 相比 WS + TLS (WebSocket + TLS) ，在浏览网页时有一些优势。速度是差不多的啦

## mKCP

mKCP 这个东东其实就是 KCP 协议，反正你知道是能提速的就行，但是不保证都能提速，还能避免 TCP 阻断，但是也可以会被运营商 Qos.
使用方法：服务器输入 `v2ray config` 回车，然后选择 修改 V2Ray 传输协议，之后再选择 mKCP 相关的就行

## 搬瓦工 VPS 速度慢

由于本教程使用了 搬瓦工 VPS 做为教程的一部分，那么相信有些新接触 VPS 的同学可能会是按照教程使用了 搬瓦工 VPS 翻墙。
如果你觉得搬瓦工 VPS 速度慢，你可以尝试修改一下端口，服务器输入 `v2ray config` ，，然后选择 修改 V2Ray 端口 即可，建议使用常见的端口，比如说 443，当然，为了更加安全隐蔽，你可以直接尝试使用 WebSocket + TLS 或者 HTTP/2 协议，但是使用这两个协议对于没有接触过 域名 的同学相对来说会是比较困难的。
搬瓦工 VPS 速度慢的一个主要原因可能会是因为端口限速，如果你已经修改端口为 443，速度还是慢的话，我建议你尝试使用 mKCP 协议。

## Telegram 专用代理

如果你在使用 Telegram 的话，你可以配置一个 Telegram 的专用代理，这样来，在某些情况下你就不需要再开一个代理软件了。
输入 `v2ray tg` 即可配置 TG 专用代理
配置 Telegram MTProto

![配置Telegram MTProto](https://camo.githubusercontent.com/76790417fd184b9156d04f34d0020cf42e950ec9/68747470733a2f2f692e6c6f6c692e6e65742f323031392f30312f30352f356333303532323438656137342e6a7067)

Telegram MTProto 配置信息

![Telegram MTProto 配置信息](https://camo.githubusercontent.com/f95d1c5bbe481b175defd9768972498250c7bef4/68747470733a2f2f692e6c6f6c692e6e65742f323031392f30312f30352f356333303532323438613130662e6a7067)

## V2Ray 多用户

目前此 V2Ray 一键脚本只支持配置一个 V2Ray 账号…一个 Shadowsocks 账号
说着当然，如果你是大佬，配置 多用户 这种事，不是分分钟的事么？

## 查看配置 / 修改配置 / 端口 / 传输协议…… ？

请看上面的快速管理。。。或者直接输入 `v2ray` 回车，找到你想要执行的功能。

## 哪个传输协议好？

心中无杂念，用 TCP
ISP 常作怪，用 动态端口
小鸡速度不好，用 mKCP
处子之身，用 WS + TLS

## V2Ray 脚本说明

[V2Ray 一键安装脚本](https://github.com/233boy/v2ray/wiki/V2Ray一键安装脚本)

## 反馈问题

请先查阅：[V2Ray 一键安装脚本疑问集合](https://233v2.com/post/10/)
Telegram 群组：[ https://t.me/blog233](https://t.me/blog233)
Github 反馈：[ https://github.com/233boy/v2ray/issues](https://github.com/233boy/v2ray/issues)
任何有关于 V2Ray 的问题，请自行到 V2Ray 官方反馈。
**目前只支持配置一个 V2Ray 账号…一个 Shadowsocks 账号。。不支持 SSR。。**

## 分享

如果这篇文章对你帮助的话，记得分享给你的小伙伴们。

## 其他

请勿违反国家法律法规，否则后果自负！
低调低调低调。

## 资助 V2Ray

如果你觉得 V2Ray 很好用，能解决你的某些问题，请考虑 [资助 V2Ray 发展 ](https://www.v2ray.com/chapter_00/02_donate.html)。

## 结束

我有写少了什么吗？我这种小小白萌新看了这教程都觉得很明白了啊。
一次不会，那么就两次，还是不会，那就再来一次。可还是不会啊？大佬请收下我的膝盖。

![img](https://camo.githubusercontent.com/0bc4a705093d07e48104a20d4f0ff5ce5f28c2a2/68747470733a2f2f616666706173732e636f6d2f67613f67613d76327261792664743d6769746875622e77696b692e322664723d26756c3d7a682d434e2673643d32342d6269742673723d2676703d267a3d3026646c3d2f6769746875622f32)

[返回主页](https://github.com/233boy/v2ray/wiki)

###  Pages 5



- **Home**
- **V2RayN使用教程**
- **V2Ray一键安装脚本**
- **V2Ray搭建详细图文教程**
- **使用Cloudflare中转V2Ray流量**

##### Clone this wiki locally