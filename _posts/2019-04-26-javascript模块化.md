---
layout:     post
title:      javascript模块化
subtitle:   模块化的演变
date:       2019-04-26
author:     QY
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Javascript
    - 进阶知识
    - 模块化
---

# 前言

> javascript本身没有模块的概念, 但当项目足够庞大时, 变量很可能会互相污染导致不必要的后期维护, 模块化则是在一步步更轻松解决这个问题的环境下诞生的

### 引入模块化

- 原始写法

    * 直接将需要声明的变量写在全局中

    ```javascript
    function foo1(){}
    function foo2(){}
    foo1()
    foo2()
    ```

    * 特点
      
        污染全局

- namespace方式

    * 将需要声明的变量封装到一个函数中

    ```javascript
    let module = {
        foo1(){},
        foo2(){}
    }
    module.foo1()
    module.foo2()
    ```
    * 特点
      
        本质是对象，可以随时获取内部数据，不安全

- IIFE(立即调用函数)方式

    * 通过闭包的方式，只暴露想要暴露的数据或方法

    ```javascript
    let Module = (function module(){
        var __private = 'save now'
        function foo(){}
        return {foo: foo}
    })()
    Module.foo()
    Module.__private    //undefined
    ```

    * 特点
      
        函数是唯一的local scope

- 引入依赖

    * 通过给闭包函数引入参数的形式来传递依赖

    ```javascript
    let Module = (function module($){
        var __private = 'save now'
        $('#test').hide()
        function foo(){}
        return {foo: foo}
    })($)
    ```

    * 这就是模块模式, 即现代模块化的基石

### 模块化的理念

- 为什么要模块化

    1. 现代网页更像一个web app

    2. 代码复杂度逐渐上升

    3. 需要高解耦性的js文件

    4. 需要部署一种最大优化http数量的网站

- 模块化的好处

    1. 避免命名冲突

    2. 更好的分离，按需加载

    3. 更高复用性

    4. 高可维护性

- 通过页面引入script的方式

    1. 提高了http数量，降低网站性能

    2. 引入顺序改变会导致报错   

    3. 依赖关系模糊

    4. 难以维护

### 模块化规范

- commonjs

    * 每个文件都可以当做一个模块, 反映为文件名就是模块名

    * 常用于服务器端, 因为文件存储在本地, 同步加载不影响响应速度且nodeJS默认支持此规范

    * 如果用于浏览器端需要提前编译打包

    * 属于运行时加载的模块化, 本质还是对变量的一种赋值, 即值拷贝

    - 基于服务器端

        * 文件目录：

        ```
        project
          |modules
          |node_modules
          |app.js
          |package.json
        ```

        * 基本语法

          1. `module.exports`
              
              当前模块对外的接口, 当其他模块引入此模块时就是引入了这个对象

          2. `exports`
            
              本质是对`module.exports`引用地址的拷贝, 因此当被赋值过后会失去与之的关联

          3. `require()`

              用来引入其他模块

        ```javascript
        var value = 0
        var func = function() {
          console.log(value)
        }
        var final = 1
        exports.value = value
        console.log(module.exports.value) //0
        module.exports = {value, func}
        console.log(exports.func) //Undefined
        exports = {value, func, final}
        console.log(module.exports.final) //Undefined
        ```

    - 基于浏览器端

        * 文件目录：

        ```
        project
          |js
            |dist         打包后的文件目录
            |src          源码所在目录
              |modules
              |app.js
          |node_modules
          |index.html
          |package.json
        ```

        * 使用browerify打包
          ```
          * npm install -g browerify
          * npm install --save-dev browerify
          * browerify src.js -o destination.js
          ```

