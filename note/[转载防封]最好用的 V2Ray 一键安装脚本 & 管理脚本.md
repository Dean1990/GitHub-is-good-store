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