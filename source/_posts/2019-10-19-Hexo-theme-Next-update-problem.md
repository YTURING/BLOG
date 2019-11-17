---
title: Hexo-theme-Next 升级出现的问题
date: 2019-10-19 19:23:07
categories:
- [Linux]
tags:
- [Hexo]
---
突然想升级一下Hexo主题，升级完发现文章全部成了白色，还以为我哪里弄错了，最后才发现是cdn问题，因为我用的是cloudflare，默认speed下的Rocket Loader是开启的，hexo-theme-next官方说这个会有冲突。关了就可以正常显示了。  

![cloudflare](/images/20191019200643.png)
