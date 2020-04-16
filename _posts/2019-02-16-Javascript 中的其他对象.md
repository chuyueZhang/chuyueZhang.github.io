---
layout:     post
title:      Javascript 中的全局对象
subtitle:   需要了解的常用全局对象及其方法
date:       2019-02-16
author:     QY
header-img: img/post-bg-desk.jpg
catalog: true
tags:
    - Javascript
    - 基础知识
    - 内建对象
---
# 前言

>在对象与函数基础篇中已经介绍了创建对象与函数的相关方法，现在我们来看一下js都有哪些常用的内建对象吧，当然js中远不止这些对象，如果想深入学习建议可以参考&laquo;Javascript高级程序设计&raquo;哦~

#### 数组

- 数组也是一个对象，与对象的区别在于数组只能通过索引来查找值，并且存储效率要比普通对象更高，所以具有所有对象具有的特性

    **创建数组**

    ```javascript
    `var arr = new Array();`
    ```

    arr具有三个对象属性，`constructor`(构造函数的引用), `length`(数组长度), `prototype`(原型对象的引用)

    可以直接以索引的形式向数组最后一位添加内容

    ```
    数组[数组.length] = 值;
    ```

    可以通过修改数组长度来删除一些数据

    ```javascript
    arr[0] = 0;
    arr[1] = 1;
    arr[2] = 2;
    arr[3] = 3;
    arr.length = 3;
    console.log(arr[3]);
    //结果为undefined
    ```

    当访问数组中不存在的数据时，会返回`undefined`而不是报错

    可以使用数组字面量方便地创建数组

    ```javascript
    //语法：[]
    var arr = [];
    ```


    在数组字面量中可以直接加入参数来给数组赋值

    ```javascript
    var arr = [0, 1, 2, 3];
    console.log(arr);
    //结果为0,1,2,3     
    ```

    同样也能向数组类添加参数来直接给数组赋值

    ```javascript
    var arr1 = new Array(0, 1, 2, 3);
    console.log(arr1);
    //结果为0,1,2,3
    ```

    但是当参数只有一个时，结果会有所不同

    ```javascript
    var arr = [10];         //创建一个第一个值为10的数组
    var arr1 = new Array(10);       //创建一个长度为10的数组
    console.log(arr);
    console.log(arr1);
    //结果分别为10和,,,,,,,,,
    ```

    **数组常用方法**
    1. `push()`

        - 向数组最后添加一个或多个元素并将数组长度返回

    2. `pop()`

        - 删除数组最后一个元素并将该元素返回

    3. `unshift()`

        - 向数组开头添加一个或多个元素并将数组长度返回

    4. `shift()`

        - 删除数组开头的元素并将该元素返回

    5. `forEach()`

        - 按顺序遍历整个数组
        - 支持IE8以上或者其他的浏览器
        - 由自己创建但不由自己调用的函数称为回调函数
        - 语法：

            *数组.forEach(function(value, index, arr){});*
        
        它会在回调函数中以实参的形式传递3个参数：

        1. 当前循环中的元素
        2. 当前循环中的索引
        3. 调用forEach函数的数组

    6. `slice()`

        - 从一个数组中截取特定范围的元素并将这些元素以数组的形式返回，不改变原数组
        - 语法：

            ```
            数组.slice(start, end);
            ```

        - 第一个参数是截取开始的索引，返回数组会包括开始索引的元素
        - 第二个参数是截取结束的索引，返回数组不会包括结束索引的元素

            ```javascript
            var arr = [0,1,2,3,4,5];
            var arr1 = arr.slice(0,4);
            console.log(arr1);
            //结果为0,1,2,3
            ```

        - 参数可以是负值，如果为负就是从后往前计数

            ```javascript
            var arr = [0,1,2,3,4,5];
            var arr1 = arr.slice(-3,-1);
            console.log(arr1);
            //结果为3,4
            ```

    7. `splice()`

        - 删除或者添加元素，直接改变原数组,返回值为删除的元素
        - 语法：

            ```
            数组.splice(start, number[,元素1, 元素2...]);
            ```

        - 第一个参数为从哪个索引开始删除元素
        - 第二个参数为删除几个元素
        - 从第三个参数开始的参数都是是在第一个参数的索引之前添加这些元素

            ```javascript
            var arr = [0,1,2,3,4,5];
            arr.splice(0,1,7,8,9);
            console.log(arr);
            //结果为7,8,9,1,2,3,4,5
            ```

    8. `concat()`

        - 可以将两个或者多个数组连接成一个数组
        - 不会改变原数组
        - 语法：

            *数组.concat(任意数据类型[，任意数据类型...]);*

            ```javascript
            
            var arr = [1,2,3,4];
            var result = arr.concat([5,6,7,8],1,"a", false, null, undefined, {});
            console.log(result);
            //结果为1,2,3,4,5,6,7,8,1,"a",false,null,undefined,{}
            ```
            
    9. `join()`

        - 将数组中的元素转换成字符串, 可以添加参数指定元素之间的连接符, 无参数时默认为逗号, 不会改变原数组
        - 语法：

            *数组.join([字符串]);*

            ```javascript
            var arr = [1,2,3,4];
            var result = arr.join(" ");
            console.log(result);
            //结果为"1 2 3 4"
            ```

    10. `reverse()`

        - 调换数组中元素的排列顺序
        - 会修改原数组，并且修改后的数组与返回值相同
        - 语法：

            *数组.reverse();*

            ```javascript
            var arr = [1,2,3,4];
            var result = arr.reverse()
            console.log(result);
            console.log(arr);
            //结果都为4,3,2,1
            ```

    11. `sort()`

        - 给数组中的元素排序，默认以unicode编码顺序排列，因此直接对数组中的数字排序会产生预料外的结果
        - 可以传递一个回调函数作为`sort`的参数，回调函数中有两个形参分别表示数组中一前一后的两个元素，具体是哪两个元素需要根据循环确认
        - 函数的返回值决定是否交换这个两个元素，当返回值大于0时交换，小于0时不交换，等于0时认为两个值相等不交换
        - 会直接修改原数组的元素，与方法的返回值相同
        - 语法：

            *数组.sort([回调函数]);*

            ```javascript
            var arr = [5,3,6,767,34,2];
            arr.sort();
            console.log(arr);
            //结果为2,3,34,5,6,767
            arr.sort(function(a, b){
                return a-b;
            });
            console.log(arr);
            //结果为2,3,5,6,34,767
            ```

