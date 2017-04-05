---
title: webstorm-vue
date: 2016-11-11 18:33:49
tags: webstorm vue es6
---
## Webstorm Vue es6报错解决方案

Webstorm项目设置为Ecmascript6

Webstorm安装Vuejs插件

然后修改.vue文件中的script标签修改为如下格式

```
<script type="text/babel">
```

### 如果你使用less的话,.vue文件中的style标签修改为如下格式
```
<style rel="stylesheet/less" lang="less" scoped>
```

通过如上两个修改,可以友好的使用vue开发了