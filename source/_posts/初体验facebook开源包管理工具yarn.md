---
title: 初体验facebook开源包管理工具yarn
date: 2016-10-18 14:27:07
tags: [node.js, yarn]
---

害怕 facebook 网站小伙伴们被墙, 搞了传到 csdn 的图片一张.

![官方LOGO](http://img.blog.csdn.net/20161012124212953)

我勒个去，现在的 node 相关的开源社区太活跃了，github 上 facebook 活跃度已经是第二了，第一是微软🙄.

yarn 又是什么？

> →_→ 包管理工具

包管理工具不是有: npm、bower ...... 了吗!？

> →_→ 好用吗？下载速度快吗？安全吗？可靠吗？有良好的依赖吗？

不知道 ... 凑合用吧.

<!--more-->

哈哈哈哈开始说之前, 我也说不出来 npm 是不是好用, 但是慢不是一个人说的, 我觉得也慢.

yarn 还简化了 npm 的使用.

就冲这两点, 也应该看一下.

好下面直接开始!

##### 安装
```
$ npm install -g yarn
```

##### 初始化
```
$ yarn init

# 查看是否正确安装好
$ yarn --version
```

我尼玛和 npm 有什么区别!？

> 别着急嘛

##### 管理依赖

###### 添加依赖
```
# 这里不需要 --save 参数直接可以写到 package.json 文件里, 后面可跟@version版本号、也可以跟@tag 指代git上的推送的tag
$ yarn add [package]
```

###### 更新依赖
```
# 更新某个包, 同时也有 @version 、 @tag. 不加包名即更新所有依赖
$ yarn upgrade [package]

```

###### 移除依赖
```
# 移除某个包 
$ yarn remove [package]
```

###### 如果有 package.json 或者是 bower.json
```
# 直接 yarn 或者 yarn install 即可安装所有依赖
$ yarn
```

###### 下一个包玩玩
```
$ yarn add g2
yarn add v0.15.1
info No lockfile found.
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 📃  Building fresh packages...
success Saved lockfile.
success Saved 1 new dependency
└─ g2@2.0.4
✨  Done in 2.10s.
```
运行下来没有那么多警告, 还有很友好的时间等其他的提示, 还会直接建立 `package.json` 文件.

```
{
  "dependencies": {
    "g2": "^2.0.4"
  }
}
```

是不是用着很舒服呢？

至于那个什么锁的东西我还不会...😁哈哈哈哈