#### 函数对象

- `call`和`apply`
    
    - 函数对象都具有这两个方法，可以向这两个方法中的第一个参数传入一个对象用来修改这个方法的`this`对象
    - 如果函数对象需要形参

        `call`方法中第二个参数开始依次输入要传递给函数对象的实参

        将需要传递的实参封装成一个数组作为`apply`的第二个参数将实参传递给函数对象

    - 语法：

        *函数.call(对象[,参数1...]);*

        *函数.apply(对象[,数组]);*

        ```javascript
        var obj = {};
        function a(a){
            console.log("a = " + a);
            console.log(this);
        }
        a(1);
        a.call(obj, 1);
        a.apply(obj, [1]);
        //结果为
        //    window,1
        //    object,1
        //    object,1
        ```

- `arguments`

    - 在调用函数时，浏览器还会隐式传递一个参数`arguments`，它是一个类数组对象，不是数组对象
    - 使用索引来查询调用函数时传入的参数
    - 拥有`length`属性来表示传入参数的数量
    - 拥有`callee`属性表示当前指向的函数引用，可以用来编写递归函数
    ```javascript
    function a(){
        console.log(arguments.length);
        console.log(arguments[0]);
        console.log(arguments[1]);
        console.log(arguments.callee);
    }
    a(1,2);
    //结果为2,1,2,a函数本身
    ```

#### global对象

- ECMAScript中非常特别的对象，理论上存在但是又无法获取
- 不属于其他任何对象的属性或者对象都是这个对象的属性和方法
- 也就是说所有在全局作用域中定义的属性与对象可以说都是这个对象的属性和方法
- ECMAScript没有指出如何访问`global`对象，但是浏览器会将这个对象当作`window`的一部分加以实现
- 常用方法：

    1. `encodeURL()`

        用来将整个url编码成浏览器能够识别的字符串，即将一些特殊字符进行编码，如：空格->%20，url中合法特殊字符不会被编码

    2. `encodeURLComponent()`

        用来将某一段url编码成浏览器能够识别的字符串，url中合法特殊字符也会被编码

    3. `decodeURL()`

        `encodeURL`的反向操作，规则与之相同

    4. `decodeURLComponent()`

        `encodeURLComponent`的反向操作，规则与之相同


#### Date对象

- 用来操作与时间有关的对象

```javascript
var date = new Date();      //实例化Date对象的值就是执行这行代码时的时间
var date = new Date("12/11/2016 0:0:0");  //当输入参数时以参数对应的含义输出时间，按照mm/dd/yyyy h:m:s的格式输入
```

