---
title: ssh安全协议
date: 2017-12-07 10:09:29
tags: ssh ssh-keygen
---

## ssh好文
[SSH Essentials: Working with SSH Servers, Clients, and Keys](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
[SSH要领：用SSH服务器，客户机和Keys工作](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
[SSH免密码登录](https://my.oschina.net/aiguozhe/blog/33994)

## ssh 安全加密原理

客户端通常使用非常安全的密码（不太安全，不推荐）或SSH密钥进行身份验证。 密码登录是加密的，对新用户来说很容易理解。然而，自动机器人和恶意用户经常会反复尝试对允许基于密码登录的帐户进行身份验证，这可能导致安全性损失。因此，我们建议您为大多数配置始终设置基于SSH的身份验证。 SSH密钥是可以用于认证的密码密钥的匹配集合。每个集合包含公共密钥和私钥。公钥可以自由地共享，而不用担心，而私钥必须警惕保护，不要暴露给任何人。 要使用SSH密钥进行身份验证，用户必须在其本地计算机上具有SSH密钥对。在远程服务器上，公共密钥必须被复制到用户的主目录中的文件~/.ssh/authorized_keys 。此文件包含有权登录此帐户的公钥的列表，每行一个。 当客户端连接到主机时，希望使用SSH密钥身份验证，它将通知服务器此意图，并告知服务器要使用哪个公钥。然后，服务器检查它authorized_keys文件的公共密钥，生成一个随机字符串，并使用公钥加密。此加密消息只能使用关联的私钥进行解密。服务器将这个加密的消息发送到客户端，以测试它们是否实际上具有相关联的私钥。 在接收到该消息时，客户端将使用私钥对其进行解密，并将所显示的随机串与先前协商的会话ID组合。然后它生成此值的MD5哈希值并将其发送回服务器。服务器已经具有原始消息和会话ID，因此它可以比较由这些值生成的MD5散列并确定客户端必须具有私钥。 现在你知道SSH的工作原理，我们可以开始讨论一些例子来演示使用SSH的不同方法

## ssh 常用命令

### 生成ssh秘钥对
```shell
ssh-keygen
ssh-keygen -t rsa -C "xxx@xxx.com"
ssh-keygen -t rsa -C "xxx@xxx.com" -f ~/.ssh/alias
```

### 登录远程服务器

```shell
ssh root@100.100.100.100   // ssh默认端口是 22
ssh root@100.100.100.100 -p 3000    // 指定端口
ssh -i ~/.ssh/yun root@100.100.100.100  // 指定公钥，默认使用id_rsa
```

### 复制本地公钥到服务器
```
ssh root@100.100.100.100 "mkdir .ssh;chmod 0700 .ssh"
scp ~/.ssh/id_rsa.pub root@172.24.253.2:.ssh/id_rsa.pub
```

### ssh 调试

```
ssh root@100.100.100.100 -vvv
```