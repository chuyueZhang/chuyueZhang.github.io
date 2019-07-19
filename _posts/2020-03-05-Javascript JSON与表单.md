---
layout:     post
title:      Javascript JSON与表单
subtitle:   浅谈JSON以及表单的属性与方法
date:       2019-03-05
author:     QY
header-img: img/post-bg-github-cup.jpg
catalog: true
tags:
    - Javascript
    - 基础知识
    - DOM
    - 表单
---
# 前言

>表单是DOM中最常用的元素节点，而JSON又是浏览器通过表单向服务器发送消息的最常见的方法，因此有必要单独拿出来讲解


#### 表单脚本
- 表单对应的是`HTMLFormElement`类型，继承自`HTMLElement`，并且有一些额外的属性和方法

    `acceptCharset`: 服务器能够处理的字符集，等价于HTML中的`accpet-charset`特性

    `action`：接受请求的url，等同于表单的action

    `elements`：表单中所有控件的集合(`HTMLCollection`)

    `enctype`: 请求的编码类型，等同于HTML中的`enctype`

    `length`：表单中控件的数量

    `method`：等价于HTML中的方法，通常是get或post

    `name`：表单的名称

    `reset()`:将所有表单域重置为默认值

    `submit()`:提交表单

    `target`：用于发送请求和接收相应的窗口名称，等价于HTML中的`target`

可以通过常规的DOM操作来获取表单元素，也可以通过`document.forms`来获取一个页面中所有表单的`HTMLCollection`

- 提交表单
 
    当表单提交时会触发submit事件

- 重置表单

    让表单重置时会触发reset事件，在体验上最好不要主动触发重置事件，会严重影响用户体验，必要时使用回退到上一个页面代替

- 表单字段
  
    可以通过常规的DOM操作来获取表单内控件，也可以使用表单内属性elements返回一个表单控件的集合

    这个集合是一个`HTMLFormControlsCollection`，它继承自`HTMLCollection`并重新封装了`namedItem`方法

    当通过控件的`name`来访问时，如`document.forms[0].elements['color']`, 它的返回值是一个`NodeList`

    也可以通过访问表单的属性来访问，如`document.forms[0]['color']`，但这是向后兼容的方式，最好不要使用

- 表单控件的共有属性

    - 除了`fieldset`以外所有表单控件都公用一组属性
 
        disabled    布尔值，表示当前控件是否被禁用

        form    表示指向当前表单的指针

        name    表示当前控件的名称

        readOnly    布尔值，表示是否只读

        tabIndex    表示tab键切换的序号

        type    表示控件类型，在`input`中就是`type`指定的类型，在`button`中默认是`submit`，在单选列表中是`select-one`，在多选列表中是`select-mltiple`

        value   表示将会提交给服务器的值


- 表单控件的共有方法

    - 每个表单控件都拥有两个方法

        focus   使当前控件获取焦点

        blur    使当前控件失去焦点

    - 当对隐藏的控件调用这些方法时会抛出错误

- 表单控件的共有事件

    - 除了支持鼠标，键盘，更改，HTML事件外所有控件都支持三个事件

        change  对于`input`与`textarea`来说，失去焦点且值改变时触发，对`select`来说，切换就触发

        blur    与change事件的先后顺序不定，不能提前假定某个事件一定先触发

        focus   获得焦点时触发

###### 文本框脚本

- 在表单中有两种文本框`text`与`textarea`，两者特性大致上是相同的，但是仍然有些重要的区别
    `text`可以设置`maxlength`来指定文本框最大能够输入的字符数量，size指定能够显示的最大字符数量即文本框大小，`value`指定文本初始值
    `textarea`的文本初始值只能在`<textarea></textarea>`之间设定，文本框大小通过`rows`和`cols`来设置，不能设置最多能接受的字符数量

- 对于这两种文本框都最好使用`value`值来设定文本内容而不要使用标准DOM方法:

    用`setAttribute`设置`input`的`value`
    获取第一个子节点修改`textarea`的`value`

- 两个文本框都支持`select`方法，可以用来全选文本

- 相对应的两个文本框都会触发`select`事件，除了IE8及以下的浏览器会在选中并释放鼠标触发，IE在选中时就触发

