---
title: redis
date: 2017-12-11 21:50:31
tags: redis
---

```angular2html
brew install redis
```

启动服务，本地默认端6379
```angular2html
redis-server
```

进入redis客户端
```angular2html
redis-cli
```

常用命令
```angular2html
keys '**'
set yourKey yourVal
get yourKey
```