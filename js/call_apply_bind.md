# call/apply/bind

在 javascript 中，

call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向。

bind方法虽然与call,apply的用法有点不同，但是其作用也是一样的，也是可以改变函数体内this的指向。

# call 和 apply 的相同点

    1. 都是Function.prototype上的方法
    2. 都是为了改变函数体内部this的指向
    3. 都接收两个参数， 第一个参数是函数运行时的 this 指向的对象
    4. 如果第一个参数是 空(不传)/null/undefined, 则默认传入 window 
    5. 如果第一个参数是一个原始值(number/boolean/string)，那么这个原始值会自动转成对应的包装对象，然后传入call/apply方法。

# call 和 apply 的异同点

## call()
----

call 方法接收两个参数， 第一个参数是函数运行时指定的this值， 第二个参数是一个参数列表。

call(thisArg, arg1, arg2);

函数实例的 call 方法， 可以指定函数内部的this指向， 即函数运行时的上下文， 然后在指定的作用域中， 调用该函数。

    function fn() {
        this.name = 'a';
        return this;
    }
    fn(); // window

    var obj = {};
    fn.call(obj); // {name: "a"}

    obj.name = 'b';
    fn.call(obj); // {name: "a"}
    fn.call('d'); String {"d", name: "a"}
    即函数fn运行时， 函数内部的this指代的是obj, 而不再是正常运行时指代的window
    
     
### 应用案例
调用对象的原生方法

    Object.prototype.toString.call('8ik')


## apply()
----

apply 方法接收两个参数， 第一个参数是函数运行时指定的this值， 第二个参数是一个数组。

apply(thisArg, [arg1, arg2]);

函数实例的 apply 方法， 可以指定函数内部的this指向， 即函数运行时的上下文， 然后在指定的作用域中， 调用该函数。

    function fn() {
        this.name = 'a';
        return this;
    }
    fn(); // window
    var obj = {
        name: 'b',
    }
    fn.apply(obj); // {name: "a"}
    即函数fn运行时， 函数内部的this指代的是obj, 而不再是正常运行时指代的window
### 应用案例
- **数组求最值**

        由于apply的特性， 我们可以直接对一个数组求最值
            var arr = [-2, 8, 0, 34, -99, 23];
            Math.max.apply(Math, arr); // 34
            Math.min.apply(Math, arr); // -99

        Math是javascript内置的一个对象， 拥有max(), min() 等方法， 接收一个参数列表， 返回参数列表的最值。

        正常工作中， 我们常常需要对一个数组求取最值， 这里即可使用apply方法。 正常情况下， Math.max(2, 0, 4), 函数max运行过程中上下文this指代的是Math, 由于这里我们还是想使用max方法，而只是改变参数列表为数组， 所以使用apply方法， 第一个参数依旧是Math, 而第二个参数传我们想要求职的数组即可。
- **合并数组**

        var arr1 = [1, 2, 3];
        var arr2 = ["a", "b", "c"];
        Array.prototype.push.apply(arr1, arr2);
        arr1:  [1, 2, 3, "a", "b", "c"]
        arr2:  ["a", "b", "c"]

- **将数组的空元素变成undefined**

        Array.apply(null, ['a', ,'b'])
        // [ 'a', undefined, 'b' ]

        空元素与undefined的差别在于，数组的forEach方法会跳过空元素，但是不会跳过undefined。因此，遍历内部元素的时候，会得到不同的结果。

        var a = ['a', , 'b'];
        function print(i) {
            console.log(i);
        }
        a.forEach(print); // a b
        Array.apply(null, a).forEach(print); // a undefined b
- **转换类似数组的对象**

    当然es6有新的方法将类数组转换为数组， 这个后续再说

        Array.prototype.slice.apply({0: 1, length: 2}); // [1, undefined]

# bind

定义：bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

    function fn(a) {
        console.log(a, this.name);
    }
    var obj = {
        name: 'a'
    }
    fn.bind(obj);
    // 运行该行代码后， 浏览器只是返回了一个新的函数， 由bind()函数创建， 函数中的this指代得是obj, 此时函数并没有直接运行。
    fn.bind(obj)(); 调用后才会运行。 // undefined 'a'
    fn.bind({name: 'b'}, 3)(); // 3 "b"

# call/apply 和 bind 的相/异同点
- 三者都可以改变函数内this的指向， 可以接收多个参数， 第一个参数都是this的指向
- bind()方法会创建一个新的函数， 只有在需要的时候再调用， 而call/apply会立即执行该函数
- 第二个参数传入方式不同


# 模拟实现 call

    Function.prototype.selfCall = function(thisArg) {
        // 不传第一个参数， 或者第一个参数是null, undefined, 当前this指向window
        let currentObj = thisArg || window;
        
        // 将调用selfCall方法的函数赋值给当前对象的属性上
        currentObj.fn = this; 
        
        // 获取参数，由于第一个参数是重定义的this指向, 所以除去第一个参数， 剩余的参数才是原函数需要的参数
        let args = [...arguments].slice(1); 
        
        // 调用函数传入参数, 所以函数是在call内部被调用的， 调用结果也在call中最终返回
        let result = currentObj.fn(...args);
        delete currentObj.fn;
        
        return result;
    } 

    function fn(a, b) {
        console.log(22, this);
        return a + b;
    }
    var obj = {
        name: 'a',
    }
    const res =  fn.selfCall(obj, 1, 2);
# 模拟实现 apply
# 模拟实现 bind


    