- HTML5中还支持部分选中文本的api: `setSelectionRange(start, end)`接收两个参数表示偏移量

- 在IE8及以下想要部分选中需要使用文本范围的技术

- 剪切板操作
    - clipboardData对象

        在IE中是`window`属性，其他浏览器中是`event`的属性并且只有触发剪切板相关事件才能访问

    - 剪切板事件

        beforecopy

        copy

        beforepaste

        paste

        beforecut

        cut

        **其中before相关的事件在IE中只要显示了文本相关的上下文菜单都会被触发，在其他浏览器中是在copy,paste,cut等事件触发前触发*

    - 剪切板方法

        1. `setData()`

            第一个参数在普通浏览器中是MIME类型但在IE是text或URL

            第二个参数是想要复制进剪切板的内容

        2. `getData()`

            第一个参数在普通浏览器中是MIME类型但在IE是`text`或`URL`，但是对普通浏览器使用`text`也会被识别成`text/plain`

        3. `clearData()`
 
            清除剪切板中的数据


###### 选择框脚本

- 通过`select`元素创建的是选择框，除了表单字段中列举的属性和方法它还具有以下属性和方法：

    `add(new, ref)`   向控件中插入new，位置在ref之前

    `multiple`        布尔值，表示表单是否允许多选

    `options`         控件中所有`option`的`HTMLCollection`

    `remove`          移除给定位置的选项

    `selectedIndex`   可读写的number类型，基于0的选中项的索引，没有选中项则为-1，对于支持多选的只保留第一个选中项的索引，被赋值是指定项被选中其他项会取消选中

    `size`            选择框中可见的行数，与等价于HTML中的`size`属性

- 每个`option`都有以下属性:

    `index`           当前选项在`option`中的索引

    `label`           当前选项的`label`

    `selected`        可读写布尔值，当前选项是否被选中，对单选

    `text`            文本节点的内容

    `value`         `value`值

#### JSON
- 全称为javascript object notation
- JSON是一个特定格式的字符串，几乎所有编程语言都能识别
- 能够转换成每个编程语言的对象，主要用于数据交互
- JSON与js对象字面量格式相同但是属性和值必须加引号
- JSON分类：

    对象

    数组

- JSON中可以使用的值：

    数字

    字符串(当JSON没有使用对象自面量格式而是简单字符串格式时不能使用)

    布尔值

    null

    对象

    数组

    ```javascript
    JSON.parse("1")     //1
    JSON.parse("true")  //true
    JSON.parse("null")  //null
    JSON.parse("{}")    //{}
    JSON.parse("[]")    //[]
    ```

- JSON ---> js对象

    `JSON.parse()`

    使用`JSON`对象作为参数，返回Javascript对象

    - 第一个参数是JSON字符串
    - 第二个参数是还原函数，有两个形参分别是key跟value，对于每个key，函数返回值就是最终结果的每个键值对中的值

    *兼容IE7及以下使用eval来代替不能使用的JSON.parse(),或者引入JSON2库*

    `eval()`

    如果执行的字符串中有`{}`，则会被当做代码块，如果不希望被当做代码块被解析，在{}前后使用`()`
    由于效率与安全问题，尽量不要使用

- js对象 ---> JSON

    `JSON.stringify()`

    使用Javascript对象作为参数，返回`JSON`

    - 第一个参数是一个`JSON`对象
    - 第二个参数是个过滤数组或者函数，函数的两个形参是key, value, 对于每个key，函数返回值就是最终键值对的值
    - 第三个参数是个数字，表示缩进的空格数

- `toJSON()`

    每个对象都可以创建`toJSON`方法，在对这个对象使用`JSON.stringify`会先调用这个方法获取的返回值再对此进行操作

- `JSON.stringify`的处理流程

    如果要真正弄清楚如何将JS对象转化成指定的JSON字符串需要了解`JSON.stringify`的处理流程

    1. 如果第一个参数有`toJSON`方法就先调用，否则不调用
    2. 将toJSON的返回值或者第一个参数当做下一步的输入值
    3. 如果有第二个参数就用来过滤上一步的输出值，如果没有就将结果当做下一步的输入值
    4. 如果有第三个参数则按照这个值格式化上一步的输出值没有直接将最后的值返回


