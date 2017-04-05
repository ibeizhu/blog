---
title: vue-plugin
date: 2016-10-20 18:39:51
tags:
---
## Vue.js插件书写规范

# Vue插件书写规范，导出一个带有install方法的对象

使用时，可以通过官方写法将自定义插件挂载到Vue上面：
```
import Vue from 'vue'
import YourPlugin from 'YourPlugin'
Vue.use(YourPlugin)
```

# 闭包里导出一个带有install方法的对象

```
(function () {

   var YourPlugin = {
      install:function(Vue,options){
        // todo your code,such as
        // Vue.component('CustomComponent', CustomComponent)
      }
   }
  // 这里为了支持CMD,AMD,CommonJs,以及script标签导入的方式
  if (typeof exports == "object") {
    module.exports = YourPlugin
  } else if (typeof define == "function" && define.amd) {
    define([], function(){ return YourPlugin })
  } else if (window.Vue) {
    Vue.use(YourPlugin)
  }

})()
```

#### 至于为什么要这样写,为什么要导出install方法？相信大家肯定有疑问，这里摘录Vue源码的use方法,大家就能一目了然
```
Vue.use = function (plugin) {
    /* istanbul ignore if */
    if (plugin.installed) {
      return
    }
    // additional parameters
    var args = toArray(arguments, 1)
    args.unshift(this)
    if (typeof plugin.install === 'function') {
      plugin.install.apply(plugin, args)
    } else {
      plugin.apply(null, args)
    }
    plugin.installed = true
    return this
  }
```
#### 当前后续最好将插件通过npm publish发布到npm包里面，这样其他小伙伴就能通过以下方式直接安装了
```
npm install YourPlugin
```