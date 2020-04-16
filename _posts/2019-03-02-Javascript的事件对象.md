---
layout:     post
title:      Javascript的事件对象
subtitle:   介绍原生事件与自定义事件
date:       2019-03-02
author:     QY
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - Javascript
    - 基础知识
    - 事件对象
---
# 前言

>如果说DOM操作是JS与HTML交互的基础，那么事件就是动态交互的核心技术了，这一篇来介绍一下事件对象和自定义事件吧~
>
>在开始前先来回顾一下什么是响应函数
>
>#### 响应函数
>
>- 当用户与浏览器进行任何交互时都会产生相应的事件
>
>- JS可以通过给事件绑定函数来使事件触发时执行被绑定的函数,这个函数就叫做响应函数


#### 绑定事件的简单介绍

给一个节点绑定响应函数有两种方法, 这两种方法分别被写进W3C的DOM1与DOM2的规范中

- DOM1方法

```javascript
window.onclick = function(){}
```

- DOM2方法

```javascript
document.addEventListener('click', function(){})
```

#### 事件对象

- 当给某个元素对象绑定响应函数后，浏览器在执行这个响应函数后会向这个函数自动传递一个实参，该实参封装了与此事件有关的任何数据

- 可以给响应函数添加形参来调用这个事件对象

    ```javascript
    var div = document.getElementByTagName("div")[0];
    div.onclick = function(event){}
    ```

- IE8及以下浏览器不会传递此实参，因此会无法获取到

- IE与chrome浏览器也会将这个实参保存在`window.event`中，可以用来兼容IE8及以下浏览器

    ```javascript
    var div = document.getElementByTagName("div")[0];
    div.onclick = function(event){
        event = event || window.event;
    }
    ```

- 浏览器的滚动条在chrome中被认为是`documentElement`的`scrollTop`和`scrollLeft`的值，而在IE与firefox被认为是`body`的`scrollTop`和`scrollLeft`的值

    因此可以使用一下代码来兼容三种浏览器

    ```javascript
    var scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
    var scrollLeft = document.body.scrollLeft || document.documentElement.scrollLeft;
    ```

- 常用属性：

    1. clientX

        返回触发此事件时鼠标在视图中的X坐标

    2. clientY

        返回触发此事件时鼠标在视图中的Y坐标
        
    3. pageX

        返回触发此事件时鼠标在触发此事件的元素中的X坐标，仅支持IE8以上及其他浏览器

    4. pageY

        返回触发此事件时鼠标在触发此事件的元素中的Y坐标，仅支持IE8以上及其他浏览器


IE浏览器的浏览器默认行为不能使用`return false`来阻止某些浏览器默认行为如文字拖拽
在IE浏览器中可以设置`obj.setCapture`来让所有的操作都被设置的obj捕获，其他对象就不会执行任何响应函数

同样可以设置`document.releaseCapture`来取消事件捕获,这个可以与其他浏览器的在响应函数中`return false`一起使用来达到取消浏览器默认行为的作用

`return false`与`event.returnValue=false`作用相同

对于其他默认行为由于IE不支持`event.preventDefault`，因此可以使用一下代码做到兼容所有浏览器
    
```javascript
event.preventDefault && event.preventDefault();
return false;
```


当事件由`addEventListener`绑定时，不能使用`return false`取消默认行为而需要`event.preventDefault`，对于不支持此方法的IE，使用`event.returnValue=false`

###### 事件冒泡

- 当某个元素的事件触发时，相同的事件会一层层向这个元素的父元素传递，如果不希望这个元素向外进行事件冒泡有两种方法来取消冒泡

- 是否传递是根据dom树结构来决定而不是页面上是否包裹，只要是父元素，不管是否与子元素重叠都会将子元素的事件冒泡给父元素

1. `event.cancelBubble=true`

    不属于W3C标准，但是更方便，新版本的chrome与firefox都已经支持

2. `event.stopPropogation()`

    W3C标准，但是IE不支持

