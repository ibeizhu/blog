---
title: mac OSX系统下使用charles 抓 mobile 的包
date: 2017-04-20 18:57:11
tags: charles mobile proxy mac
---
## mac OSX系统下使用charles 抓 mobile 的包

1.首先手机和Mac必须在同一局域网下,比如都连接了A wifi,

2.手机设置网络代理,iphone下 打开设置手动代理
路径打开 : *设置->Wi-Fi->A wifi* ,找到HTTP代理,手动代理
填写服务器和端口号,比如:

```
192.168.8.132
8888
```

3、此时理论上可以抓包了

4、对于抓取HTTPS请求,需要在手机上安装charles证书
打开charles,按照以下步骤:
*Help->SSL Proxying->Install Charles Root Certificate on a Mobile Device or Remote Brower*
然后手机浏览器打开https://charlesproxy.com/getssl 地址下载ssl证书

5.在charles的设置SSL Proxy Settings
*Proxy->SSL Proxy Settings*
添加你想要抓包的https域名,端口设置为443即可

完成以上步骤就可以使用charles抓mobile的包了