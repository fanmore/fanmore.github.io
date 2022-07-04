---
title: Hexo博客系列：Next主题
categories: Ⅰ随笔
sort: 2
tags:
  - 博客
  - Next主题
  - Hexo
date: 2022-06-28 11:22:44
updated: 2022-06-28 11:22:48
---

> Hexo静态博客生成器下一款博客展示样式，博客网站的样式比较好看，同时扩展了很多功能，功能比较全的主题，使用版本：8.12.1

# 官网及下载

[Next官网](http://theme-next.iissnan.com/)

下载方式到[最新的Next仓库](https://github.com/next-theme/hexo-theme-next)拉取，然后放到`themes`下，在配置文件中启用

```
// 这里是文件夹名字
theme: next
```

Next主题的所有配置在Next目录下的`_config.yml`中，更改文件即会生效，不需要重启

# 主题选择

```yml
# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```

<!-- more -->

# 网站图标

```yml
favicon:
  small: https://s1.ax1x.com/2020/11/10/BqTOfA.png
  medium: https://s1.ax1x.com/2020/11/10/BqTOfA.png
  apple_touch_icon: https://s1.ax1x.com/2020/11/10/BqTOfA.png
  safari_pinned_tab: https://s1.ax1x.com/2020/11/10/BqTOfA.png
```

# 菜单配置

```yml
menu:
  home: / || fa fa-home
  导航: /nav || fa fa-paper-plane
  categories: /categories || fa fa-th
  archives: /archives || fa fa-archive
  作者: /author || fa fa-user
```

通过命令`hexo n page nav`创建与路径同名的文件，然后进行配置：

```
---
title: 标签
date: 2020-11-10 13:57:59
type: "tags"
comments: false
---
```

Next使用的图标为[Font Awesome](https://fontawesome.dashgame.com/)，可根据喜欢自行选择

# 开启版权

```yml
creative_commons:
  license: by-nc-sa
  sidebar: true
  post: false
  language:
```

# 浏览进度

```yml
back2top:
  enable: true
  # Back to top in sidebar. 放在作者下
  sidebar: true
  # Scroll percent label in b2t button.
  scrollpercent: true
```

# 底部运行时间

```yml
footer:
  # Specify the year when the site was setup. If not defined, current year will be used.
  since: 2021
```

# 侧边头像

```yml
# Sidebar Avatar
avatar:
  # Replace the default image and set the url here.
  url: /imgs/avatar.png
  # If true, the avatar will be displayed in circle. 圆形
  rounded: false
  # If true, the avatar will be rotated with the cursor. 旋转
  rotated: true
```

# 背景彩带

```yml
canvas_ribbon:
  enable: true
  size: 300 # The width of the ribbon
  alpha: 0.6 # The transparency of the ribbon
  zIndex: -1 # The display level of the ribbon
```

# 侧边社交

```yml
social:
  GitHub: https://gitee.com/riskfan || fab fa-github
```

# 代码复制功能

```yml
copy_button:
    enable: true
```

# 异步加载

相同的数据不在重复加载，加快页面加载，页面转换更流畅，最喜欢的一个点在于我放在侧边的音乐不会因为浏览不同的页面而使音乐暂停😄

```yml
# Easily enable fast Ajax navigation on your website.
# For more information: https://github.com/next-theme/pjax
pjax: true
```

# 图片放大功能

```yml
# FancyBox is a tool that offers a nice and elegant way to add zooming functionality for images.
# For more information: https://fancyapps.com/fancybox/
fancybox: true
```

# 导航开启统计

```yml
menu_settings:
  icons: true
  badges: true
```

在进入文章后页面后，导航是没有小点的，这个时候会出现导航右侧空白的情况，不喜欢该风格的情况可以开启导航统计，缓解右侧空白的情况并且导航统计是左右对称的，小点是靠右一丢丢的，与自定义的左侧分类能保持一致的宽度

# 底部驱动

个人建议还是开启，毕竟开源需要大家维护

```yml
footer：
  # Powered by Hexo & NexT
  powered: true
```

# ICP备案展示

```
footer:
  beian:
    enable: true
    icp: xxxx
```

# 文章底部标签图标

```yml
# Use icon instead of the symbol # to indicate the tag at the bottom of the post
tag_icon: true
```

默认使用的是`#`，开启后就有图标了

# 开启图片懒加载

```yml
# Vanilla JavaScript plugin for lazyloading images.
# For more information: https://apoorv.pro/lozad.js/demo/
lazyload: true
```

# 网页初始化慢问题

有天发现网站刷新后要等很久网页才出来，网上说的都是字体问题，在配置文件中也发现了其字体的配置位置：

```yml
font:
  enable: false

  # Uri of fonts host, e.g. https://fonts.googleapis.com (Default).
  host:
```

根据网上的方法进行了配置，刚开始并没有效果，不知是否缓存问题，之后有了效果

```yml
font:
  enable: true

  # Uri of fonts host, e.g. https://fonts.googleapis.com (Default).
  host: //fonts.lug.ustc.edu.cn
```