---
title: 本博客说明书
date: 2021-09-07 11:19:02
tags: 博客说明
categories: 博客说明
comments: true
---
{% blockquote %}
幸福不是一件容易的事：她很难求之于自身，但要想在别处得到则不可能。——尚福尔
{% endblockquote %}

19年搭过一个博客，可惜不好用，上传的图片经常不显示，于是就放弃使用了，后来把地址搞丢了找不回来了。
所以我又重新用hexo框架重新搭了一个。
后续会把写过的文档陆续上传到这边

以下是功能测试：

嵌入视频
{% youtube lJIrF4YjHfQ %}

嵌入B站视频测试
{% raw %}
<div style="position: relative; width: 100%; height: 0; padding-bottom: 75%;">
<iframe src="//player.bilibili.com/player.html?aid=846655043&bvid=BV1e54y1n7XK&cid=371989206&page=1" scrolling="no" border="0" frameborder="no" framespacing="0"allowfullscreen="true" style="position: absolute; width: 100%; height: 100%; Left: 0; top: 0;"> </iframe></div>
{% endraw %}


嵌入图片
![](/img/bg/bg.jpg)


代码块
```javaScript
console.log('zouHuaLu')
hexo g
hexo g -w
hexo server
hexo clean
hexo deploy
```

如果你看到`We were unable to load Disqus. If you are a moderator please see our troubleshooting guide.`,说明本博客的评论功能有问题，后续有时间再改吧