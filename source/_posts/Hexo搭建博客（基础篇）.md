---
title: Hexo搭建博客（基础篇）
date: 2018-10-15 12:46:29
tags: [hexo,github]
category: hexo
comments: true
---
Hexo 是一个基于Node.js的快速、简洁且高效的博客框架。使用Markdown来编写文章，利用靓丽的主题可快速、轻松生成静态网页。
<!-- more -->

### 环境准备
在安装Hexo之前，我们必须保证已安装一下应用程序：
* Node.js
* Git
#### 1. 安装Node.js（如已安装，可忽略该步骤）
下载地址：[Node.js](https://nodejs.org/en/download/)
安装成功后使用以下命令测试是否安装成功：
`node -v`
#### 2. 安装Git（如已安装，可忽略该步骤）
下载地址：[Git](https://git-scm.com/downloads) 
安装成功后使用以下命令测试是否安装成功：
`git --version`

### 搭建博客
#### 1. 安装hexo，命令：
`npm install -g hexo-cli`
#### 2. 初始化博客，命令：
```
hexo init <blog_name> --<blog_name>为你自己博客项目的名称，或者可以 mkdir <blog_name>，进入该目录下执行 hexo init
cd <blog_name>
npm install
```
#### 3. 博客初始化后，可以使用以下命令启动博客：
```
hexo clean --清除缓存文件 (db.json) 和已生成的静态文件 (public)
hexo g(hexo generate) --生成静态页面 
hexo s(hexo server) --开启本地服务
```
#### 4. 此时打开浏览器，访问 http://localhost:4000 即可看到自带默认主题的博客。
![](http://pgnau40bx.bkt.clouddn.com/%E5%88%9D%E5%A7%8B%E5%8D%9A%E5%AE%A2.jpeg)

#### 5. Hexo命令知多少
```
hexo help --可查看hexo命令
hexo generate --生成博客静态页面，也可用缩写命令 hexo g
hexo clean --清除生成的静态页面
hexo server --开启本地服务，也可用缩写命令 hexo s、hexo server --debug 是开启debug模式
hexo deploy --部署博客，也可用缩写命令 hexo d
hexo new 'postName' --新建文章，默认会在 source/_posts/ 下生成<postName>.md的Markdown文件
hexo new page 'pageName' --新建页面，默认会在 source目录下，生成<pageName>文件夹，并在该文件夹下生成 index.md 文件
```

### 博客上github
#### 1. 确保有github账号，没有的可自己去[github](https://github.com)申请。
#### 2. 创建代码库，命名规则为`yourname.github.io`，‘yourname’为你自己github的用户名。
![enter description here](http://pgnau40bx.bkt.clouddn.com/github%E5%88%9B%E5%BB%BA%E4%BB%A3%E7%A0%81%E5%BA%93.jpeg)
#### 3. 生成ssh keys
`ssh-keygen -t rsa -C "your_email@example.com"`，一路回车即可，创建成功后，会在用户目录下的 .ssh 目录下生成 id_rsa和id_rsa.pub两个文件，我们把id_rsa.pub中的内容拷贝到github上即可，title可以随便填写。
![enter description here](http://pgnau40bx.bkt.clouddn.com/github%E9%85%8D%E7%BD%AEssh_key.jpeg)
配置好后，可以使用`ssh -T git@github.com`命令来测试是否配置成功。
#### 4. 修改Hexo配置
* 安装hexo-deployer-git插件，命令为：
`npm install --save hexo-deployer-git`
* 打开根目录下的_config.yml配置文件，修改配置最后部分为
```
deploy:
    type: git
    repo: git@github.com:(username)/(repoName).github.io.git #括号里面换成自己的用户名和仓库名,去掉括号
    branch: master
```
#### 5. 测试并部署
```
hexo clean --清空静态页面
hexo g --生成新的静态页面
hexo d --将public目录下的内容不素到github上
```
按以上目录操作完成后，可打开浏览器访问 `http://username.github.io`来访问博客(username.github.io为你的仓库名称)
#### 6. 绑定域名
* 如果想通过域名访问的就继续，前提是要有自己的域名，要是通过上面的仓库名可以访问就满足的可以跳过这一步
* 去自己的域名下添加解析记录类型为 CNAME 主机记录为 @ 线路选择默认，TTL 选择 600，记录值为 github 的仓库名 username.github.io
* 配置 hexo
创建 CNAME 配置文件 `touch ~/hexo/source/CNAME`，去 CNAME 文件 下添加刚才解析的域名 例如： `zhangsan.com`。然后重新部署一下 `hexo g -d`。此时即可用自己的域名来访问博客了。

### 博客上coding
#### 1. 确保有coding账号，没有的可去[coding](https://coding.net)申请。
#### 2. 创建代码库，命名规则为`username.coding.me`，username为你自己的coding的用户名。
#### 3. 拷贝上面已生成的ssh keys的公钥到coding上
![](http://pgnau40bx.bkt.clouddn.com/coding%E9%85%8D%E7%BD%AE%E5%85%AC%E9%92%A5.png)
#### 4. 修改hexo配置
在根目录配置文件_config.yml中：
```
deploy:
  type: git
  repo: git@github.com:(username)/(repoName).github.io.git #括号里面换成自己的用户名和仓库名,去掉括号
  branch: master
```
改为如下配置：
```
deploy:
  type: git
  repo: 
	github: git@github.com:(username)/(repoName).github.io.git #括号里面换成自己的用户名和仓库名,去掉括号
	coding: git@git.coding.net:(username)/(repoName).coding.me.git #括号里面换成自己的用户名和仓库名,去掉括号
  branch: master
```
#### 5. 再次部署并访问`http://username.coding.me` 。
#### 6. 绑定域名
* 去自己的域名下添加解析记录类型为 CNAME 主机记录为 @ 线路选择默认，TTL 选择 600，记录值为 coding的仓库名 username.coding.me。
* 在coding代码库的左侧`代码`菜单下点击`Pages 服务`，点击`一键开启Coding Pages，进入Pages服务页面偶点击右侧的`设置图标`，在自定义域名下，绑定刚才解析的域名。
* 重新部署hexo。

