---
title: mysql-pm2
date: 2017-05-12 19:20:40
tags: mysql pm2 thinkjs pm2 kill
---
## 关于新服务器的系统迁移

最近购买了腾讯云服务器，正在把之前的搬瓦工上面的系统逐渐迁移到新服务器上，其中涉及到基于thinkjs的数据迁移

#### 1.第一步，首先将老服务器上面的mysql数据库导出，保证数据的同步
```
mysqldump -u root -p root --databases moment.db >/root/project/data.sql
```

#### 2.在新系统安装mysql,使用yum安装
```
yum -y install mysql-server mysql
```

#### 3.登录mysql,导入刚刚导出的数据文件data.sql
```
mysql -u root -p
source /root/project/data.sql
```

#### 4.以上步骤已经完成了数据库的数据同步，最后基于pm2发布服务

这里我遇到了一个小坑，因为换了个服务器，然后我的项目文件目录路径和之前有了改变，而我项目的pm2.json
文件的cwd指向了之前的老目录，导致 pm2 start pm2.json时访问页面一直502，之后使用pm2 show命令查看了error log,
发现一直报错"ENOENT: no such file or directory, uv_chdir". 其实就是pm2的启动服务的js文件找不到，发现
问题之后，使用pm2 stop,然后pm2 start pm2.json还是不行，后来使用pm2 kill杀掉进程，在执行pm2 start pm2.json大功告成。