- AMD

    * 异步模块定义

    * 专门用于浏览器端, 模块的加载是异步的

    - 基本语法

        * 定义没有依赖的模块

        ```javascript
        define(function(){})
        ```

        * 定义有依赖的模块

        ```javascript
        define([module1, module2], function(module1, module2){    //显式声明，依赖注入
            return {}       //将想要暴露的模块对象直接返回
        })  
        ```

        * 引入并使用模块

        ```javascript
        require([module1, module2], function(module1, module2){})
        ```

    - 使用

        - 项目目录：

            ```
            js
              |libs
                |require.js
                |angular.js
                |jQuery.js
              |modules
                |module1.js
                |module2.js
              |main.js
            index.html
            ```

        1. 引入[requirejs](http://requirejs.org)

              ```html
              <script data-main="./js/main.js" src="./js/libs/require.js"></script>
              <script src="./js/main.js"></script>
              ```

        1. 在main.js中使用requirejs来构建模块与文件的关系

            ```javascript
            (function(){
                requirejs.config({
                      baseUrl: 'js/',     //设置根目录
                      paths:{
                          module1: 'modules/module1', //后缀js会自动添加，所以不能手动加.js后缀
                          module2: 'modules/module2',
                          jQuery: 'libs/jQuery'
                      },
                      shim:{
                          angular:{
                              exports: 'angular'  //对于原生不支持AMD的第三方模块, 如果要将第三方对象暴露给window对象的话, 需要在此手动配置暴露的对象名
                          }
                      }
                  })
                  requirejs(['module1', 'module2', 'jQuery', 'angular'], (m1, m2, jQuery, angular)=>{})
              })()
              ```

### ES6模块化规范

* 属于静态关系的模块化定义, 引入值时是引用地址的拷贝

* 依赖模块需要编译打包处理

- 文档结构

    ```
    project
      |js
        |src
        |build
        |dist
      |index.html
      |.babelrc
    ```

- 编译过程

1. 安装babel-cli babel-reset-es2015 browerify

    ```
    npm install -g babel-cli browerify
    npm install --save-dev babel-reset-es2015
    ```

2. 在项目根目录自定义.babelrc文件

   ```json
   {
       "reset": ["es2015"]
   }
   ```

3. 编写es6模块化语法

      - 常规暴露

          * 在src文件夹新建module1.js

              ```javascript
              function foo(){
                  console.log('我是module1')
              }
              export foo     //单独暴露
              /*
              *export {foo}    //统一暴露
              */
              ```

          * 在src文件夹新建main.js

              ```javascript
              import foo from './module1.js'  //单独暴露时
              /*
              *export {foo} from './module1.js'   //统一暴露时
              */
              foo()   //打印'我是module1'
              ```

      - 默认暴露

          * 在src文件夹新建module1.js

              ```javascript
              function foo(){
                  console.log('我是module1')
              }

              export default foo         //引入时可以是声明成任意变量来引入暴露出去的数据
              ```

          * 在src文件夹新建main.js

              ```javascript
              import a from './module1.js'

              a()   //'我是module1'
              ```

4. 在项目根目录使用babel将ES6语法转换成ES5语法

    ```
    babel ./src -d ./build
    ```

5. 在项目根目录使用browerify将ES5语法中的commonjs转换成普通ES5语法

    ```
    browerify ./build/main.js -o ./dist/bundle.js
    ```

6. 在index.html中引入最终编译好的js文件

    ```html
    <script src="./js/dist/bundle.js"></script>
    ```

### commonJS与ES6模块化的区别

- 在上文已经提到过, 主要具有两大区别:

    1. commonJS在引入变量时是值拷贝, 而ES6模块化是引用拷贝
    
    2. commonJS是在运行时进行加载, ES6模块化是编译时的静态定义

- 通过代码结果能够证实这种差异

    - commonJS

        ```javascript
        //module1
        var value = 3
        var func = function() {
          console.log(value)
        }

        module.exports = {value, func}

        //module2
        var module1 = require('./module1')

        console.log(module1.value)    //3
        module1.value++
        module1.func()    //3
        ```

    - ES6模块化

        ```javascript
        //module1
        var value = 3
        var func = function() {
          console.log(value)
        }

        export {value, func}

        //module2
        import {value, func} from 'module1'

        console.log(value)    //3
        value++
        func()    //4
        ```