---
date: 2018-10-28 09:22:32
title: commit message 规范
tags: [git]
categories: [SVN&GIT]
---

在提交代码时，`commit message` 必不可少，`commit`的规范如下：

``` xml
<type>类别(<scope>这里说明 commit 影响的范围</scope>)</type>:<subject>这里写上简短的内容</subject>
<!-- 空一行 -->
<body>对这次 commit 的详细描述</body>
<!-- 空一行 -->
<footer>这部分的情况有两种： 1. 不兼容的变动 2. 关闭 issue</footer>
```

### type

* feat：新功能
* fix：修补 bug
* docs：文档
* style：格式（不影响代码格式的变动）
* refactor：重构（既不是新增功能，也不是修复 bug）
* test：增加测试
* chore：构建过程或辅助工具的变动



参考文献

1. [掘金-规范你的 commit message 并且根据 commit 自动生成 CHANGELOG.md](https://juejin.im/post/5bd2debfe51d457abc710b57)