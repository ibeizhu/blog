---
title: webpack-module
date: 2017-07-16 20:54:37
tags: webpack module module.exports 内存分配
---

 ## webpack模块打包导出变量详解，涉及js分配内存机制

 ### webpack打包文件时候，我们通常使用module.exports = {} 或者 exports.getName = function(){}，这样的方式来导出模块的代码，大家有没有真正了解为什么我们要这样写？其中的原理是什么？今天来详解一下。

 ### 先看几个例子

 ## Demo 1
 ```
var module =  {
    exports:{}
}

function doSomeThing(module,exports) {
	module.exports = {
		id:1001,
		name:'Moment'
	}
}

doSomeThing(module,module.exports)
console.log(module)

// 输出结果
// 形参传入引用类型，函数内部直接给__module.exports__赋予了新内存地址，
// 所以此时千万不要对__exports__导出变量，无效
//  module = {
//      exports: {
//          id:1001,
//          name:'Moment'
//      }
//  }
 ```


 ## Demo 2
 ```
var module =  {
    exports:{}
}

function doSomeThing(module,exports) {
	module.exports = {
		id:1001,
		name:'Moment'
	}
    exports.addr = 'Hangzhou';
    exports.getAddr = function(){};
}

doSomeThing(module,module.exports)
console.log(module)

//  输出结果
//  __module.exports__被赋予新的引用类型，地址指向已经改变，
// 形参__exports__的赋值还是在之前的内存地址修改，对此时的module.exports无效
//  module = {
//      exports: {
//          id:1001,
//          name:'Moment'
//      }
//  }
 ```

 ## Demo 3
 ```
var module =  {
    exports:{}
}

function doSomeThing(module,exports) {
    module.exports.id = 1001;
    module.exports.name = 'Moment';
    exports.addr = 'Hangzhou';
    exports.getAddr = function(){};
}

doSomeThing(module,module.exports)
console.log(module)

//  输出结果
//  __module.exports__ 和 __exports__内存地址指向同一处，所以修改同步生效
//  module = {
//      exports: {
//          id:1001,
//          name:'Moment',
//          addr = 'Hangzhou';
//          getAddr = function(){};
//      }
//  }
 ```


 ## Demo 4
 ```
var module =  {
    exports:{}
}

function doSomeThing(module,exports) {
    exports = {
        id:1001,
		name:'Moment'
    }
    exports.addr = 'Hangzhou';
    exports.getAddr = function(){};
}

doSomeThing(module,module.exports)
console.log(module)

//  输出结果
//  输出空对象，保持初始化的状态，exports被赋予新的内存地址，不会对原有地址改变
//  module = {
//      exports: {
//      }
//  }
 ```

 ### 综上所述，我们在使用 module.exports 或者 exports 来导出变量的注意点，千万不要去改变exports的内存地址(也就是重新赋值)

```
 exports.id = '11111'   // right
 exports.id = {}   // right
 exports = {}   // error
 module.exports.id = '1111'  // right
 module.exports.id = {}  // right
 module.exports = {}  // right,但是此时不能再使用exports导出,除非重新给exports赋值(exports = module.export;)，保持两者关联
```

### 其实只要你对js的引用类型足够了解的话，以上都不是问题，写一个最小例子
```
var a = {id:1};
var b = a;
b.id = 2;
此时a.id === 2,因为a 和b 共同指向了同一个内存地址
```

```
var a = {id:1};
var b = a;
b = {name:'b'}
此时a 还是为{id:1}，因为当b被重新赋值时，不是去修改原地址，而是给b重新分配新的内存地址，所以a的地址保存不变
```

## 最后附上webpack打包的bundle文件，关键点做了注释说明，为什么我们使用的是module.exports 和 exports来导出模块？

```
(function(modules) { 
    // webpackBootstrap
    // The module cache
    var installedModules = {};

    // The require function
    function __webpack_require__(moduleId) {

        /*--------------------------------------------------------------------------*/
        // webpack 缓存处理，对已经加载过的js模块直接从缓存拿，避免重复调用，性能优化
        /*--------------------------------------------------------------------------*/
        // Check if module is in cache
        if(installedModules[moduleId]) {
            return installedModules[moduleId].exports;
        }
        // Create a new module (and put it into the cache)
        var module = installedModules[moduleId] = {
            i: moduleId,
            l: false,
            exports: {}
        };
       /*--------------------------------------------------------------------------*/
        // 这里就是调用每个模块文件的地方，传入module.exports、module、require三个参数
        // 使得我们可以通过module.exports、module来导出模块，使用require来引入模块
        /*--------------------------------------------------------------------------*/
        // Execute the module function
        modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

        // Flag the module as loaded
        module.l = true;

       /*--------------------------------------------------------------------------*/
        // 这里是导出每个模块文件的地方
        // 通过module.exports来导出需要暴露给其他模块require的函数、变量
        /*--------------------------------------------------------------------------*/
        // Return the exports of the module
        return module.exports;
    }


    // expose the modules object (__webpack_modules__)
    __webpack_require__.m = modules;

    // expose the module cache
    __webpack_require__.c = installedModules;

    // define getter function for harmony exports
    __webpack_require__.d = function(exports, name, getter) {
        if(!__webpack_require__.o(exports, name)) {
            Object.defineProperty(exports, name, {
                configurable: false,
                enumerable: true,
                get: getter
            });
        }
    };

    // getDefaultExport function for compatibility with non-harmony modules
    __webpack_require__.n = function(module) {
        var getter = module && module.__esModule ?
            function getDefault() { return module['default']; } :
            function getModuleExports() { return module; };
        __webpack_require__.d(getter, 'a', getter);
        return getter;
    };

    // Object.prototype.hasOwnProperty.call
    __webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };

    // __webpack_public_path__
    __webpack_require__.p = "";

    // Load entry module and return exports
    return __webpack_require__(__webpack_require__.s = 0);
})([
    /*--------------------------------------------------------------------------*/
        这里是每个文件的模块代码，0,1,2三个文件id,自执行函数的数组形参传递，进行初始化
    /*--------------------------------------------------------------------------*/
    /* 0 */
    /*--------------------------------------------------------------------------*/
        这里的形参名叫module, exports, __webpack_require__，
        所以明白我们已经明白为什么要使用module.exports和exports了
        require关键字会在打包过程中被__webpack_require__替代掉
    /*--------------------------------------------------------------------------*/
    (function(module, exports, __webpack_require__) {
        eval("var a = __webpack_require__(1)\nvar b = __webpack_require__(2)\nconsole.log(a+b);\n\n\n//////////////////\n// WEBPACK FOOTER\n// ./index.js\n// module id = 0\n// module chunks = 0\n\n//# sourceURL=webpack:///./index.js?");
    }),
    /* 1 */
    (function(module, exports) {
        eval("var a = 1;\nmodule.exports = a;\n\n\n//////////////////\n// WEBPACK FOOTER\n// ./modules/a.js\n// module id = 1\n// module chunks = 0\n\n//# sourceURL=webpack:///./modules/a.js?");
    }),
    /* 2 */
    (function(module, exports) {
        eval("var b = 10;\nmodule.exports = b;\n\n\n//////////////////\n// WEBPACK FOOTER\n// ./modules/b.js\n// module id = 2\n// module chunks = 0\n\n//# sourceURL=webpack:///./modules/b.js?");
    })
]);
```