ES6增加了模块功能，可完全取代CommonJS和AMD规范， 成为浏览器和服务器通用的模块化解决方案

## 特点
- 一个js文件就是一个单独的模块， 内部所有变量， 外部无法获取
- 静态化， 在编译时就能确定模块的依赖化关系，以及输入输出的变量
- 支持异步加载
- 可以按需加载


## 加载模块
使用 **import** 加载依赖的模块

    import {f1, f2, f3} from './a.js';
    import * as f from './a.js';
    import React from 'react';
    import React, {Component} from 'react';

- import命令在编译时执行， 也就是在代码运行前
- import命令存在提升效果， 会提升到模块顶部执行
    
## 导出接口
使用 **export** 导出对外的接口

    export var a = '111';
    export function a() {}
    export {
        a: '111',
        f: function() {}
    }
    function fn() {}
    export {
        a as stra, // 重命名
        fn as deleteFn,
    }
    import {stra, deleteFn} from './a.js';

## import()函数
- import命令能够接受什么参数，import()函数就能接受什么参数，两者区别主要是后者为动态加载。
- import()返回一个 Promise 对象

        if(1) {
            import('a.js')
            .then(ret => {
                ...
            })
        } else {
            import('b.js')
            .then(ret => {
                ...
            })
        }
        
使用场景：

    - 按需加载
    - 条件加载
    - 动态的模块路径