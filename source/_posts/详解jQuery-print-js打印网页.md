---
title: 详解jQuery.print.js打印网页.
date: 2017-03-28 12:05:53
tags: [jquery.js, jquery.print.js]
---

jQuery.print.js 是基于jQuery的网页打印插件, 可以打印全网页, 也可以打印页面某一部分.

[GitHub](https://github.com/DoersGuild/jQuery.print)

使用很简单

<!--more-->

html代码:
```html
<!DOCTYPE html>
<html lang="zh_cn">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<div id="print">
		<p>123</p>
		<p>456</p>
		<p>789</p>
	</div>
	<script src="http://libs.baidu.com/jquery/1.11.1/jquery.min.js"></script>
	<script src="js/jQuery.print.js"></script>
	<script>
		$('#print').print()
	</script>

</body>
</html>
```

参数列表:

##### 1. globalStyles
* 说明: 是否包含父元素的样式
* 参数类型: Boolean
* 默认: true

##### 2. mediaPrint
* 说明: 是否应包括带有media='print'的链接标签
* 参数类型: Boolean
* 默认: false

##### 3. stylesheet
* 说明: 添加一个外部的css样式文件
* 参数类型: URL-string
* 默认: null

##### 4. noPrintSelector
* 说明: 跳过打印的标签, 元素带有class="no-print"时跳过打印.
* 参数类型: Any valid `jQuery-selector`
* 默认: ".no-print"

##### 5. iframe
* 说明: 是否从iframe中打印, 可以使用自己定义的iframe元素, 注意: 设置为false时, chrome会拦截.
* 参数类型: Any valid jQuery-selector or Boolean
* 默认: true, 如果iframe参数无效, 则创建一个隐藏的iframe

##### 6. append/prepend
* 说明: 添加选中的html元素或自定义标签, prepend(在前面添加), append(在后面添加)
* 参数类型: Any valid `jQuery-selector` or HTML-text
* 默认: null

##### 7. title
* 说明: 修改打印标题, 默认html文档标题
* 参数类型: string
* 默认: null






