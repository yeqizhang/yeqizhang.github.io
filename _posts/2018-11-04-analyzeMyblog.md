---
layout: post
title: 分析本 Jekyll 博客语法与文件结构
categories: [Jekyll]
description: 记录分析本 Jekyll 博客的语法以及文件结构，以便后面定制修改界面。
keywords: 博客, Jekyll, 语法
---

# 前言

*本博客其实就固定几个页面，各个页面可以独立存在。我们访问 Github Pages 网站时的入口为 index.html ，然后通过菜单跳转到各个页面。
但各个页面也有相同的部分，那就是最上面（ header ）“ title + 菜单 ”的那一行、搜索框，以及底部（ footer ）的一部分；*

# 一、基础语法

## 1、配置

### _config.yml

jekyll 的全局配置在 _config.yml 文件中配置.

比如网站的名字, 网站的域名, 网站的链接格式等等.

### _includes文件夹

对于网站的头部, 底部, 侧栏等公共部分, 为了维护方便, 我们可能想提取出来单独编写, 然后使用的时候包含进去即可.

这时我们可以把那些公共部分放在这个目录下.

使用时只需要`引入`即可.

`{ % include filename % }`

### _layouts 文件夹

对于网站的布局,我们一般会写成**模板**的形式,这样对于写实质性的内容时,比如文章,只需要专心写文章的内容, 然后加个标签**指定用哪个模板**即可. 对于内容,指定了模板后,我们可以称**内容是模板的儿子**.
为什么这样说呢? 因为这个模板时**可以多层嵌套**的, 内容实际上属于模板,只不过是叶子节点而已.

1、在模板中, *引入*儿子的内容.

`{ { content } }`

2、在儿子中,指定父节点模板(注意,必须在子节点的顶部，比如写post博文时指定模板).
```
---

layout: post

---
```

### _data 文件夹

也用于配置一些全局变量,不过数据比较多,所以放在这里。

比如这个网站如果是多人开发, 我们通常会在这里面定义一个 members.yml 文件.

文件内容为
```
- name: 袁小康
  github: tiankonguse
  oldnick : shen1000
  nick : skyyuan
```
然后在模板中我们就可以通过下面语法使用这些数据了.

`site.data.members`

## 2、模板语法

### 使用变量

所有的变量都是一个树节点, 比如模板中定义的头部变量,需要使用下面的语法获得

`page.title`

page 是**当前页面的根节点**.

其中**全局根结点**有

site  _config.yml 中配置的信息
page  页面的配置信息
content  模板中,用于引入子节点的内容
paginator  分页信息

1、site 下的变量 (site节点下的变量)
```
site.time 运行 jekyll 的时间
site.pages 所有页面
site.posts 所有文章
site.related_posts 类似的10篇文章,默认最新的10篇文章,指定lsi为相似的文章
site.static_files 没有被 jekyll 处理的文章,有属性 path, modified_time 和 extname.
site.html_pages 所有的 html 页面
site.collections 新功能,没使用过
site.data _data 目录下的数据
site.documents 所有 collections 里面的文档
site.categories 所有的 categorie
site.tags 所有的 tag
site.[CONFIGURATION_DATA] 自定义变量
```

2、page 下的变量
```
page.content 页面的内容
page.title 标题
page.excerpt 摘要
page.url 链接
page.date 时间
page.id 唯一标示
page.categories 分类
page.tags 标签
page.path 源代码位置
page.next 下一篇文章
page.previous 上一篇文章
```
3、paginator 下的变量
```
paginator.per_page 每一页的数量
paginator.posts 这一页的数量
paginator.total_posts 所有文章的数量
paginator.total_pages 总的页数
paginator.page 当前页数
paginator.previous_page 上一页的页数
paginator.previous_page_path 上一页的路径
paginator.next_page 下一页的页数
paginator.next_page_path 下一页的路径
```

