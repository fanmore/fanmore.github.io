---
title: Hexo博客系列：高级配置
categories: Ⅰ随笔
sort: 3
tags:
  - 博客
  - Next主题
  - Hexo
date: 2022-06-28 14:16:30
updated: 2022-06-28 14:16:26
---

> 使用了Hexo+Next的组合搭建了博客平台后，发现有些地方和样式不是很喜欢，于是进行了深层的自定义，Next测试版本为8.8.2

# CSS样式调整

在文件`source/css/main.styl`中的末尾增加自己喜欢的样式如：

```css
body {
  /* 字体调整 */
  font-family: 'Helvetica Neue', Helvetica, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', '微软雅黑', Arial, sans-serif;
  /* 防止页面因没有滚动条导致的右移现象 */
  min-height: 101vh;
}

/* 定义滚动条高宽、宽度及背景 */
::-webkit-scrollbar
{
  width: 7px;
  height: 7px;
  background-color: #f5f7f9;
}
/* 定义滚动条轨道 内阴影+圆角 */
::-webkit-scrollbar-track
{
  -webkit-box-shadow: inset 0 0 6px #f5f7f9;
  background-color: #fff5f7f9ffff;
}
/* 定义滑块 内阴影+圆角 */
::-webkit-scrollbar-thumb
{
  -webkit-box-shadow: inset 0 0 6px #c1c1c1;
  background-color: #c1c1c1;
  transition: all .2s;
}
/*滑块效果*/
::-webkit-scrollbar-thumb:hover
{
  -webkit-box-shadow: inset 0 0 5px #a8a8a8;
  background: #a8a8a8;
}
```

<!-- more -->

# 固定页面宽度

注意：测试主题为`Pisces`

使用过`5.x`版本的`Next`就会发现，那时的页面宽度是固定的，并且比较窄，而升级到`8.x`后，发现页面宽度较宽，而且屏幕越大越宽😂解决办法：编辑`source/css/_variables/Pisces.styl`：

```
$content-desktop-large        = 1200px;
$content-desktop-largest      = 1200px;
```

# 个人调整的样式

```css
// 侧边网站标题背景
.site-brand-container {
  background: #399C9C;
}
// 引用样式
blockquote {
  border-left: 4px solid #399C9C;
  background-color: #eee;
  margin-bottom: 20px;
}
// 引用距离调整
blockquote p {
  margin: 0;
  padding: 10px 0;
}
// 版权
.post-copyright ul {
  border-left: 3px solid #399C9C;
  font-size: 13px
}
// 选中文字颜色
::selection {
    background:#399C9C; 
    color:#FFF;
}
::-moz-selection {
    background:#399C9C; 
    color:#FFF;
}
::-webkit-selection {
    background:#399C9C; 
    color:#FFF;
}
// 行内代码块
code {
    color: #ff7600;
    background: #EFF2F3;
    margin: 2px;
}
// 首页文章标题与图片距离
.posts-expand .post-gallery {
  margin-bottom: 10px;
}
// 首页文章标题与简介距离
.posts-expand .post-header {
  margin-bottom: 10px;
}
// 首页文章简介与阅读更多按钮距离
.post-button {
  margin-top: 20px;  
}
// 整体字体大小
.post-body {
  font-size: 16px;  
}
// 分页颜色
.pagination .page-number.current {
  background: #399C9C;
  border-color: #399C9C;
  color: #fff;  
}
// 阅读全文按钮样式
.post-button .btn {
  border-color: #399C9C;
  color: #399C9C;
}
.post-button .btn:hover {
  background-color: #399C9C;
  color: #fff;
}
// 文章标题距离顶部
.post-block:first-of-type {
  padding-top: 0;
}
.posts-expand .post-title {
  margin-bottom: 10px;
}
.post-meta {
  margin-bottom: 10px;
}
// 文字链接
.post-body p a {
  color: #65a1d6;
  transition: all .1s;
}
.post-body p a:hover {
  color: #ff7600;
}
// 白色图片增加边框
.post-body img {
  border: 1px solid rgba(0,0,0,0.1);  
}
// 取消顶部横条
.headband {
  display: none;  
}
// 隐藏作者名字（底部有名字）
.site-author-name {
  display: none;
}
// 对作者描述文字进行靠左处理（字数少不建议）
.site-description {
  text-align: left;
  text-indent: 2em;
  margin-top: 10px;
  font-size: 0.85em;
  padding: 0 5px;
}
// 底部调整为一列展示
.footer-inner {
  display: block;
}
.footer-inner div {
  display: inline-block
}
```

