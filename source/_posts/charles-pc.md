---
title: mac OSX系统下使用charles 抓PC 浏览器的包
date: 2017-04-20 18:40:34
tags: charles pc browser proxy mac
---
## mac OSX系统下使用charles 抓PC 浏览器的包

### charles 安装配置不在本次教程之中

1、勾选顶部状态栏的

*Proxy->Start Recording 和 maxOS Proxy*

2、打开mac代理配置界面,具体步骤如下:

(1)steps:*系统偏好设置->网络->Wi-Fi 高级->代理*


(2)勾选网页代理(HTTP) 和 安全网页代理(HTTPS)
代理服务器地址和端口号设置为(charles 默认端口8888)

```
127.0.0.1
8888
```

然后点击好,应用就能抓取PC 浏览器的请求了。

*chrome默认不是使用系统代理,如果发现抓不到chrome的包,请检查一下
chrome://settings/ 里面的代理设置