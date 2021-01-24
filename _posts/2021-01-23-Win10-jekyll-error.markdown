---
layout: post
title:  "Win10下跑jekyll问题记录"
date:   2021-01-23 21:43:18 +0800
categories: jekyll
typora-root-url: ..\..
---

# 一、前言
  [jekyll](https://jekyllrb.com) 环境第一个是在[MatePad Pro](https://www.vmall.com/product/10086663107289.html)（一台Android设备）上搭的（[Termux](https://play.google.com/store/apps/details?id=com.termux) + [Acode](https://play.google.com/store/apps/details?id=com.foxdebug.acodefree) + [Chrome](https://play.google.com/store/apps/details?id=com.android.chrome)）,整个安装过程非常顺利所以没记录。为了随时可以写Blog,目前所有有键盘的设备都环境都准备好！！！！  

![matepadpro_jekyll](/blog/assets/images/2021-01-23-Win10-jekyll-error/matepadpro_jekyll.jpg)

# 二、问题记录

遇到错误```cannot load such file -- webrick (LoadError)```

```
D:\work\jekyll\my-awesome-site>bundle exec jekyll serve
Configuration file: D:/work/jekyll/my-awesome-site/_config.yml
            Source: D:/work/jekyll/my-awesome-site
       Destination: D:/work/jekyll/my-awesome-site/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.561 seconds.
 Auto-regeneration: enabled for 'D:/work/jekyll/my-awesome-site'
                    ------------------------------------------------
      Jekyll 4.2.0   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
                    ------------------------------------------------
C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
```

参考：[https://github.com/jekyll/jekyll/issues/8523](https://github.com/jekyll/jekyll/issues/8523)

执行：```bundle add webrick```

```
D:\work\jekyll\my-awesome-site>bundle add webrick
Fetching source index from https://mirrors.ustc.edu.cn/rubygems/
Resolving dependencies...
Fetching source index from https://mirrors.ustc.edu.cn/rubygems/
Resolving dependencies...
Using public_suffix 4.0.6
Using addressable 2.7.0
Using bundler 2.2.6
Using colorator 1.1.0
Using concurrent-ruby 1.1.8
Using eventmachine 1.2.7 (x64-mingw32)
Using http_parser.rb 0.6.0
Using em-websocket 0.5.2
Using ffi 1.14.2 (x64-mingw32)
Using forwardable-extended 2.6.0
Using i18n 1.8.7
Using sassc 2.4.0 (x64-mingw32)
Using jekyll-sass-converter 2.1.0
Using rb-fsevent 0.10.4
Using rb-inotify 0.10.1
Using listen 3.4.1
Using jekyll-watch 2.2.1
Using rexml 3.2.4
Using kramdown 2.3.0
Using kramdown-parser-gfm 1.1.0
Using liquid 4.0.3
Using mercenary 0.4.0
Using pathutil 0.16.2
Using rouge 3.26.0
Using safe_yaml 1.0.5
Using unicode-display_width 1.7.0
Using terminal-table 2.0.0
Using jekyll 4.2.0
Using jekyll-feed 0.15.1
Using jekyll-seo-tag 2.7.1
Using minima 2.5.1
Using thread_safe 0.3.6
Using tzinfo 1.2.9
Using tzinfo-data 1.2020.6
Using wdm 0.1.1
Using webrick 1.7.0
```

还是Mac下方便，按[官方文档](https://jekyllrb.com/docs/)操作一点问题也没遇到。