# 分类中多余图片

分类的作用本就是快速查找到文章，结果发现文章中`photo`的图片出现在了分类中，实属占地😂，去除方法，打开`layout/_macro/post-collapse.njk`，注释或删除以下代码：

```
{{ post_gallery(post.photos) }}
```

# 文章加密

在`layout/_partials/head/head-unique.njk`中末尾加上以下代码：

```html
<script>
  (function(){
      if('{{ page.password }}'){
          if (prompt('请输入文章密码') !== '{{ page.password }}'){
              history.back();
          }
      }
  })();
</script>
```

然后在文章的头部加上`password`即可加密文章，在访问时需要输入密码

# 自定义分类到侧边

个人对于文章的管理喜欢从分类中查找，这样更加便捷，但是`Next`的分类需要多点击一次，无法直接查看，所以将分类提取到了侧边

<img style="width:250px " src="/images/1656566039755.png" />

编辑`layout/_layout.njk`，在`<header>`中加入以下代码：

```html
<header class="header" itemscope itemtype="http://schema.org/WPHeader">
  <div class="header-inner">
    {%- include '_partials/header/index.njk' -%}
  </div>

  {# 自定义分类 #}
  <div class="header-inner myCate">{% include '_partials/page/categoriesCustom.njk' %}</div>
  
  {%- if theme.sidebar.display !== 'remove' %}
    {% block sidebar %}{% endblock %}
  {%- endif %}
</header>
```

然后创建`layout/_partials/page/categoriesCustom.njk`文件，内容为：

```html
<div class="category-all-page">
  <div class="category-all">
    {{ list_categories() }}
  </div>
</div>
```

然后就可以把导航中的分类隐藏了，并对样式做相应的调整：

```css
// 自定义分类样式调整
.myCate {
  margin-top: 12px;
  box-sizing: border-box;  
  padding: 5px 0;
  border-radius: 1px;
}
.cate-title {
  height: 20px;
  width: 100%;
  font-size: 0.8125em;
  padding: 0 20px 10px 20px;
  font-weight: bold;
}
.myCate li {
  padding: 0 23px;
  line-height: 32px;
}
.myCate .category-list-item {
  margin: 0;
  transition: all .2s;
}
.myCate .category-list-item:hover {
  background-color: #F5F5F5;  
}
.myCate .category-all {
  margin-top: 0;  
}
// 自定义分类统计靠右
.myCate .category-list-count {
  float: right;  
}
// 自定义分类下划线
.myCate .category-list-link {
  border-bottom: none;
  font-size: 0.8125em;  
}
.myCate .category-list-count {
  font-size: 0.8125em;  
}
// 去除文章数量的括号
.myCate .category-list-count::before {
  content: '';
}
.myCate .category-list-count::after {
  content: '';
}
// 取消代码行数字，兼容横向滑动
.table-container .gutter {
  display: none !important;
}
```

调整分类中的时间显示格式，在`layout/_macro/post-collapse.njk`将格式化更改为`YYYY-MM-DD`：

```
<div class="post-meta-container">
  <time itemprop="dateCreated"
        datetime="{{ moment(post.date).format() }}"
        content="{{ date(post.date, config.date_format) }}">
    {{ date(post.date, 'YYYY-MM-DD') }}
  </time>
</div>
```

当文章的年份不一样时，会出现以下情况，不是很美观

<img style="width:400px" src="/images/1656565186296.png"/>  

所以建议将年取消，在`layout/_macro/post-collapse.njk`将以下代码删除

```
{%- if year !== current_year %}
  {%- set current_year = year %}
  <div class="collection-year">
    <span class="collection-header">{{ current_year }}</span>
  </div>
{%- endif %}
```

同时对样式进行调整：

```css
// 调整分类中文章间隔
.posts-collapse .post-content .post-header {
  margin: 15px 0px 
}

// 修复分类中文章做出小点位置
.posts-collapse .post-content .post-header::before {
  left: -4px;
}
```

最后样子

<img style="width:400px" src="/images/1656565558788.png"/>

