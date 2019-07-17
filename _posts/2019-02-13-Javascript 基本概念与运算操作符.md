---
layout:     post
title:      Javascript 基本概念与运算操作符
subtitle:   了解Javascript核心语法
date:       2019-02-13
author:     QY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Javascript
    - 基础知识
---

# 前言

**Javascript最重要也是最基础的就是那些运算操作符了，这些内容是必须时刻掌握一点都不能忘**

#### js输出

```javascript
document.write()   //向body中写字符串
console.log()      //向控制台输出
alert()            //弹出警告框输出
```

#### js编写位置

* 外联文件
 ```HTML
     <script src="引入的文件位置"></script>
 ```
* 内联文件
 ```HTML
     <script type="text/javascript">
         js代码编写的位置
     </script>
 ```
* 内嵌代码(结构与行为耦合，不使用)
 ```HTML
     <input type="button" onclick="alert('弹出')"></input>
     <a href="javascript:alert('弹出');"></a>
 ```

#### js基本语法

* 严格区分大小写
* 语句分号结尾
* 没有添加分号时浏览器自动添加，但是消耗资源并且可能添加出错

#### js字面量和变量

* 字面量即为常量
* 变量可被字面量赋值
* 变量声明和赋值可分开或一起
```javascript
        var a;
        a = 1;
    //或者
        var a = 1;
```

#### js标识符

* 所有可以自定义的变量都叫做标识符，并且遵循以下规范
    * 只能以字母数字，下划线，$构成
    * 不能以数字开头
    * 不能使用ES的关键字和保留字
    * 一般使用驼峰命名法
* 标识符以unicode编码表示，因此可以使用UTF-8的所有内容，但是一般只使用英文

#### js基本数据类型

* `string`
* `number`
    * 可以表示整数与浮点数
    * 2进制浮点数以分数表示，不准确
    * NaN与Infinity是数值的字面量，表示非数与无穷
    * typeof 参数  检测某个值的类型
* `boolean`
    * 只有两个值：`true  false`
* `null`
```javascript
//表示一个空对象
         var a = null;
         console.log(typeof a);
//结果为object
```
* `undefined`
```javascript
//已经声明的变量未赋值则成为undefined
        var a;
        console.log(typeof a);
//结果为undefined
```

#### 强制类型转换

* `string`
    * 两种办法：`a.toString()`或者`String()`
      * toString只能用于对象，因此null和undefined无法调用
      * String()对于Number, String, Boolean来说会调用底层的toString()方法，对于null和undefined会直接进行转换
* `number`
    * 三种方法：`Number()`, `parseInt()`, `parseFloat()`
        * `Number()`：
            *    对于字符串来说如果只包含数字，直接转换成数字，如果包含非数字转换成NaN，如果是""或者"  "则转换成0
            *    对于boolean值，true转换成1，false转换成0
            *    对于null，转换成0
            *    对于undefined，转换成NaN
        * `parseInt()`：
            *    首先将所有内容转换成字符串再开始解析。
            *    从左到右依次解析，需要非整数直接舍去，第一位非整数返回NaN
        * `parseFloat()`：
            *    与parseInt()相似，只是遇到第一位小数点不会忽略会转换成小数，其余与之相同
* `boolean`
    * 一种方法：`Boolean()`
        *    对于数字：只有0跟NaN会转换成false
        *    对于字符串：只有""会转换成false
        *    对于null和undefined，只会转换成false

#### 进制

* 16进制在数字前加`0x`
* 8进制在数字前加`0`
* 在某些浏览器中键入以下代码：
```javascript
  var a = "070";
  console.log(parseInt(a));
  //结果会输出56。因为浏览器将其当做8进制，解决方法是输入第二个参数，强制以10进制输出
  parseInt(a, 10);
```
* 2进制在数字前加`0b`
    * 某些浏览器无法解析2进制如IE浏览器，同时也不常用

#### 运算符

> 运算符有以下种类：`typeof，+，-，*，/，%`所有的运算符都不改变原始变量而是返回进行运算后的结果,并且NaN与任何值进行运算结果都为NaN


* `typeof`
```javascript
            //typeof返回一个变量或者字面量的类型，返回值为string
            var a = typeof 2;
            console.log(a);
            console.log(typeof a);
            //结果为：number与string
```
* `+`
    * 数字的加法运算
    * 遇到非number的值，会将其转换成number
    * 遇到string的值，会转换成string然后进行接串操作，可应用于长字符串的换行与隐性string类型转换
