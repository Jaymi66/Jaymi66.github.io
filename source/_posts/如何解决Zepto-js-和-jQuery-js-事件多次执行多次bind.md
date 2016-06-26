---
title: 如何解决Zepto.js 和 jQuery.js 事件多次执行多次bind?
date: 2016-06-26 17:51:48
tags: [jQuery.js, Zepto.js]
---

* Zepto.js 和 jQuery.js 绑定click事件的时候,如果多次执行监听事件,会出现累积bind同一个事件的问题。
* 这时候执行该事件会根据绑定的次数来多次执行该事件，如何解决这个问题？

<!--more-->


* Zepto.js 和 jQuery.js 绑定click事件的时候,如果多次执行监听事件,会出现累积bind同一个事件的问题。
* 这时候执行该事件会根据绑定的次数来多次执行该事件，如何解决这个问题？
* 如何让事件多次执行监听而只bind一次。

```
$(‘.class’).unbind(‘click’).on(‘click’, function(){ … });
```

* 可以通过 先解绑 click 事件，然后再 bind click 事件来解决。
