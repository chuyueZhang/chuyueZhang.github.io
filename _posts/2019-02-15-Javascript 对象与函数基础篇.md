---
layout:     post
title:      Javascript 基础
subtitle:   Javascript 对象与函数基础篇
date:       2019-02-16
author:     QY
header-img: img/post-bg-coffee.jpg
catalog: true
tags:
    - Javascript
    - 基础知识
    - 对象与函数
---
# 前言

>js中最复杂也是最难的知识点就是函数与对象了，由于内容实在太多，便拆分成基础篇与进阶篇，我们先来看看基础篇都有什么内容吧~

#### 对象

* 对象属于复合数据类型，有三种，分别是：

    1. 内建对象
        * 由ES标准中内建的对象，在任何的ES实现中都可以使用，如`Math, String, Boolean, Number, Function, Object...`
    2. 宿主对象
        * 由js运行的环境所提供的对象，一般指浏览器，如`DOM, BOM...`
    3. 自定义对象
        * 由开发人员自定义的对象

###### 创建对象

 * new所调用的函数是一个构造函数`constructor()`，构造函数是专门用来创建对象的函数
 * 使用typeof语句会返回object
```javascript
var obj = new Object();
console.log(typeof obj);    //"object"
```

###### 增加属性

  * 语法：

      对象.属性名 = 属性值;

      `obj.name = "111";`

  * 属性名可以不遵循标识符的规范，不遵循规范时需要其他方式来增删查改但是一般尽量遵守规范，属性值可以是任何数据类型，包括null,undefined,object当是object时可以无限嵌套
```javascript
var obj2 = new Object();
obj2.name = "333";
obj.test = obj2;
```

###### 修改属性

* 语法：

    对象.属性名 = 属性值;

    `obj.name = "222";`

* 与增加属性方法类似，只是将已有值覆盖

###### 查询属性
- 语法：

    对象.属性名

    `console.log(obj.name); //222`

- 当对象的属性为另一个对象时.重复使用来获取对象的对象的属性值

    `console.log(obj.test.name);  //333`

###### 删除属性
- 语法：

    delete 对象.属性

    `delete obj.name;`

    `console.log(obj.name); //undefined`

- 当查询对象的某个属性不存在时，会返回undefined
  
###### 使用中括号

- 当属性名没有遵循标识符规范时需要使用[]来增删查改相应属性，属性名可以是变量或者字符串
    - 语法：

        对象[属性名] = 属性值

```javascript
        obj["123"] = 345;  //字符串
        var test = "123";
        console.log(obj[test]);  //变量
        //结果为345
```

###### in语句

用来查询某个对象是否有相应属性名,属性名必须是字符串或者是变量，有则返回true，没有返回false

- 语法：

    属性名 in 对象

    `console.log("123" in obj);`

    结果为true

###### 基本和引用数据类型

在js中，内存分为栈内存和堆内存，因为基本数据大小一般比较小，js专门将这些数据存放在固定的内存范围内即栈内存来保存变量与变量值，而引用数据大小一般较大，js需要创建新内存空间即堆内存来保存对象的内容

1. 当声明变量时，会在栈内存最下层中新建一个变量
2. 取值时按照声明顺序取值
3. 当调用new新建对象时会在堆内存中创建新的内存空间来新建一个对象
4. 由于新建的内存地址不确定，取对象时需要用相应内存地址取用相应的对象
5. 给变量赋值为基本数据类型时，会直接修改栈内存中变量对应的变量值为相应的变量值
6. 给变量赋值为引用数据类型时，会直接修改栈内存中变量对应的变量值为内存地址
7. 两个变量的内存地址指向同一个对象时，修改一个变量的对象的值，另一个变量的对象的值也会发生改变

```javascript
    var obj1 = new Object();
    obj.name = "111";
    var a = obj1,b = obj1;
    a.name = "222";
    console.log(b.name);
    //结果为222
```

###### 对象字面量

- 可以使用对象字面量来新建对象，效果与new Object()相同
- 语法：

    {属性名: 属性值, 属性名: 属性值...};

    `var obj = {name: "a", age: "16", gender: "男"};`

- 属性名可以使用引号包起来，但是一般不使用，当属性名不遵循标识符规范时，需要使用引号包起来，最后一个属性写完后不加逗号

#### 函数

- 函数也是对象，它具有普通对象具有的所有功能
    
- 新建函数对象：

    语法：

    ```javascript
            var func = new Function("需要执行的代码块");
            //在构造函数中可以加入字符串参数代码使得调用函数时可以直接执行
            var func = new Function("console.log(111)");
            func();
            //结果为111
    ```

- 函数声明：

    语法：

    ```javascript
        function foo(param1,param2, param3...) {
            语句...
        }
    ```

- 函数表达式:

    语法：

    ```javascript
        var func = function([param1,param2, param3...]) {
            语句...
        }
    ```
                
###### 形参与实参

