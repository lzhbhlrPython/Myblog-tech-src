---
title: 是GitHub Page还是Vercel？
date: 2021-03-14 23:24:19
tags: [DEVlog]
cover: /cover/gitpage_or_vercel.jpg
mp3: /cover/gitpage_or_vercel.mp3
---

故事要发生在一个友链上，那是我第一次在GitHub上面直接编辑Markdown文档.

([起源LINK](https://tszhong0411.vercel.app/))

编辑完了一想，坏了，由于Vercel对Hexo的支持老是出现Publish编译之后出现问题（其实是因为我的主题用的是SubModule），我一直在用手动编译！！

啊……这可怎么办呢……

机缘巧合之下，一位使用Hugo的热心同学李某给我推荐了GitHub-Action。我立刻将自己的博客与GitHubPage分开了……

真好！

### GitHub Action的失败尝试

![image-20210314233101501](/img/gitpage_or_vercel-1.png)

如您所见，在更换了无数个Action源码，编译错误十次以后……

**我 崩 溃 了** 

这也直接导致了前段时间的博客不可用！先对友链的哥哥姐姐抱歉！

既然GitHub不行……那……还有没有什么可以替代的CI平台呢……？

我一遍一遍的在网上搜寻着。

### Travis CI Plan?

什么？还有个东西叫Travis？

我立马访问了他的官网。

啊……还要手动写配置文件吗……

然后我就写了如下图所示的文件

```yaml
anguage: node_js

node_js: stable

cache:
  directories:
  - node_modules
before_install:

  - npm install -g hexo-cli

install:
  - npm install

script:
  - git submodule update
  - hexo g 

after_script:
  - cd ./public
  - git init
  - git config user.name "lzhbhlrpython"
  - git config user.email "Liubaolin20070125@outlook.com"
  - git add .
  - git commit -m "TravisCI Auto Deploy"
  # Github Pages
  - git push --force --quiet "https://${TOKEN}@${GH_REF}" master:master

env:
  global:
    - GH_REF: github.com/lzhbhlrpython/lzhbhlrpython.github.io.git
  
notifications:
  email:
    - Liubaolin20070125@outlook.com
  on_success: change
  on_failure: always

```

然后竟然成功了呜呜呜！

本博客成功的部署在[GitHub Page](https://lzhbhlrpython.github.io)和[Vercel Page](https://blog.pyliubaolin.top)上了！！

目前这两个博客是同步的（

而且GitHub那边暂时不搞CNAME了。

后续推出Travis教程，感谢您的观看！

> 需要白嫖三级域名的同学欢迎来留言板私聊，我可以帮你推到我的DNS解析上去。
>
> yourfront.yourname.pyliubaolin.top
>
> 域名未备案，仅供您方便与别人互加友链，代TXT解析

> Blog的后续计划:
>
> 发文章！画logo！

