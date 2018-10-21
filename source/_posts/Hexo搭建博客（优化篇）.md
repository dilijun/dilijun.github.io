---
title: Hexo搭建博客（优化篇）
date: 2018-10-19 12:46:29
category: hexo
tags: [hexo, github, coding]
---

上一篇博客介绍了如何搭建一个Hexo博客，但是你以为Hexo就只有这样吗？No！
<!-- more -->
### Hexo基础配置
#### 1.打开根目录下_config.yml配置文件，修改以下配置：
```
title: Mr.D's Blog      #博客标题
subtitle: 红尘嚣 浮华一世转瞬空  #博客子标题
description:   #描述
keywords:     #关键字
author: Mr.D  #博客作者
language: zh-CN     #语言
timezone:
```
### 主题配置
[Hexo官方主题](https://hexo.io/themes/)这里是Hexo的官方主题，本博客使用的是`NexT`主题，其他主题可以去上面的地址查看选>用。
#### 1. 在博客根目录下执行以下命令安装主题：`git clone https://github.com/theme-next/hexo-theme-next themes/next`。
#### 2. 修改根目录下的 _config.yml 配置文件theme，设置主题为`next`。
![](http://pgnau40bx.bkt.clouddn.com/Hexo%E9%85%8D%E7%BD%AE%E4%B8%BB%E9%A2%98.png)
#### 3.在scheme settings下选择自己喜欢的一款主题风格
![](http://pgnau40bx.bkt.clouddn.com/hexo%E4%B8%BB%E9%A2%98%E9%A3%8E%E6%A0%BC%E8%AE%BE%E7%BD%AE.png)
#### 4.配置导航菜单
Hexo默认配置只有`首页`和`归档`
![](http://pgnau40bx.bkt.clouddn.com/Hexo%E9%BB%98%E8%AE%A4%E8%8F%9C%E5%8D%95.png)
我们可以编辑`theme/next`下的`_config.yml`配置来添加其他导航菜单，将需要添加的菜单注释放开即可：
```
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
  ```
![](http://pgnau40bx.bkt.clouddn.com/Hexo%E6%B7%BB%E5%8A%A0%E5%90%8E%E7%9A%84%E8%8F%9C%E5%8D%95%E5%AF%BC%E8%88%AA.png)
这个时候我们新添加的菜单是不可用的，我们必须在`source`目录下创建对应的`page`文件夹，比如要创建`标签`文件夹，执行以下命令：
```
hexo new page `tags`
```
会在`source`下生成`tags`文件夹，文件加下会生成`index.md`文件，编辑`index.md`文件，删除`title`行，添加`type: 'tags'`，`comments: false`的作用是添加评论插件后阻塞用户在该标签页下发表评论：
```
---
type: 'tags'
date: 2018-10-18 22:24:29
comments: false
---
```
#### 5.添加RSS
* 安装插件：在根目录下执行命令  `npm install hexo-generator-feed --save`
* 在根配置文件`_config.yml`文件中配置：
```
# Extensions
## Plugins: https://hexo.io/plugins/
plugins: hexo-generate-feed
```
* 在主题配置文件`theme/next/_config.yml`文件中，找到`rss`，添加如下配置：
```
rss: /atom.xml
```
#### 6.添加 Fork me on Github
在主题配置文件`theme/next/_config.yml`文件中，搜索`Follow me on GitHub`，找到如何配置，打开最后一行的注释，并把 `yourname` 改为你自己的github用户名即可：
```
# Follow me on GitHub banner in right-top corner.
# Usage: `permalink || title`
# Value before `||` delimeter is the target permalink.
# Value after `||` delimeter is the title and aria-label name.
github_banner: https://github.com/yourname || Follow me on GitHub
```
#### 7.博客背景添加粒子效果
* 进入`theme/next/`目录下，执行 以下命令：
`git clone https://github.com/theme-next/theme-next-canvas-nest source/lib/canvas-nest`
* 执行成功后，将主题配置文件`_config.yml`中的 `canvas_nest` 值设置为 `ture`：
```
# Canvas-nest
# Dependencies: https://github.com/theme-next/theme-next-canvas-nest
canvas_nest: true
```
#### 8.修改文章内链样式
* nexT提供了允许用户自定义样式的功能，我们可以进入`theme/next/source/css/_custom/custom.styl`文件中，添加想要的样式即可：
```
//自定义文章内链样式
.post-body
    p
        a
            color: #f0ad4e;
            border-bottom: 1px solid #f0ad4e;
            &:hover 
                color: #0593d3;
                border-bottom: 1px solid #0593d3;
```
#### 9.给文章底部的标签添加图标
* 打开文件`themes/next/layout/_macro/post.swig`，把
```
<div class="post-tags">
      {% for tag in post.tags %}
        <a href="{{ url_for(tag.path) }}" rel="tag"># {{ tag.name }}</a>
      {% endfor %}
</div>
```
中的 `#` 替换为 `<i class="fa fa-tag"></i>` 即可：
```
<div class="post-tags">
      {% for tag in post.tags %}
        <a href="{{ url_for(tag.path) }}" rel="tag">
              <i class="fa fa-tag"></i> 
              {{ tag.name }}
        </a>
      {% endfor %}
</div>
```
![](http://pgnau40bx.bkt.clouddn.com/%E6%96%87%E7%AB%A0%E5%BA%95%E9%83%A8%E6%A0%87%E7%AD%BE%E6%B7%BB%E5%8A%A0%E5%9B%BE%E6%A0%87.png)
#### 10.设置头像动画效果
* 把头像图片添加到 `theme/next/source/images` 下，命名为 `avatar.jpg`
* 打开主题配置文件 `theme/next/_config.yml`，修改以下配置：
```
# Sidebar Avatar
avatar:
  # in theme directory(source/images): /images/avatar.gif
  # in site  directory(source/uploads): /uploads/avatar.gif
  # You can also use other linking images.
  url: /images/avatar.jpg    #头像地址
  # If true, the avatar would be dispalyed in circle.
  rounded: true     #开启圆形头像
  # The value of opacity should be choose from 0 to 1 to set the opacity of the avatar.
  opacity: 1
  # If true, the avatar would be rotated with the cursor.
  rotated: true    #鼠标一上去时头像自动旋转
```
#### 11.给文章添加阴影效果
* 打开文件`theme/next/source/css/_custom/custom.styl`，添加如下配置：
```
//主页文章添加阴影效果    
.post 
     margin-top: 60px;
     margin-bottom: 60px;
     padding: 25px;
     -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
     -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
```
#### 12.开启文章访问量设置
* 进入主题配置文件 `theme/next/_config.yml`，修改 `busuanzi_count` 的 `enable` 属性为 `true` ：
```
# Show Views/Visitors of the website/page with busuanzi.
# Get more information on http://ibruce.info/2015/04/04/busuanzi/
busuanzi_count:
  enable: true
  total_visitors: true
  total_visitors_icon: user
  total_views: true
  total_views_icon: eye
  post_views: true
  post_views_icon: eye
```
#### 13.开启文章中代码复制功能
* 进入主题配置文件 `theme/next/_config.yml`，修改 `codeblock` 下的  `copy_button` 的 `enable` 属性为 `true` ：
```
codeblock:
  # Manual define the border radius in codeblock
  # Leave it empty for the default 1
  border_radius:
  # Add copy button on codeblock
  copy_button:
    enable: true
    # Show text copy result
    show_result: false
```
#### 14.开启网页顶部的进度加载条
* 进入 `theme/next/` 目录下，执行以下命令：
```
git clone https://github.com/theme-next/theme-next-pace source/lib/pace
```
* 进入主题配置文件 `theme/next/_config.yml`，修改 `pace` 属性的值为 `true` ，并选择一款自己喜欢的加载样式 ：
```
pace: true
# Themes list:
#pace-theme-big-counter
#pace-theme-bounce
#pace-theme-barber-shop
#pace-theme-center-atom
#pace-theme-center-circle
#pace-theme-center-radar
#pace-theme-center-simple
#pace-theme-corner-indicator
#pace-theme-fill-left
#pace-theme-flash
#pace-theme-loading-bar
#pace-theme-mac-osx
#pace-theme-minimal
# For example
# pace_theme: pace-theme-center-simple
pace_theme: pace-theme-minimal
```
#### 15.添加评论功能
* 本博客使用的是[来必力](https://livere.com/)，没有账号的可先去注册一个。
* 安装并配置好后，拷贝 `data-uid`的值。
![](http://pgnau40bx.bkt.clouddn.com/Hexo%E8%AF%84%E8%AE%BA%E7%B3%BB%E7%BB%9F.png)
* 在主题配置 `theme/next/_config.yml` 下找到 `livere_uid`，打开注释并配置上一步拷贝出来的 `data-uid`即可。
```
# Support for LiveRe comments system.
# You can get your uid from https://livere.com/insight/myCode (General web site)
livere_uid: your uid
```
#### 16.隐藏底部Hexo强力驱动、主题等信息
* 在主题配置 `theme/next/_config.yml` 下找到 `powered`，把所有属性的值都改为 `false` 。
```
powered:
    # Hexo link (Powered by Hexo).
    enable: false
    # Version info of Hexo after Hexo link (vX.X.X).
    version: false
```
* 在主题配置 `theme/next/_config.yml` 下找到 `theme`，把所有属性的值都改为 `false` 。
```
theme:
    # Theme & scheme info link (Theme - NexT.scheme).
    enable: false
    # Version info of NexT after scheme info (vX.X.X).
    version: false
```
#### 17.添加站内搜索
* 在根目录下执行以下命令安装 `hexo-generator-searchdb`：
```
npm install hexo-generator-searchdb --save
```
* 上一步命令执行成功后，打开主题配置文件 `theme/next/_config.yml`，找到 `local search` 配置，设置属性 `enable` 为 `true` 即可：
```
# Local search
# Dependencies: https://github.com/theme-next/hexo-generator-searchdb
local_search:
  enable: true
  # if auto, trigger search by changing input
  # if manual, trigger search by pressing enter key or search button
  trigger: auto
  # show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # unescape html strings to the readable one
  unescape: false
```
#### 18.开启社交图标
* 打开主题配置文件 `theme/next/_config.yml`，找到 `Social Links` 配置，配置响应的社交链接：
```
# Social Links.
social:
  GitHub: https://github.com/dilijun || github
  #E-Mail: mailto:yourname@gmail.com || envelope
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype
```
#### 19.文章底部配置版权信息
* 打开主题配置文件 `theme/next/_config.yml`，找到 `Declare license on posts`，配置 `post_copyright` 下的 `enable` 属性为 `true`：
```
# Declare license on posts
post_copyright:
  enable: true
  license: <a href="https://creativecommons.org/licenses/by-nc-sa/4.0/" rel="external nofollow" target="_blank">CC BY-NC-SA 4.0</a>
```
#### 20.开启文章阅读进度条
* 在 `theme/next/` 目录下执行以下命令：
```
git clone https://github.com/theme-next/theme-next-reading-progress source/lib/reading_progress
```
* 执行成功后，进入主题配置中，找到 `Reading progress bar`，设置 `enable` 属性为 `true` 即可。
```
# Reading progress bar
# Dependencies: https://github.com/theme-next/theme-next-reading-progress
reading_progress:
  enable: true
  color: "#37c6c0"
  height: 2px
```
#### 21.开启`fancybox`图片插件
* 在 `theme/next/` 目录下执行以下命令：
```
git clone https://github.com/theme-next/theme-next-fancybox3 source/lib/fancybox
```
* 命令执行成功后，打开主题配置，找到 `Fancybox`配置，设置为 `true`。
```
# Fancybox. There is support for old version 2 and new version 3.
fancybox: true
```