- 常用方法：

    1. `getDate()`
    
        获取时间的日

    2. `getDay()`

        获取时间的星期，值为0-6，从周日计算
        
    3. `getMonth()`

        获取时间的月份，值为0-11，从1月计算
        
    4. `getFullYear()`

        获取时间的年份
        
    5. `getTime()`

        获取时间的时间戳，为格林尼治标准时间1970/1/1 0:0:0开始到特定世界为止经过的毫秒


    6. `now()`     
    
        执行这行代码时的时间戳，可用来计算一段代码的性能


#### Math对象

- Math对象与其他内建对象不同，它不是构造函数而是一个工具类，不需要实例化，直接使用
- 语法：

    *Math.方法();*

- 常用属性：

    E 自然对数

    PI 圆周率

- 常用方法：
    1. `ceil()`

        向上取整

    2. `floor()`

        向下取整

    3. `round()`

        四舍五入

    4. `random()`

        获取0-1之间的随机数
        - 当想获取x-y之间的随机数时有公式Math.random()*(y-x)+x
        
    5. `max()`

        获取多个数中的最大值
        
    6. `min()`

        获取多个数中的最小值
        
    7. `sqrt()`

        对某个数开根号
        
    8. `pow(x, y)`

        求x的y次幂


#### 包装类

- js提供了3个包装类将3种基本数据类型转换成基本数据类型对象，但是在日常开发中不要使用这种方式，因为在转换后进行比较时会产生预期外的结果

    1. String

        将基本数据类型string转换成string对象

        ```javascript
        var str = new String("a");
        console.log(typeof str);
        //结果为object
        ```

    2. Number

        将基本数据类型number转换成number对象
            
        ```javascript
        var num = new Number(1);
        console.log(typeof num);
        //结果为object
        ```

    3. Boolean

        将基本数据类型boolean转换成boolean对象

        ```javascript
        var bool = new Boolean(true);
        console.log(typeof bool);
        //结果为object
        ```

- 当对这三种基本数据类型操作包装类中的方法或者属性时，浏览器会临时创建一个基本数据类型对象再调用这些方法，然后再将其转换成基本数据类型

    ```javascript
        var a = 1;
        a.toString();
        console.log(typeof a);
        //结果为string

        //执行这两行代码时不会报错
        //因为实际在对临时创建的基本数据类型对象进行属性赋值操作
        //语句执行完毕后临时对象就被销毁，因此第二次执行时结果为undefined

        a.name = "a";
        console.log(a.name);
        //结果为undefined   
    ```

#### String对象

- string的底层是用字符数组进行表示的，因此许多数组可以使用的方法在字符串中同样可以使用
        
    ```javascript
    var str = "abcdef";
    console.log(str[0]);
    //结果为"a"
    ```

- 常用属性:

    length 字符串的长度

- 常用方法：(全都不改变原字符串)

    1. `charAt()`

        返回指定位置的字符，与用索引表示相同
        
    2. `charCodeAt()`

        返回指定位置的字符unicode编码
        
    3. `fromCharCode()`

        返回指定unicode编码对应的字符
        
    4. `concat()`

        连接多个字符串，与使用加号+连接字符串相同
        
    5. `indexOf()`

        返回指定字符在字符串中第一次出现的索引，当没有时返回-1

        - 参数：

            第一个参数：需要查找的字符

            第二个参数：从第几个索引开始进行查找
            
    6. `lastIndexOf()`

        与indexOf相似，但是它返回的是字符在字符串中最后一次出现的索引，没有时返回-1

        - 参数：

            第一个参数：需要查找的字符

            第二个参数：从第几个索引开始进行查找
            
    7. `toUpperCase()`

        将所有字符转化成大写
        
    8. `toLowerCase()`

        将所有字符转化成大写
        
    9. `slice()`
    
        截取指定范围内的字符串，与数组中的slice相似

        - 参数：

            第一个参数：索引开始处，包括这个索引

            第二个参数：索引结束处，不包括这个索引

        - 参数可以是负数，当为负数时从字符串后往前数
        - 不输入第二个参数时，截取第一个参数开始至后面全部的字符串

            ```javascript
            var str = "abcdef";
            var result = str.slice(0,-2);
            console.log(result);
            //结果为"abcd"
            ```
        
    10. `subString()`

        与slice相似，不同在于参数为负数时默认输入是0，当第一个参数大于第二个参数时会交换两个参数的位置

        ```javascript
        var str = "abcdef";
        var result = str.subString(1, -3);
        //结果为"a"
        ```
            
    11. `subStr()`

        同样为截取一段字符串，与前两个方法的区别在于参数不同，这个方法不是ES标准，不推荐使用

        - 参数：

            第一个参数：索引开始处，包括这个索引

            第二个参数：需要截取的字符数量
            
    12. `split()`

        与数组的join方法相反。将字符串以特定方式转换成数组
        - 参数：

            1. 第一个参数：将字符串以哪个字符进行拆分充当新数组的元素

                当参数为空字符串时，将每个字符都拆分成一个元素存入新数组

                ```javascript
                var str = "abtcdtef";
                var result = str.split("t");
                console.log(result);
                结果为ab,cd,ef
                ```

            2. 第二个参数：决定返回的数组长度