- 示例代码：

    ```html
    <html>
        <head>
                <style>
                    #outer{
                        width: 500px;
                        height: 500px;
                        padding: 10px;
                        border: 20px solid gray;
                        background-color: blue;
                        position:relative;
                    }
                    #inner{
                        width: 400px;
                        height: 400px;
                        padding: 10px;
                        border: 20px solid rgb(0, 0, 0);
                        background-color: green;
                        position:relative;
                    }
                    #content{
                        width: 300px;
                        height: 300px;
                        padding: 10px;
                        border: 20px solid red;
                        background-color: yellow;
                    }
                </style>
        </head>
        <body>
            <div id="outer">
                <div id="inner">
                    <div id="content">

                    </div>
                </div>
            </div>
        </body>
        <script>
            window.onload=function(){
                var outer = document.getElementById("outer");
                var inner = document.getElementById("inner");
                var content = document.getElementById("content");
                content.onclick = function(event){
                    alert("content");
                };
                inner.onclick = function(event){
                    alert("inner");
                    event.cancelBubble = true;
                };
                outer.onclick = function(event){
                    alert("outer");
                };
            }
            
        </script>
    </html>
    ```

    结果为弹出两个框分别为`content`与`inner`

###### 事件委派

- 当在给节点添加新节点并且需要让这个新节点与其他兄弟节点有同样的响应函数时可以使用事件委派的功能

- 当事件触发时响应函数的形参属性`event.target`会指定实际触发此函数的节点

- 将响应函数绑定到父元素上，并且使用响应函数的形参的属性进行节点判断可以完成新增子节点不需要重新绑定响应函数的功能

```html
<html>
    <head>
        <script>
            window.onload = function(){
                var ul = document.getElementByTagName("ul")[0];
                as[i].onclick = function(event){
                    if(event.target.className == "a"){
                        alert("我是a");
                    }
                };
            }
        </script>

    </head>
    <body>
        <ul>
        <li><a class="a" href="javascript:;">1</a></li>
        <li><a class="a" href="javascript:;">2</a></li>
        <li><a class="a" href="javascript:;">3</a></li>
        <li><a class="a" href="javascript:;">4</a></li>
        <li><a class="a" href="javascript:;">5</a></li>
        </ul>
    </body>
</html>
```

###### 事件绑定

- 当对同一个节点的事件重复绑定响应函数时后绑定的函数会覆盖前绑定的函数导致前绑定的函数不会被执行

```javascript
a.onclick = function(){
    console.log(1);
};
a.onclick = function(){
    console.log(2);
};
//结果为2
```

使用`addEventListener`来为节点添加监听函数，响应函数可以绑定多次，并且会按顺序执行,支持IE8以上或者其他的浏览器，响应函数内部的`this`是节点对象

- 解绑使用`removeEventListener`
    - 参数：

        事件名，不带on

        响应函数

        布尔值，决定事件是否在捕获阶段执行，默认为`false`，在冒泡阶段执行

    - 语法：

        *对象.addEventListener("", 响应函数, false/true)*

想要兼容IE8及以下浏览器时使用`attachEvent`，同样可以绑定多次，但是顺序为后绑定先执行，响应函数内部的`this`是`window`

- 解绑使用`detatchEvent`
    - 参数：

        事件名，带on

        响应函数

    - 语法：

        *对象.addEventListener("", 响应函数)*

- 可以定义一个`bind`函数来兼容两者

    - 参数：

        需要绑定响应函数的节点对象

        事件名，不带on

        响应函数

    ```javascript
    function bind(obj, eventStr, func){
        obj.addEventListener ? obj.addEventListener(eventStr, func) : obj.attachEvent("on" + eventStr, function(){func.call(obj)});
    }
    ```

###### 事件传播

浏览器在实现时对于事件传播微软与网景有不同的理解方式

--微软认为事件由子元素向父元素传播

--网景认为事件由父元素向子元素传播

W3C标准实现时参考两种方法给出了事件执行的阶段：

