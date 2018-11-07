## CMD（Common Module Definition）

CMD与AMD一样也是一种模块定义规范, 明确了模块的基本书写格式和基本交互规则

## 特点

- 一个文件就是一个独立的模块
- 依赖就近加载（即在使用的时候在加载依赖项）， 延迟执行

## 实现

CMD是一种浏览器异步加载规范， 那么这种规范怎么应用到浏览器呢？ 

Sea.js即是一个遵循CMD规范的工具库

## 定义模块

    define(factory)

factory参数可以是一个函数，也可以是一个对象或者字符串

    define({
        name: 'as'
    })

    define('ncd jdkjsfk fjsdkjf');

    define(function(require, exports, module) {
        ...
    })
- require: 用来获取其他模块提供的接口， require('a.js')
- exports: 是一个对象， 用来对外提供该模块的导出内容
- module: 是一个对象， 存储了与当前模块相关的一些属性和方法

例：

    a.js
    define(function(require, exports, module){
        exports.doSomeing = function() {
            console.log('a-dosomeing');
        }
    })
    b.js
    define(function(require, exports, module){
        exports.doSomeing = function() {
            console.log('b-dosomeing');
        }
        //或者module.exports对外暴露整个接口
        module.exports = {
            doSomeing: function() {
                console.log('b-dosomeing');
            }
        }

    })
    main.js
    define(function(require, exports, module) {
        const o1 = require('./a');
        // 也可以异步加载模块，在加载完成时，执行回调
        <!-- require.async('./a', function(b) {
            b.doSomething();
        }); -->
        o1.doSomeing();
        const o2 = require('./b'); // 就近引入
        o2.doSomeing();
    })

## 加载模块-sea.js

加载上面定义好的模块

    seajs.use('main.js')

    seajs.use(['a.js'], function(obja) {
        obja.doSomeing();
    })

    seajs.use(['a.js', 'b.js'], function(obja, objb) {
        obja.doSomeing();
        objb.doSomeing();
    })

#### sea.js特性

- 简单友好的模块定义规范：[sea.js](https://seajs.github.io/seajs/docs/#quick-start) 遵循 CMD 规范，可以像 Node.js 一般书写模块代码。
- 自然直观的代码组织方式：依赖的自动加载、配置的简洁清晰，可以让我们更多地享受编码的乐趣。

#### 配置sea.js

    seajs.config({
        // 设置路径，方便跨目录调用
        paths: {
            'arale': 'https://a.alipayobjects.com/arale',
            'jquery': 'https://a.alipayobjects.com/jquery'
        },

        // 设置别名，方便调用
        alias: {
            'class': 'arale/class/1.0.0/class',
            'jquery': 'jquery/jquery/1.10.1/jquery'
        }
    });

#### 构建工具
- spm
- grunt
