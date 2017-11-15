---
title: 理解说明Servlet生命周期.
date: 2017-11-15 11:08:27
tags: [java, servlet]
---

理解 Servlet 的生命周期

<!-- more -->

Servlet 有三个阶段:
1. 初始化: 
  当容器启动或者第一次执行, Servlet#init(ServletConfig) 方法被执行, 初始化当前 Servlet.
2. 处理请求: 
  当 HTTP 请求到达容器时, Servlet#service(ServletRequest, ServletResponse) 方法被执行, 来处理请求.
  > 其中 ServletRequest 是请求对象(输入), ServletResponse 是响应对象(输出)
3. 销毁:
  当容器关闭时, 容器会调用Servlet#destory方法被执行, 销毁当前 Servlet.