#### 正则表达式对象

- 用来匹配字符串是否满足要求       
- 语法：
    - 匹配模式

        "i" ----忽略大小写

        "g" ----全局匹配

    ```javascript
    var reg = new RegExp("正则表达式", "匹配模式");
    ```
    

- 方法：

    1. `test()`

        测试指定字符串是否满足正则的匹配要求，满足返回true，否则返回false


- 正则表达式字面量

    - 语法：

        *var reg = /正则表达式/匹配模式;*

    - 使用字面量更方便，使用构造函数更灵活，因为可以使用变量

- 正则表达式语法：

    1. `|`

        表示或 

        ```javascript
        /a|b/.test("ac"); //true
        ```

    2. `[ ]`

        与`|`含义相同

        ```javascript
        /[ab]/.test("bc"); //true
        ```

    4. `.`

        表示任意字符

    5. `\`

        表示转义，将特殊符号转化成字符

    6. `*`

        表示0个或多个字符
        
    7. `+`

        表示1个或多个字符
        
    8. `^`

        表示开头的某个字符
        
    9. `$`

        表示结尾的某个字符
        
    10. `\w`

        表示任意字母数字及`_`
        
    11. `\W`

        除了任意字母数字及`_`
        
    12. `\d`

        表示任意数字
        
    13. `\D`

        除了任意数字
        
    14. `\s`

        表示空格
        
    15. `\S`

        除了空格
        
    16. `\b`

        表示单词分隔符
        
    17. `\B`

        除了单词分隔符
        
    18. `{}`

        限制字符出现的次数

        ```javascript
        /^a{3,5}$/.test("aaaaaa");    //false
        ```
        
    19. `()`

        将几个字符当做整体

        ```javascript
        /^(ab){2,3}$/.test("abababab"); //false
        ```

- 常用语法：

    `[a-z]`  小写字母

    `[A-Z]`   大写字母

    `[A-z]`   英文字母
    
    `[0-9]`   数字

- 可以使用正则的字符串方法：

    1. `split()`

        将字符串以正则表达式作为间断符号拆分成数组

        ```javascript
        var str = "1a2s3d4f5g6h7jk8";
        console.log(str.split(/[1-9]/));
        //结果为a,s,d,f,g,h,jk
        ```
        
        正则不设置匹配模式为g也会全局拆分

    2. `search()`

        查询正则表达式中的字符位置，不存在则返回-1，与indexOf相似

        ```javascript
        var str = "1a2s3d4f5g6h7jk8";
        console.log(str.search(/[a-z]/));
        //结果为0
        ```
            
        正则设置匹配模式为g也不会全局匹配

    3. `match()`

        返回满足正则表达式匹配的字符

        ```javascript
        var str = "1a2s3d4f5g6h7jk8";
        console.log(str.match(/[a-z]/));
        //结果为a
        ```

        如果需要返回全局匹配的结果需要设置匹配模式为g，返回为数组

        ```javascript
        console.log(str.match(/[a-z]/g));
        结果为a,s,d,f,g,h,j,k
        ```

    4. `replace()`

        将正则表达式匹配到的字符串用新字符串代替

        - 参数：

            第一个参数：需要被替换的原字符串，可以用正则表达式

            第二个参数：需要替换的新字符串，可以是空串

            ```javascript
            var str = "1a2s3d4f5g6h7jk8";
            console.log(str.replace(/[a-z]/, "!"));
            //结果为"1!2s3d4f5g6h7jk8"
            ```
        如果需要返回全局匹配的结果需要设置匹配模式为g

        ```javascript
        console.log(str.replace(/[a-z]/g, "!"));
        //结果为"1!2!3!4!5!6!7!!8"
        ```