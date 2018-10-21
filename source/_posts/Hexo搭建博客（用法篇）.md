---
title: Hexo搭建博客（用法篇）
date: 2018-10-20 12:46:29
category: hexo
tags: [hexo, markdown]
---

本片主要介绍如何使用Hexo博客。
<!-- more -->

Hexo博客支持Markdown语法，所以我们要使用Hexo博客，基本的Markdown还是要会的，不会的朋友可以去 [Markdown基本语法](https://www.jianshu.com/p/191d1e21f7ed) 这里学习一下。

### 新建博客文章
在博客根目录下使用命令 `hexo new '文章名称'` 创建文章，会在source/_posts/目录下生成名称为*文章名称.md*的博客。我们也可以直接进入该目录下手动创建markdown文件。

### 为文章添加meta信息
我们新创建的文章，头部只有 *title*、*date* 和 *tags* 属性，我们可以增加 *category*、*tags* 和 *comments* 等属性。
* category：文章分类，hexo会自动生成该分类信息
* tags：文章标签，可以添加多个标签，hexo会自动生成标签信息
`tags: [tag1, tag2...]`
* comments：添加评论插件后，设置是否可以评论。

### 图片的插入
文章中想要插入图片，有多中方式选择：
1. 在source目录下创建images目录，用于存放图片，在文章中可以直接使用`/images/avatar.gif`的方式引入。
2. 使用图床，推荐使用*七牛云*，可以免费上传图片，然后复制图片地址插入文章中即可。



