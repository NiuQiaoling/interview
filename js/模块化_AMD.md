## AMD - 异步模块定义

AMD规范是从CommonJS的讨论中诞生的， 优先照顾浏览器的模块加载场景， 采用**异步加载和回调**的方式。

## 由来

AMD 是 require.js 在推广过程中对模块定义的规范化产出.

AMD的诞生， 主要解决这两个问题：
- 实现javascript代码的异步加载， 避免阻塞造成网页失去响应
- 管理模块间的依赖

## 特点

- 一个单独的文件便是一个独立的模块
- 异步加载模块， 也可以动态加载模块
- 使用difine定义模块， 显示模块依赖关系
- 使用require加载模块


## 实现

AMD是一种浏览器异步加载规范， 那么这种规范怎么应用到浏览器呢？ 

RequireJS即是一个遵循AMD规范的工具库， 主要用于客户端的模块管理， 异步或者动态加载模块

## 定义模块

AMD规范只定义一个函数define(), 是全局变量

定义：

    define(id?, dependencies?, factory);
    即：define(模块名(可选), 模块依赖列表(可选), 工厂函数);

#### id

第一个参数 id, 是一个字符串， 表示定义模块的名字， 可选参数。

如果没有提供该参数， 模块的名字应该默认为模块加载器请求的脚本的名字；

#### dependencies

第二个参数 dependencies， 是定义中模块所依赖的所有其他模块组成的数组， 可选参数

每个依赖的模块的输出将作为参数依次传入factory中

如果忽略该参数， 则默认为["require", "exports", "module"]

#### factory

第三个参数 factory， 是一个函数或者对象， 如果是函数， 那么他的返回值就是模块的对外输出值

#### 定义无依赖模块

有些模块可以不依赖其他模块而存在， 我们可以这样定义

    /js/m1.js
    define({
        count: 0,
        addCount: function() {
            return ++this.count;
        },
    })

    或者：
    define(function() {
        ...
        return {
            count: 0,
            addCount: function() {
                return ++this.count;
            },
        }
    })

#### 定义有依赖的模块

有些模块需要依赖其他模块， 我们可以这样定义

    /js/m2.js
    define(['./m1.js'], function(objCount) {
        var count = objCount.addCount();
        ...
    })
    
    或者：
    define(['m1'], function(objCount) {
        var count = objCount.addCount();
        ...
    })
    m1指的是当前目录下的m1.js文件

如果一个模块依赖很多其他模块， 如下：

    define(['m1', 'm2', 'm3', 'm4', 'm5', 'm6'], function(m1, m2, m3, m4, m5, m6) {
        var count = objCount.addCount();
        ...
    })
我们也可以这样写

    define(function(require, exports, module) {
        var m1 = require('m1');
        var m2 = require('m2');
        var m3 = require('m3');
        var m4 = require('m4');
        var m5 = require('m5');
        var m6 = require('m6');
    })

#### 定义具名模块

    define('myModule', ['m1'], function(objCount) {
        var count = objCount.addCount();
        ...
    })


## 加载模块-require.js

require.js是一个遵循AMD规范的工具库， 用来加载符合AMD规范定义的模块(AMD的define定义的模块)

    require([module], callback)

第一个参数是一个数组， 包含加载的模块

第二个参数是加载成功后的回调函数

    /js/m1.js
    define({
        count: 0,
        addCount: function() {
            return ++this.count;
        },
    })
    /js/m2.js
    define(['./m1.js'], function(objCount) {
        var count = objCount.addCount();
        ...
    })
    require(['./m2.js'], function(m2) {
        ...
    })

    当require()在加载m2.js模块的时候会先加载m1.js

#### 加载非规范的模块

理论上，require.js 加载的模块，必须是按照 AMD 规范、用 define() 函数定义的模块。

但是require.js也可以加载非规范定义的模块， 不过在加载之前， 需要用require.config()方法， 定义他们的特征

require.config() 接受一个配置对象，
- paths 属性之外，

    paths参数指定各个模块的位置, 可以是相对位置， 也可以是外网网址

- 还有一个 shim 属性，专门用来配置不兼容的模块。具体来说，每个模块要定义：
    - exports 值（输出的变量名），表明这个模块外部调用时的名称；
    - deps 数组，表明该模块的依赖性。

比如，jQuery 的插件可以这样定义：

    require.config({
        paths: {
            "backbone": "vendor/backbone",
            "underscore": "vendor/underscore"
        },
        shim: {
            'underscore': {
        　　　  exports: '_'
        　　},
        　　 'backbone': {
            　　　deps: ['underscore', 'jquery'],
            　　　exports: 'Backbone'
        　　}
        }
    });

## 注意点

- 尽量不要提供模块的 ID，如 AMD 规范所述，这个 ID 是可选项，如果提供了，在 RequireJS 的实现中会影响模块的可迁移性，文件位置变化会导致需要手动修改该 ID。
- 每个 JavaScript 文件只定义一个模块，模块名称和文件路径的查找算法决定了这种方式是最优的，多个的模块和文件会被优化器进行优化。
- 尽量避免模块的循环依赖。

## 应用

- 引入require.js

        <script data-main="js/main" src="scripts/require.js"></script>

    data-main指的是与当前index.html同级的js文件夹下的main.作为程序的入口
    require会有先加载入口文件main.js

- 入口模块的写法

        main.js

        require(['jquery', 'backbone'], function($, _) {

        })
    这里默认'jquery', 'backbone'与main.js在同一目录下， 如果不在， 我们也可以对require进行配置

- 配置requirejs

            require.config({
                baseUrl: "js",
                paths: {
        　　　　　　  "underscore": "lib/underscore.min",
        　　　　　　  "backbone": "lib/backbone.min",
                    "jquery": "https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min",
        　　　　}，
            })
    
    配置代码可以写在入口文件main.js的头部