### 字符转义
有时候想输出 { 了,怎么办,使用 \ 转义即可.

`\{ => {`

### 输出变量

输出变量直接使用两个大括号括起来即可.

`{ { page.title } }`

### 循环
和平常的解释性语言很想.
```
{ % for post in site.posts % }
    <a href="http://blog/2014/11/10/jekyll-study/{ { post.url } }">{ { post.title } }</a>
  { % endfor % }
```
  
### 自动生成摘要
```
  { % for post in site.posts % }
     { { post.url } } { { post.title } }
      { { post.excerpt | remove: 'test' } }
  { % endfor % }
```  

### 删除指定文本
remove 可以删除变量中的指定内容

`{ { post.url | remove: 'http' } }`

### 删除 html 标签
这个在摘要中很有用.

`{ { post.excerpt | strip_html } }`

### 代码高亮
```
{ % highlight ruby linenos % }
\# some ruby code
{ % endhighlight % }
```

### 数组的大小

`{ { array | size } }`

### 赋值

`{ % assign index = 1 % }`

### 格式化时间
```
{ { site.time | date_to_xmlschema } } 2008-11-07T13:07:54-08:00
{ { site.time | date_to_rfc822 } } Mon, 07 Nov 2008 13:07:54 -0800
{ { site.time | date_to_string } } 07 Nov 2008
{ { site.time | date_to_long_string } } 07 November 2008
```

### 搜索指定key
```
# Select all the objects in an array where the key has the given value.
{ { site.members | where:"graduation_year","2014" } } 
```

### 排序

`{ { site.pages | sort: 'title', 'last' } }`

### to json

`{ { site.data.projects | jsonify } }`

### 序列化
把一个对象变成一个字符串

`{ { page.tags | array_to_sentence_string } }`

### 单词的个数

`{ { page.content | number_of_words } }`

### 指定个数
得到数组指定范围的结果集

`{ % for post in site.posts limit:20 % }`

## 3、内容名字规范

对于博客,名字必须是 YEAR-MONTH-DAY-title.MARKUP 的格式.

比如
```
-11-06-memcached-code.md
-11-06-memcached-lib.md
-11-06-sphinx-config-and-use.md
-11-07-memcached-hash-table.md
-11-07-memcached-string-hash.md
```

# 二、本博客文件结构

## 如何把_layouts 文件夹下的 default.html页面（site header区域）引入？

可通过jekyll语法：
```
---
layout: default
---
```
default.html代码：

\{`%` include header.html `%`\}

\{\{ content \}\}

\{`%` include footer.html `%`\}

可得知

default.html 将在被引入的页面前后分别加 header.html footer.html 

## 下面开始分析jekyll是怎么组合起各页面

	页面有复用。

### 1、index.html （菜单“首页”）
	通过 markdown 语法 layout: default 引入 _layouts 文件夹下的 default.html页面, 包含了header.html  footer.html 。
	
	 本身代码部分（content部分）
	 
	 2.1 banner （第1个区域）
	 
	 2.2 content （第2个区域）
	 
	 	包含 	columns（第2个区域的上面部分的左边）   搜索框和其他（第2个区域的上面部分的右边） 
	 		
	 			翻页组件 （第2个区域的下面部分）

### 2、菜单“分类”  对应categories.md编译的页面。
	categories.md通过 markdown 语法 layout: categories 引入 _layouts 文件夹下的 categories.html页面,categories.html又通
	过 markdown 语法 layout: default 引入 _layouts 文件夹下的 default.html页面, 包含了header.html  footer.html 。
	
	本身代码部分（content部分），注意两个文件的代码。
	
	前面部分为categories.md的代码，对应分类+博客文章标题列表
	
	后面部分为 categories.html的代码，对应分页、分享栏、评论栏 、以及右侧的搜索框、 Blog Categories
	
### 3、菜单“维基”  对应 wiki.md编译的页面。
 	和“分类”一样，
 	wiki.md通过 markdown 语法 layout: wiki 引入 _layouts 文件夹下的 wiki.html页面,wiki.html又通
	过 markdown 语法 layout: default 引入 _layouts 文件夹下的 default.html页面, 包含了header.html  footer.html 。
	
	本身代码部分（content部分），注意两个文件的代码。
	
	前面部分为wiki.md的代码，对应分类+博客文章标题列表
	
	后面部分为 wiki.html的代码，对应分页、分享栏、评论栏 以及右侧的搜索框。
	
### 4、菜单“链接”  对应 links.md编译的页面。   （没有links.html）
	和“维基”一样，
 	links.md通过 markdown 语法 layout: page 引入 _layouts 文件夹下的 page.html页面,page.html又通
	过 markdown 语法 layout: default 引入 _layouts 文件夹下的 default.html页面, 包含了header.html  footer.html 。
	
	本身代码部分（content部分），注意两个文件的代码。
	
	前面部分为wiki.md的代码，对应  分类+博客  文章标题列表
	
	后面部分为 wiki.html的代码，对应分页、分享栏、评论栏 以及右侧的搜索框。

### 5、菜单“关于”  对应 about.md编译的页面。   （没有about.html）
	和“链接”一样，
 	links.md通过 markdown 语法 layout: page 引入 _layouts 文件夹下的 page.html页面,page.html又通
	过 markdown 语法 layout: default 引入 _layouts 文件夹下的 default.html页面, 包含了header.html  footer.html 。
	
	本身代码部分（content部分），注意两个文件的代码。
	
	前面部分为wiki.md的代码，对应分类+博客文章标题列表
	
	后面部分为 wiki.html的代码，对应分页、分享栏、评论栏 以及右侧的搜索框。

### 6、点击博文打开的页面
	对_posts文件中符合/:year/:month/:day/:title/要求的.md文件进行编译成html，每个post的html中开始都包含了一下jekyll代码：
	```
		---
		layout: post
		title: 本地部署Jekyll并使用之写此文时查看效果
		categories: [环境搭建]
		description: 记录搭建Jekyll环境时遇到的问题和解决办法
		keywords: 部署, Jekyll
		---
	```	
	 markdown 语法 layout: post 引入 _layouts 文件夹下的 post.html模板页面,  post.html页面又引入了 _layouts 文件夹
	 下的 default.html页面，在前后追加了header.html  footer.html .
	 
	 post.html本身的代码（即default的content部分）
	 
	 和index.html本身的代码类似，只是少了翻页，换做了分享组件以及评论组件。
	 
# 参考链接

[Jekyll 语法简单笔记](http://ju.outofmemory.cn/entry/98471)