- 声明函数时传递的参数叫形参，作用相当于在函数内部声明变量
- 调用函数时传递的参数叫实参，作用相当于给函数的形参赋值
- 当实参数量大于形参时，多出来的实参会被忽略
- 当实参数量小于形参时，未赋值的形参会是undefined类型
- 实参可以是任意数据类型包括对象与函数，当实参数量过多时，可以考虑将部分实参封装成一个对象传入

    将函数当做实参传入另一个函数：
    ```javascript
        function func1(){
            console.log("a");
            return "1";
        }
        function func2(a){
            console.log(a);
        }
        func2(func1);       //结果为func1对象本身，结果为func1函数的内容
        func2(func1());     //结果为func1的函数返回值,结果为1
    ```

###### 返回值

- 使用return语句可以让函数返回特定的值，此时函数中return后面跟的所有语句都不执行

    语法：
    ```javascript
        return [返回值];

        function test(a, b){
            return a+b;
        }
        var result = test(1, 2);
        console.log(result);
        //结果为3
    ```

- 当return后不加参数时，相当于函数返回undefined
- 函数中不使用return时，也相当于返回undefined
- 返回值可以是任意类型，包括对象和函数

    ```javascript
        function func1(){
            function func2(){
                console.log("func2");
            }
            return func2;
        }
        var a = func1();
        console.log(a);
        //结果为func2函数本身的内容
        console.log(a());       //与console.log(func1()());相同
        //结果为"func2"
    ```

###### 立即执行函数

- 语法：

    ```javascript
    (function([形参1, 形参2...]) {
            语句...
        })([实参1, 实参2...])
    ```

- 当写成以下形式时，js会将前半部分当成代码块，无法识别函数声明
        
    ```javascript
    function([形参1, 形参2...]){
            语句...
        }([实参1, 实参2...])
    ```

###### 方法

- 由于对象的属性可以是任何值，因此也可以将一个函数赋值给一个对象的属性，此时这个函数属性就被叫做方法，需要注意的是，函数与方法只是名称上的不同，其他没有任何区别

- 调用对象中的函数，被称为调用这个对象的方法

    ```javascript
        var obj = new Object();
        obj.name = "111";
        obj.callName = function(){
            console.log(obj.name);
        }
        obj.callName();
        //结果为"111"
    ```

###### 枚举语句

- 语法：
    
    ```javascript
    for(var 声明变量 in 对象){
            语句...
        }
        for(var i in obj){
            console.log(i);     //i为obj对象的属性名
            console.log(obj[i]);        //obj[i]为obj对象的属性值
        }
    ```
- 对象中有多少个属性，这个循环便会执行多少次

###### 作用域

- 全局作用域
    - 直接写在script标签中的代码都属于全局作用域，它在打开页面时创建，在关闭页面时销毁
        全局作用域中所有的变量可以在页面的任意部分被访问到
        
    ```javascript
        var a = 1;
        function func1(){
            console.log(a);
        }
        //结果为1
    ```

    - 全局作用域中所有声明的变量都会被创建成window对象的属性

    ```javascript
        var a = 1;
        console.log(window.a);
        //结果为1
    ```

    - 变量的提前声明

        当使用var来声明或者声明并赋值变量时，无论声明位置在何处，声明本身这个语句会在当前script标签中的最顶端被执行
    ```javascript
    console.log(a);
    var a = 1;
    //结果为undefined而不是报错，因为var a;这条语句已经在代码最顶端被执行过了
    console.log(a);
    a = 1;
    //报错，a未被声明
    ```

    - 函数的提前声明

        无论函数在何处被声明，函数声明本身会在任何代码前被执行

        ```javascript
        func1();
        function func1(){
            console.log("1");
        }
        //结果为"1"
        ```

        但是对于函数表达式来说，由于使用var来声明函数，因此只符合变量提前声明的特性

        ```javascript
        func1();
        var func1 = function(){
            console.log("1");
        }
        //结果报错，undefined不是一个函数
        ```

- 函数作用域

    函数作用域在函数执行时创建，在执行完毕后销毁，在函数作用域内部与全局作用域相似

    当在函数中使用变量时会先向当前作用域查找，没有则向上一级作用域查找，直到全局作用域，如果全局作用域中没有此变量时报错

    ```javascript
    var a = 1;
    function func1(){
        function func2(){
            console.log(a);
        }
        func2();
    }
    func1();
    //结果为1        
    ```
    在函数作用域中可以访问到全局作用域的变量，反之不成立
    ```javascript
    function func1(){
        var a = 1;
    }
    console.log(a);
    //结果为报错，a未定义
    ```
    在函数作用域中变量声明提前与函数声明提前同样适用（当函数有形参时相当于在函数内部声明了变量）
    ```javascript
    function func1(){
        console.log(a);
        var a = 1;
    }
    func1();
    //结果为undefined
    function func1(){
        var a = 1;
        func2();
        function func2(){
            console.log(a);
        }
    }
    //结果为1
    ```

###### this的值

- 当调用函数时，解析器会隐式传入一个参数this，它是一个对象

    1. 当以函数的形式调用时，this永远是全局作用域window
    2. 当以对象的方法调用时，this是调用这个方法的对象
    3. 当以构造函数调用时，this就是新创建的对象

- 作用：
    ```javascript
    var name = 1, obj1 = {name: 2, sayName: func}, obj2 = {name:3, sayName: func};
    func(){
        console.log(this.name);
    }
    func();
    obj1.sayName();
    obj2.sayName();
    //结果为1, 2, 3
    //可以使用this来使方法/函数内的值发生变化
    ```