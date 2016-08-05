---
title: 原生javascript如何解决ajax跨域的问题？
date: 2016-08-05 10:53:03
tags: [javascript, ajax]
---

大家知道 jQuery 解决 ajax 跨域的问题很方便，直接用 jsonp 的方式提交就 ok ，网上有很多解决的办法，但是有些时候我们不得不用原生 js 来写很多东西，所以我今天把原生 js 解决 ajax 跨域的问题用一个很简单的方法来实现。

<!--more-->

> 首先来理解它的原理: 实际上实现跨域不是用的传统的 XMLHttpRequest 对象，而是用了 script 标签可以访问其它的网址的 js 来实现的。

直接上代码

```js
function jsonp(options) {
    options = options || {};
    if (!options.url || !options.callback) {
        throw new Error("参数不合法");
    }
    // 创建 script 标签并加入到页面中
    var callbackName = ('jsonpcallback_' + Math.random()).replace(".", ""); // 随机一个比较长的回调函数名称
    var oHead = document.getElementsByTagName('head')[0];
    options.data[options.callback] = callbackName; // 把回调函数名字加入到 options.data 中
    var params = formatParams(options.data); // 格式化数据
    var oS = document.createElement('script');

    oHead.appendChild(oS);
    // 创建jsonp回调函数
    window[callbackName] = function (json) {
        oHead.removeChild(oS);
        clearTimeout(oS.timer);
        window[callbackName] = null;
        options.success && options.success(json);
    };
    // 发送请求
    oS.src = options.url + '?' + params;
    // 超时处理
    if (options.time) {
        oS.timer = setTimeout(function () {
            window[callbackName] = null;
            oHead.removeChild(oS);
            options.error && options.error({ message: "超时" });
        }, options.time);
    }
}
```

formatParams 方法

```js
function formatParams(data) {
    var arr = [];
    for (var name in data) {
        arr.push(encodeURIComponent(name) + "=" + encodeURIComponent(data[name]));
    }
    arr.push(("v=" + Math.random()).replace(".", ""));
    return arr.join("&");
}
```
执行

```js
jsonp({
    url: "http://www.XXX.com/",
    callback: "callback", // callback 的名称要记住
    time: 5000,
    data: {
        a: 1
    },
    success: function (data) {
        console.log("提交成功!");
    },
    error: function (err) {
        console.log(err);
    }
});
```

可以在浏览器中看到，我们向服务器端发送的数据是 GET 请求，数据有两个，一个是参数 `a: 1` 另一个是 `callback: jsonpcallback_***`.

为什么会有两个参数？

其实我们没有发送数据请求， 我们只是创建了一个 `script` 标签， 连接到一个 js 文件，来执行这个 js 文件罢了。

所以就要后台返回的值需要是一段执行的 js 方法， 方法名称已经用 `callback` 参数发送过去了，只需要把参数执行就可以了.

后台返回的示例（以 `callback: jsonpcallback_***` 为例）， `json` 为后台返回的数据，执行这个方法就可以执行 `success` 中写的方法了

```js
jsonpcallback_***(json)
```
