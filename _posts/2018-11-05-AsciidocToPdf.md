---
layout: post
title: 将github中Asciidoc文档格式项目导出pdf文件
categories: [Asciidoc, 书籍]
description: 记录将progit2-zh这本书的源文件Asciidoc使用Ruby导出为pdf文件
keywords: progit2-zh, Asciidoc, 树莓派
---

# 前言

*github 上有许多计算机书籍的源代码，尤其是汉化版的对我这种英语差的帮助大，有时候找到的书籍的源码是 Asciidoc ，但仓库里并没有发布最新代码的 releases 文件，这时候就需要
自己动手把他转成 word 或者 pdf 了。由于博主之前转换 word 的时候好像是失败了，这里我只讲怎么转成 pdf 。使用环境为树莓派。*

# 准备工作

下载书籍源码
 
[progit2-zh](https://github.com/yeqizhang/progit2-zh)

这里我贴上的是我的仓库。

# 正式导出

## 在树莓派上安装 ruby

`sudo yum install ruby `

## 使用bundler添加gemfile的依赖
切换到 progit2-zh 路径下，执行 bundle install(没有bundle就 gem install bundle) ,中间似乎很顺利，到gem某个包时提示我的ruby版本太低了需要2.0后的版本，于是又安装 rvm 来管理 ruby ，  
下载好高版本 ruby 后，由于我的树莓派 cpu 不行，中间编译了大半个小时吧。

接着继续 bundle install （遇见错误，尝试处理后，继续执行这条命令），中间可能还是会有gem安装失败的情况。

注意：

bundle 是从 Gemfile 和 Gemfile.lock 找要 gem 的包自动去下载的，Gemfile.lock 设置了要安装的插件以及版本， 

如果安装失败 ，可以先去 https://rubygems.org/gems 搜这个插件，把 Gemfile.lock 中这个插件的版本设置的高一点，

这里贴一份博主修改后的 Gemfile.lock 的内容：

```
GEM
  remote: https://rubygems.org/
  specs:
    Ascii85 (1.0.2)
    addressable (2.3.8)
    afm (0.2.2)
    asciidoctor (1.5.0)
    asciidoctor-epub3 (1.0.0.alpha.2)
      asciidoctor (>= 1.5.0, < 1.6.0)
      gepub (~> 0.6.9.2)
      thread_safe (~> 0.3.4)
    asciidoctor-pdf (1.5.0.alpha.8)
      asciidoctor (~> 1.5.0)
      prawn (>= 1.3.0, < 3.0.0)
      prawn-icon (= 0.6.4)
      prawn-svg (= 0.20.0)
      prawn-table (= 0.2.1)
      prawn-templates (= 0.0.3)
      safe_yaml (= 1.0.4)
      thread_safe (= 0.3.4)
      treetop (= 1.5.3)
    asciidoctor-pdf-cjk (0.1.2)
      asciidoctor-pdf (~> 1.5.0.alpha.8)
    asciidoctor-pdf-cjk-kai_gen_gothic (0.1.1)
      asciidoctor-pdf-cjk (~> 0.1.2)
    awesome_print (1.2.0)
    coderay (1.1.0)
    css_parser (1.3.7)
      addressable
    epubcheck (3.0.1)
    gepub (0.6.9.2)
      nokogiri (~> 1.6.1)
      rubyzip (>= 1.1.1)
    hashery (2.1.1)
    json (2.0.4)
    kindlegen (2.9.4)
    mini_portile (0.6.0)
    multi_json (1.12.1)
    nokogiri (1.6.3.1)
      mini_portile (= 0.6.0)
    pdf-core (0.6.0)
    pdf-reader (1.3.3)
      Ascii85 (~> 1.0.0)
      afm (~> 0.2.0)
      hashery (~> 2.0)
      ruby-rc4
      ttfunk
    polyglot (0.3.5)
    prawn (2.0.2)
      pdf-core (~> 0.6.0)
      ttfunk (~> 1.4.0)
    prawn-icon (0.6.4)
      prawn (>= 1.1.0, < 3.0.0)
    prawn-svg (0.20.0)
      css_parser (~> 1.3)
      prawn (>= 0.8.4, < 3)
    prawn-table (0.2.1)
    prawn-templates (0.0.3)
      pdf-reader (~> 1.3)
      prawn (>= 0.15.0)
    pygments.rb (1.1.2)
      multi_json (>= 1.0.0)
    rake (10.3.2)
    ruby-rc4 (0.1.5)
    rubyzip (1.1.6)
    safe_yaml (1.0.4)
    thread_safe (0.3.4)
    treetop (1.5.3)
      polyglot (~> 0.3)
    ttfunk (1.4.0)

PLATFORMS
  ruby

DEPENDENCIES
  asciidoctor (= 1.5.0)
  asciidoctor-epub3 (= 1.0.0.alpha.2)
  asciidoctor-pdf (= 1.5.0.alpha.8)
  asciidoctor-pdf-cjk-kai_gen_gothic (~> 0.1.1)
  awesome_print
  coderay
  epubcheck
  json
  kindlegen
  pygments.rb
  rake
  thread_safe

BUNDLED WITH
   1.15.3
```

下面是博主所有插件装好后再次执行 bundle install 的截图：

![bundleinstall]({{ "/images/blog/2018-11-05-bundleinstall.jpg" | absolute_url }})
<div align = "center">bundle install</div><br>


## 下载语言字体包

这一步因为国内网络环境的问题尝试很多次都没有下载下来，后来找到这个插件的 github 项目，直接在发行版里找到了下载，地址
为https://github.com/chloerei/asciidoctor-pdf-cjk-kai_gen_gothic/releases ，下载好
以后放入 asciidoctor-pdf-cjk-kai_gen_gothic插件的子文件夹data/fonts文件夹里。

## 导出pdf
```
bundle exec rake book:build 
```   




