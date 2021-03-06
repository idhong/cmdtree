---
title: JavaScript BOM（浏览器对象模型）总结
date: 2017-08-26T10:40:00+00:00
category: JavaScript
---

JavaScript有三大组成部分，即`ECMAScript`、`BOM（Browser Object Model浏览器对象模型）`、`DOM（Document Object Model，文档对象模型）`，而对于Web应用，BOM无疑是真正的核心。BOM提供了一系列的对象用来访问浏览器的功能，这些功能与用DOM操作的任何网页内容无关。

![](/pics/2017/08/2501.jpg)

## Window 对象

Window对象是BOM的核心，它表示浏览器的一个实例。在网页中定义的一切对象、变量或者是函数，都以window作为其`Glbal`对象，而由于window对象同时扮演着ECMAScript中Global对象的角色，因此在全局作用域中声明的变量或者函数，都会变成window对象的属性或者方法。

```js
var name = "fanzhiyang";
function satName() {
    alert(this.age);
}

alert(window.name); // fanzhiyang
sayName();      // fanzhiyang
window.sayName();   // fanzhiyang
```

因为直接声明的全局变量有[[Configurable]]特性，这个特性的值被设置为false，所以这样的全局变量（Window属性）不可以通过delete操作符删除。


```js
var name = "fanzhiyang";
window.windowName = "fanzhiyang";

delete window.name; // flase
delete window.windowName;   // true
```

尝试访问未声明的变量会抛错，但是通过查询window对象，可以知道某个未声明的变量是否存在。

```js
var newValue = oldValue; // 出错，oldValue未定义

var newValue = window.oldValue; // 返回undefined，因为这是一次属性查询
```

### iframe

如果页面中包含框架，那么每个框架都有自己的window对象，且这些框架保存在`iframes`集合中，`window.iframes`就表示该页面所有框架的集合，我们可以通过索引来访问相应的window对象，也可以通过框架的名称（window.name）来访问，例如

```
window.frames["frameName"]; 
window.frames.frameName 
window.frames[index]
```

### 窗口位置

我们有很多的方式来获取和修改窗体的位置，IE，Safari，Opera和Chrome都提供了`screenLeft`和`screenTop`属性，分别表示window的相对于屏幕左边和上边的位置。但是Firefox提供的两个对应的属性为`screenX`和`screenY`，Safari和Chrome同时支持这两个属性。


我们可以通过判断`screenLeft`和`screenTop`两个属性是否存在，从而确定使用这两个属性或者（在Firefox中）使用`screenX`和`screenY`来取得窗口的位置。

```js
var leftPos = (typeof window.screenLeft == "number") ? 
    window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ? 
    window.screenTop : window.screenY;
```


### 间歇调用与超时调用

我们可以使用`setTimeout()`和`setInterval()`两个方法来设置超时调用和间歇调用两种功能。

**setTimeout()**

setTimeout()方法接受两个参数：要执行的代码和以毫秒计的时间。

例如：

```js
setTimeout(function() { 
    alert("Hello world!"); 
}, 1000);
```

setTimeout()执行玩后会返回一个数值ID，表示超时调用。我们可以这样把他取消掉：

```js
cleaarTimeout(timeoutId);
```

**setInterval()**

setInterval()方法接受的参数与setTimeout()相同：要执行的代码和以毫秒计的时间。 

例子：

```js
setInterval(function() { 
    alert("Hello world!"); 
}, 1000);       
```

调用setInterval()方法也会返回一个ID，用来取消间歇调用。

```js
clearInterval(intervalId);

```

## location 对象

`location`对象提供了当前URL的信息。

一些常用属性及说明。

|  属性名 | 说明  |
| ------------ | ------------ |
| hash    | 设置或返回从井号 (#) 开始的 URL（锚）。|
| host    | 设置或返回主机名和当前 URL 的端口号。|
| hostname |    设置或返回当前 URL 的主机名。|
| href    | 设置或返回完整的 URL。|
| pathname |     设置或返回当前 URL 的路径部分。|
| port |     设置或返回当前 URL 的端口号。|
| protocol |    设置或返回当前 URL 的协议。|
| search |     设置或返回从问号 (?) 开始的 URL（查询部分）。|


### 查询字符串参数

这是一个非常使用广泛的一个传递参数的方法——通过URL来传递参数，虽然可以通过`location.search`来返回从问号 (?) 开始到URL末尾的内容，但是没有办法逐个返回某个要查询的字符串参数，所以我们可以使用如下代码来具体返回一个要查询的字符串参数：

```js
function getQuery(name){
    var reg=new RegExp("(^|&)"+name+"=([^&]*)(&|$)");
    var r=window.location.search.substr(1).match(reg);
    if(r!=null)
        return unescape(r[2]);
    return null;
}

alert(getQuery("ie"));   // UTF-8

```

## navigator 对象

`navigator` 对象是一个查询浏览器客户端信息的方法。


属性及说明：


![](/pics/2017/08/2601.png)
![](/pics/2017/08/2602.png)


## screen 对象

`screen` 对象主要用来查询浏览器的一些像素颜色等信息，用处不大。

## history 对象

`history`对象顾名思义，保存着用户上网的历史记录。

### go() 方法

使用`go()`方法可以在用户的历史浏览中进行跳转，可以向前也可以向后，这个方法接受一个参数，表示要跳转的整数页数（负数表示向后跳转）。

```
history.go(-1); // 后退一页
history.go(1);  // 前进一页

history.go(2);  // 前进两页
```

> 参考 ： 《JavaScript高级程序设计（第3版）》
