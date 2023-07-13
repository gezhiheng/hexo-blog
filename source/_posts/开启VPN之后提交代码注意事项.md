---
title: 开启VPN之后提交代码注意事项
date: 2023-07-13 08:46:15
tags: [Tip, Git]
---
## 问题
开启VPN之后提交与拉取代码可能会出现如下错误
```
OpenSSL SSL_read: Connection was reset, errno 10054
```
翻译成中文 打开SSL SSL_read：连接已重置，错误 10054。
这样解释可能也比较模糊，通俗点说服务器的SSL证书没有经过第三方机构的签署。但也有人说可能是网络不稳定，连接超时导致。
## 解决方案
更改git配置
```
git config --global http.sslVerify "false"
```
如果上述指令无法解决你的问题，可以执行如下指令：
```
git config --global https.sslVerify "false"
```
## 连接超时
出现这种错误
```
Failed to connect to github.com port 443 : Timed out
```
这个时候就需要去查看代理的IP地址和端口号
![](https://my-pic-base.oss-cn-beijing.aliyuncs.com/undefinedblog-vpn-1.jpg)
如上图所示，地址和端口号为127.0.0.1:10809，然后去修改git的网络设置，将其修改为代理的地址和端口号
```
git config --global http.proxy http://127.0.0.1:10809 
git config --global https.proxy http://127.0.0.1:10809
```
最后重新拉取和提交代码就可以了