* 其余运算符进行相应数学运算并且在遇到非number值时，会全部转换成number值后再进行运算操作，此特性可用于隐式number类型转换，但还有更简单的方法
```javascript
            var a = "123";
            console.log(a / 1);
            console.log(typeof (a / 1));
           // 结果为123和number类型
```

#### 一元运算符

* 正号`+`和负号`-`
    * 两者能够进行相应的数学运算同时在遇到非number的值时会将其强制转换成number值再进行运算，此特性可用于隐式number类型转换
```javascript
            var a = "123";
            console.log(+a);
            console.log(typeof +a);
            //结果为123和number类型
```

#### 自增与自减

* `++`
    * 分为`(++a)`和`(a++)`，对于a值来说都是增加1，但是表达式的返回值不同，前者返回新值，后者返回原值
* `--`
    * 特性与自增相同，只是对于a值来说是减少1

#### 逻辑运算符

> 包括`!`, `&&`, `||`三种运算符

* `!`：两次取非会得到原值的布尔值，可以利用这个特性进行隐式布尔值转换

```javascript
            var a = "123";
            console.log("a = " + !!a);
            //结果为true，与Boolean(a)相同
```

* `&&`：两个值都为true结果才为true
* `||`：两个值都为false结果才为false
    * 在JS中`&&`与`||`都是属于短路操作，即当一个值满足要求时才会继续执行第二个操作，第一个值不满足要求时不执行第二个操作
    * 当参数不是boolean值时先会将参数转换成boolean值后再按照以上规则输出原值

```javascript
//&&：
            //当第一个值为true时，返回第二个值
            //当第一个值为false时，返回第一个值
            console.log("123" && "456");
            //结果为"456"
            console.log(NaN && "111");
            //结果为NaN
//||：
            //当第一个值为false时，返回第二个值
            //当第一个值为true时，返回第一个值
            console.log(NaN || "111");
            //结果为"111"
            console.log("123" || "456");
            //结果为"123"
```

#### 赋值运算符

* 有以下几种：`=, +=, -=, /=, *=`
```javascript
    var a += 3;
    var a = a + 3;
```
* 两者等价，对于其他的赋值运算符，与+=规则相同

#### 关系运算符

* 有以下几种：`<, >, <=, >=`
```javascript
//有一方为number值时，将非number值转换成number值再进行比较
//NaN与任何值进行任何比较结果都为false， 包括NaN本身
        console.log(NaN >= NaN);
        //结果为false
//当两方都为string时，按位比较字符编码，因此在两者都为string类型值为数字时进行比较，结果可能不符合预期，可应用于英文名字的排序
        console.log("11" > "2");
        //结果为false

```

#### unicode编码

* 在js中使用时在编码前加`\u`对编码进行转义输出
```javascript
        console.log("\u0031");
        //结果为1，编码为16进制
```
* 在HTML中使用时以 &#编码; 的格式输出
```HTML
        &#0048;
<!-- 结果为1，编码为10进制 -->
```

#### 相等运算符

* 包括`==, !=, ===, !==`
  * `==, !=`
    * 两者类型相同时判断是否相等，类型不同时进行类型转换再判断是否相等，转换成哪种类型无法确定
    * 由于undefined衍生于null，因此两者相等
  
        `console.log(undefined == null) //true`

    * NaN与任何值进行运算结果都为false，包括自己

        `console.log(NaN == NaN) //false`

  * `===, !==`
    * 除不进行类型转换外，规则与`==`, `!==`类似
    * 当两者类型不同时，`===`直接返回false，`!==`直接返回true
    * NaN的规则在此处同样适用
  
        `console.log(NaN === NaN) //false`

    * undefined与null在这种运算符下，才会不相等
  
        `console.log(undefined === null) //false`

#### 三元运算符

* 语法格式为`(表达式)?(语句1):(语句2)`
    * 当表达式结果为true时执行语句1，否则执行语句2
    * 当表达式结果为非boolean值时，会转换成boolean值后再对表达式进行判断

#### 逗号运算符

* 用来分割不同语句，可以同时声明多个变量

#### 运算符优先级

* 按照优先级表进行先后运算，可以使用()改变优先级

#### 代码块