- 捕获阶段

    事件先从父元素向子元素传递

- 目标阶段

    捕获阶段完成后事件在目标阶段执行

- 冒泡阶段

    然后事件再从子元素向父元素传递

当需要设置某个事件在捕获阶段执行时，将`addEventListener()`的第三个参数设置为`true`即可，IE8及以下浏览器没有实现捕获阶段所以没有办法设置

###### 需要兼容的事件

1. onmousewheel

    当鼠标滚轮滑动时触发此事件，事件结果由`event.wheelDelta`返回向上滚时返回120，向下滚时返回-120，可以只关注正负，同时不被firefox支持

    想在firefox中监听此事件需要使用`addEventListener("DOMMouseScroll", func)`来监听鼠标滚轮的操作，事件结果由`event.detail`返回，向上滚动时返回-3，向下滚动时返回3
    
    当浏览器中有滚动条时鼠标滚动操作会默认滚动滚动条，在IE与chrome中可以使用`return false`来取消默认行为，firefox需要使用`preventDefault`来取消

    ```javascript
    obj.onmousewheel = function(event){
        event = event || window.event;
        if(event.wheelDelta > 0 || event.detail <0){
            //向上滚动
        }else{
            //向下滚动
        }
    };
    test.addEventListener && test.addEventListener("DOMMouseScroll", test.onmousewheel, false);
    ```

2. 键盘事件

    只有可以获取焦点的事件或者让`document`绑定键盘事件才可以触发

    1. onkeydown

        当一直按着时，事件会被连续触发，当连续触发的时候，第一次和第二次间隔会稍微长一些后面的才会快速触发，为了防止误操作

        在文本框键入字母属于文本框的默认行为

        - 事件属性：

            1. event.altKey
            2. event.ctrlKey
            3. event.shiftKey

        当这三个键被按下时它们的值会被赋值为true否则为false

    2. mouseenter    鼠标移入

    3. mouseleave    鼠标移出

    4. mouseover      鼠标移入

    5. mouseout      鼠标移出

    6. contextmenu       上下文菜单事件，可以通过阻止默认行为来自定义上下文菜单

    7. beforeunload     通常在切换页面时触发

    8. DOMContextLoad    在DOM树渲染完成后触发，不管CSS，JS文件是否渲染完毕

    以上四个事件触发机制: 当鼠标移动至绑定监听器的元素或者子元素时就会被触发

    区别: 前两者不会冒泡后两者会

###### 事件模拟

可以运行在普通浏览器, 还包括IE10及以上

API:

1. `document.createEvent(eventname)`

    - 传入一个字符串当做参数，这个字符串在DOM2与DOM3标准中有不同的形式，DOM3中将复数变成单数，建议不要再使用DOM2的标准

        1. UIEvents 一般化的UI事件，鼠标与键盘事件都继承自此事件，在DOM3中是UIEvent
        2. MouseEvents 鼠标事件，在DOM3中是MouseEvent
        3. MutationEvents   一般化的DOM变动事件，在DOM3中是MutationEvent
        4. HTMLEvents    一般化的HTML事件，没有对应的DOM3事件

    在DOM2中没有键盘事件，但是在DOM3中实现了`KeyboardEvent`

    返回值是一个对应的`event`对象，根据不同的事件子类有不同的初始化方法
    
    以`MouseEvent`为例，初始化方法为`initMouseEvent`，传入参数与事件类型有关，详细内容需要参考mdn文档

    在DOM3中还定义了自定义事件`CustomEvent`，返回的对象通过`initCustomEvent`初始化

2. `element.dispatchEvent(event)`

    传入一个事件对象

    用来触发事件对象定义的事件

运行在IE9及以下

API:

1. `document.createEventObject()`

    没有参数
    返回值是一个`event`对象，`event`的所有属性与方法都需要自定义，没有预设参数

2. `element.fireEvent(eventname, event)`
    第一个参数是事件名，如`onclick`等
    第二个参数是自定义的`event`对象
