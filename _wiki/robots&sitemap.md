---
layout: wiki
title: robots&sitemap
categories: website
description: robots和sitemap的相关知识
keywords: robots, website
---

# robots

## robots.txt是什么？

robots.txt 是搜索引擎中访问网站的时候要查看的第一个文件。 robots.txt 文件告诉蜘蛛程序在服务器上什么文件是可以被查看的。
当一个搜索蜘蛛访问一个站点时，它会首先检查该站点根目录下是否存在 robots.txt ，如果存在，搜索机器人就会按照该文件中的内容来确定访问的范围；如果该文件不存在，
所有的搜索蜘蛛将能够访问网站上所有没有被口令保护的页面。百度官方建议，仅当您的网站包含不希望被搜索引擎收录的内容时，才需要使用 robots.txt 文件。如果您希望
搜索引擎收录网站上所有内容，请勿建立 robots.txt 文件。

如果将网站视为酒店里的一个房间， robots.txt 就是主人在房间门口悬挂的“请勿打扰”或“欢迎打扫”的提示牌。这个文件告诉来访的搜索引擎哪些房间可以进入和参观，哪些
房间因为存放贵重物品，或可能涉及住户及访客的隐私而不对搜索引擎开放。但 robots.txt 不是命令，也不是防火墙，如同守门人无法阻止窃贼等恶意闯入者。

##robots协议

Robots 协议（也称为爬虫协议、机器人协议等）的全称是“网络爬虫排除标准”（ Robots Exclusion Protocol ），网站通过 Robots 协议告诉搜索引
擎哪些页面可以抓取，哪些页面不能抓取。

Robots 协议是国际互联网界通行的道德规范，基于以下原则建立：
```
1、搜索技术应服务于人类，同时尊重信息提供者的意愿，并维护其隐私权；

2、网站有义务保护其使用者的个人信息和隐私不被侵犯。
```

## 总结

所以，robots.txt 主要是为了保护网站主人的隐私权，任何遵循 Robots 协议的搜索引擎都不会爬取到不允许爬的页面，但不遵循此协议的则可能不会如此。

# sitemap

打开robots.txt有时候会展示一个 Sitemap 。
例如，打开

`https://yeqizhang.github.io/robots.txt`

显示的内容为：

`Sitemap: https://yeqizhang.github.io/sitemap.xml`

Sitemap 可方便网站管理员通知搜索引擎他们网站上有哪些可供抓取的网页。最简单的 Sitemap 形式，就是 XML 文件，在其中列出网站中的网址以及关于每个网址的其
他元数据（上次更新的时间、更改的频率以及相对于网站上其他网址的重要程度为何等），以便搜索引擎可以更加智能地抓取网站。

Google 、雅虎、和微软都支持一个被称为xml网站地图（ xml Sitemaps ）的协议，而百度 Sitemap 是指百度支持的收录标准，在原有协议上做出了扩展。百度sitemap的作用
是通过 Sitemap 告诉百度蜘蛛全面的站点链接，优化自己的网站。百度 Sitemap 分为三种格式：txt 文本格式、xml 格式、 Sitemap 索引格式。

总结：

`可以用来做seo优化~`