* 将多个语句用`{}`包含起来，这一堆语句称为代码块，它只具有分组的作用，没有其他用途
```javascript
        {
            var a = 1;
            console.log("a");
        }
            console.log("a");
        //结果为1 /n 1
```

#### 条件判断语句

```javascript
// 
//     语法1：满足条件只执行紧接着的第一条语句，后面语句与判断语句无关
//         if(表达式)
//             语句1;
//     语法2：满足条件执行代码块中的代码，代码块外的代码与判断语句无关
//         if(表达式){
//             语句1;
//             语句2;
//         } 
//     语法3：满足条件执行前一个代码块中的代码，否则执行后一个代码块中的代码
//         if(表达式){
//             语句...
//         }else{
//             语句...
//         }
//     语法4：从上到下依次判断，当满足一个表达式后后面的代码不再执行
//         if(表达式){
//             语句...
//         }else if{
//             语句...
//         }else if{
//             语句...
//         }else if{
//             语句...
//         }else{
//             语句...
//         }
//     语法5：条件判断语句可以嵌套
//         if(表达式){
//             语句...
//         }else{
//             语句...
//             if(表达式){
//                 语句...
//             }
//         }
//         
```

#### 条件分支语句

```javascript
// 
//     语法：
//         switch(表达式){
//             case 值1: 
//                 语句...
//                 break;
//             case 值2: 
//                 语句...
//                 break;
//             case 值3: 
//                 语句...
//                 break;
//             default:
//                 语句...
//                 break;
//         }
//     break语句用来跳出switch语句，当没有此语句时，如果表达式的值满足某个case时，后面的语句都会执行
//         var a = 1;
//         switch(a){
//             case 1:
//                 console.log("1");
//             case 2:
//                 console.log("2");
//             default:
//                 console.log("其他");
//         }
//         结果为1, 2, 其他
// 
```

#### while循环

```javascript
// 
//     while语句：
//         语法1：
//             while(true){    //1.将表达式写死
//                 if(条件表达式){  //2.当满足某种条件时退出
//                     break;
//                 }
//                 语句...
//             }
//         语法2：
//             var i = 0;     //1.初始化变量
//             while(i < 10){  //2.当不满足条件时退出循环
//                 语句...
//                 i++;        //3.更新变量
//             }
//     do{}while()语句：
//         语法：
//             do{
//                 语句...
//             }while(表达式)
//         与while语句的唯一不同是，while语句先对表达式进行判断，当不满足条件时不继续执行后面的代码块，而do{}while()语句是先执行do后面的代码块再判断表达式，不满足时不执行第二次
//             var i = 11;
//             while(i <= 10){
//                 console.log(i);
//             }
//             结果为无
//             do{
//                 console.log(i);
//             }while(i <= 10)
//             结果为11
// 
```

#### for循环

```javascript
// 
//     语法：
//         for(初始化变量; 条件表达式; 更新变量){

//         }
//     作用与while循环相同，写法也可与while循环语法2相同，只要初始化变量与更新变量分别写在for循环外部与内部
//         var a = 0;
//         for(; a< 10;){
//             a++;
//         }
//     当for循环表达式不写任何内容时则成为死循环
//         for(;;){

//         }
// 
```
#### break和continue

* `break` 用来跳出循环或者switch
    * 用在循环语句时跳出最近的循环，不能跳出if语句
* `continue` 用来跳过当前循环，执行下一次循环
    * 不能跳出if语句
* `label`：在循环语句的前一行使用label语句用来标识某一个循环
* 两者都可增加参数，参数是循环的标识名，可以跳出所指定的循环
```javascript
good:
        for(var i=1; i<10; i++){
            console.log(i);
            for(var j=1; j<10; j++){
                break good;       
            }
        }
        //结果只有一个1
    bad:
        for(var i=1; i<10; i++){
            console.log(i);
            for(var j=1; j<10; j++){
                if(i==2){
                    continue bad;       
                }
            }
        }
        //结果为1,3,4,5,6,7,8,9
```

#### 优化程序效率

* 可以考虑在循环语句中增加`break`来提高程序运行效率
* 使用`console.time("字符串标志")`来标记一个计时器
* 使用`console.timeEnd("字符串标志")`来结束某个计时器并打印出计时器经过的时间
```javascript
    console.time("a");
    console.timeEnd("a");
//结果是名为a的计时器经过的时间，单位是ms，只能用在浏览器中
```
