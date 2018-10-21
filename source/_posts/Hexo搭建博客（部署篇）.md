---
title: Hexo搭建博客（部署篇）
date: 2018-10-21 12:46:29
category: hexo
tags: [hexo, github, coding]
---
目前为止我们的博客是搭建起来了，但在实际使用中我们会发现有诸多不便。比如说，我们要发表一篇博文，必须手动hexo g 命令生成静态网页，然后 hexo d 命令将静态文件推送到GitHub远程仓库。第一是比计较麻烦；第二，如果我们换了一个新的环境就没法发博客了。So，我们就要祭出我们的法宝-`Travis CI`了。
<!-- more -->

### 什么是 `Travis CI`
>Travis CI 是目前新兴的开源持续集成构建项目，它与jenkins，GO的很明显的特别在于采用yaml格式，简洁清新独树一帜。目前大多数的github项目都已经移入到Travis CI的构建队列中，据说Travis CI每天运行超过4000次完整构建。

### 部署思路
* 在github博客项目下创建一个新的分支`hexo`，将hexo博客源码push到该分支，`master`分支给git pages部署静态博客文件使用。
* 使用`Travis CI`自动部署`hexo`分支，每次我们写完博客后提交到`hexo`分支，`Travis CI`检测到后会自动部署将静态文件推送到`master`分支和`coding`上的我们配置的项目中。

### 使用 `Travis CI`部署博客
* 要使用 Travis CI，必须要有一个`Github`账号，因为Travis CI只支持在Github上构建项目。
* 登陆Travis CI（使用Github账号登陆）,把github账号的项目加载进来，如果没有自动加载进来，可以手动点击右上角的 `Sync account` 按钮，待到同步完成后将要自动构建的项目开启。
![](http://pgnau40bx.bkt.clouddn.com/travis%20ci%E8%AE%BE%E7%BD%AE.png)
* travis ci设置：
![](http://pgnau40bx.bkt.clouddn.com/travis%20ci%E8%AE%BE%E7%BD%AE2.png)
开启`General`下的两项选线 。
* 配合Access Token：获取github生成的Access Token和coding上生成的Access Token；配置到`Travis CI`下的`Environment Variables`下。
* 编写配置文件 `.travis.yml`
```
language: node_js
node_js:
- 8.9.0
cache:
  directories:
  - node_modules
before_install:
- npm install hexo-cli -g
install:
- npm install
script:
- hexo clean
- hexo generate
after_script:
- cd ./public
- git init
- git config user.name "dilijun"    # 配置自己的用户名
- git config user.email "dlj4job@sina.com"  # 配置自己的邮箱
- git add .
- git commit -m "TravisCI 自动部署"
# Github Pages  CI_TOKEN 为上一步在 Travis CI的 Environment Variables 下配置的变量名称
- git push --force --quiet "https://${CI_TOKEN}@${GH_REF}" master:master
# Coding Pages <username>写自己coding上的用户名，CO_TOKEN同上
- git push --force --quiet "https://<username>:${CO_TOKEN}@${CO_REF}" master:master
branches:
  only:
  - hexo
env:
  global:
  # Github Pages and Coding Pages 配置自己的github和coding地址
  - GH_REF: github.com/dilijun/dilijun.github.io.git
  - GH_REF: git.coding.net/dilijun/dilijun.coding.me.git
```
* 将修改推送到 `hexo` 分支，然后等Travis CI 构建并自动部署成功。
![](http://pgnau40bx.bkt.clouddn.com/Travis%20CI%20%E6%9E%84%E5%BB%BA.png)
