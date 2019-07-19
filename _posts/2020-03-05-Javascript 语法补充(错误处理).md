---
layout:     post
title:      Javascript 语法补充(错误处理)
subtitle:   简要说说JS中的错误处理对象与方式
date:       2019-03-05
author:     QY
header-img: img/post-bg-debug.jpg
catalog: true
tags:
    - Javascript
    - 基础知识
    - 错误处理
---
# 前言

>虽然这一篇文章讲的属于JS中的基础语法，但是少了它似乎也不会影响代码的运行
>
>但是对于一个追求精致，立志于让程序在任何情况下都不会轻易奔溃的程序员，错误处理机制还是必不可少的~
>
>这篇只是对JS基础语法的补充所以就不会很长咯

#### 错误处理机制

###### try-catch语句

```javascript
try{

}catch(err){

}finally{

}
```

`finally`子语句是可选的，且必定执行，无论`catch`语句有没有执行

当有`finally`存在时，`try`与`catch`中的`return`会失效

- 错误类型

    1. `Error`

        基本错误类型，其他错误类型都继承自该类型

    2. `EvalError`

        当没有把eval当做函数调用时会抛出`EvalError`错误

    3. `RangeError`

        当超出数组范围或者值不合法时会抛出`RangeError`

    4. `ReferrenceError`

        当对象找不到时会抛出`ReferenceError`

    5. `SyntaxError`

        当eval中执行的代码有语法错误时会抛出`SyntaxError`，如果在外部有语法错误会直接停止执行一般不会抛出此错误

    6. `TypeError`

        当实际变量类型与预期不一致时会抛出`TypeError`, 如让一个新创建的普通对象调用数组中的`forEach`方法时

    7. `URLError`

        当使用`decodeURL`和`encodeURL`中的url格式错误时会抛出`URLError`
    
- 与`try-catch`相配合的还有`throw`操作符

    当遇到`throw`操作符时代码会停止运行，只有当`try-catch`捕获到时才会继续运行
    抛出错误类型的实例时能够更加真实的模拟浏览器错误

###### 自定义错误

通过原型链继承可以自定义错误类型，只要继承自`error`的对象都会被浏览器当做错误对象处理

当自定义错误时需要为新错误类型指定`name`与`message`属性

```javascript
try {
    var udError = new Error();
    udError.name = "undefined Error";
    udError.message = "throw undefined Error";
    throw udError;
} catch(e) {
    console.log(e)  //undefined Error: throw undefined Error
}
```

