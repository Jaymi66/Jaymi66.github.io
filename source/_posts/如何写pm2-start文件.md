---
title: 如何写pm2 start文件
date: 2016-12-02 11:22:54
tags: [node.js, pm2]
---

🍺 pm2 是啥我就不过多介绍了, 总之是神器.

pm2 写一个配置文件就可以运行你的代码.

写出配置文件后就不用每次写命令时带很多参数了, 减轻了命令行的代码量.
同时查看log信息也不需要用命令行查看, 直接把log文件打印到项目中随时查看log, 方便快捷.

<!--more-->

项目根目录新建一个 `pm2.config.json` 文件

👇

```json
{
    // 项目名称
    "name": "app name",
    // 启动文件
    "script": "./app.js",
    // 是否监听文件变化
    "watch": true,
    // 如果你是koa框架需要加入 --harmony 或者别的参数都可以加到这里
    "node_args": "--harmony",
    
    // log 相关
    // 输出 log 时, 前缀时间格式化
    "log_date_format": "YYYY-MM-DD HH:mm Z -> ",
    // out log 文件地址
    "out_file": "./logs/pm2-out.log",
    // error log 文件地址
    "error_file": "./logs/pm2-error.log"
}
```

更多参数请查看 [官方文档](http://pm2.keymetrics.io/docs/usage/application-declaration/#multiple-json)
