---
title: 理解MongoDB的mapReduce使用及流程
date: 2016-10-18 10:39:11
tags: [mongodb]
---

* mapReduce 是一个多层分组查询的方法(这是个人理解的)
* 可以分为四层
* 第一层: query 层  // 意思是把数据进行筛选
* 第二层: map 方法层  // 分组数据处理, 把数据作为 key values 的形式传进 reduce 层
* 第三层: reduce 方法层  // 统计函数, 把传进来的 key values 转换成想要的 key value 形式
* 第四层: finalize 方法层  // 最后的确认函数
* [Map-Reduce 例子](http://docs.mongoing.com/manual-zh/tutorial/map-reduce-examples.html)

<!--more-->

```js
db.collection.mapReduce(
	// 第二步: 执行 map 函数, 必须有 emit 方法
	function () {
		// emit 方法遍历 collection 中所有的记录
		emit(this.user_name, 1); // -> {key: mark, values: [1,1,1,1]} {key: runoob, values: 1}
	},
	// 第三步: 执行 reduce 函数, 遍历 map 函数得出的对象数组
	function (key, values) { // -> [{key: mark, valuse: [1,1,1,1]}, {key: runoob, values: 1}]
		// return 的数据 为需要存储的数据 -> {value: {value1: 4, value2: 4}}
		return {value1: values.length, value2: Array.sum(values)};
	},

	// 第一步: collection 和 条件 相关
	{
		out: 'post_total', // { merge: "collection" } 如果有 collection 会重写 数据, 可 merge
		query: {status: 'active'},
		// 最后确定的方法
		finalize: function (key, value) {

		}
	}
)
```