---
layout: post
title: 使用jekyll-paginate进行博客分页
description: 介绍博客分页的实现方法
tag: github, jekyll-paginate
category: 随笔、工具
---

## 背景

可预期的将来，博客中文章的数量会越来越多，如果不进行分页显示，不仅仅会增加首页的渲染时间，还会带来使用上极大的不便。因此，分页功能变得非常迫切。

jekyll 2.x本身支持分页功能，但到了3.0，便不再直接支持分页了，但我们可以使用jekyll-paginate插件来实现分页的效果。接下来，就讲讲怎么使用jekyll-paginate实现分页吧，ps：讲述各种踩坑后的感悟。

## 使用

首先，需要先安装jekyll-paginate

```bash
sudo gem install jekyll-paginate
```

安装好后，需要修改配置文件_config.yml，在配置文件中增加分页的配置

```bash
# 分页
plugins: [jekyll-paginate]
paginate: 10                  # 每页有几篇文章
paginate_path: "page:num"     # 分页路径 访问路径：/page1/
```

然后，首页中遍历渲染文章的方式需要进行修改**（注意：为了兼容页面的显示，下面的代码中的‘{ %’和‘{ {’均增加了空格，实际中需要删除空格）**

原先：

```markup
{ % for post in site.posts %}
	<li><a class="post-link" href="{ { post.url }}">{ { post.title }}</a><span class="post-des">{ { post.category }}/{ { post.date | date: "%b %d, %Y" }}</span></li>
{ % endfor %}
```

修改为：

```markup
{ % for post in paginator.posts %}
	<li><a class="post-link" href="{ { post.url }}">{ { post.title }}</a><span class="post-des">{ { post.category }}/{ { post.date | date: "%b %d, %Y" }}</span></li>
{ % endfor %}
```

这样首页（博客主页）就完成了分页功能了。喜大普奔~ (*^▽^*)。But，怎么切换分页呢？

因此，我们需要增加一个分页器。这里，我把分页器单独抽离出来，命名为pagination.html放到_includes文件夹中，然后在index.html（博客主页）引入。

index.html（博客主页）引入：

```markup
{ % include pagination.html %}
```

pagination.html

```markup
<nav class="pagination">
{ % if paginator.previous_page %}
    <span class="pagination-prev"><a href="{ { paginator.previous_page_path }}">«</a></span>
{ % else %}
    <span class="pagination-prev no-pagination"><a>«</a></span>
{ % endif %}
    <span class="pagination-count"><span>{ { paginator.page }}</span> of { { paginator.total_pages }}</span>
{ % if paginator.next_page %}
    <span class="pagination-next"><a href="{ { paginator.next_page_path }}">»</a></span>
{ % else %}
    <span class="pagination-next no-pagination"><a>»</a></span>
{ % endif %}
</nav>
```

最后，我们再写一写自己的样式，这里就不展示了。

运行！jekyll serve！

大功告成！！！