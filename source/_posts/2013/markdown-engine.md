---
title: Markdown 渲染引擎
date: 2013-05-17 22:12:00
category: Tech
tags: 
  - markdown
---

最近在GitHub Pages里提交了新的文件之后，看状态是提交成功了。但是刷新之后仍然无果，这不科学啊，以往都是秒更新的。难道今夜提交，明天才给我更新啊，有点太晚了，就洗洗睡了。

第二天打开邮箱，全是"page build failed"，这就有点犯晕啊，也没有其余的提示内容。手头也没有本地的环境用作测试，只好先借助Google，看样子应该是GitHub这边的Jekyll版本更新之后，出现了这个问题，更换搜索关键词继续，然后心里大概明白了，更改了Jekyll中Markdown的渲染引擎，之后就无忧了。

具体方法：修改配置文件`_config.yml`，    
`markdown: kramdown`，改为`markdown: redcarpet`，    
如果没有这一行，    
在`pygments: true`的上面添加`markdown: redcarpet`。

这里的正是为Jekyll指定需要用到的Markdown渲染引擎。
尝试着该过之后，再次刷新Blog，新的post已经看到了。

> Jekyll 原生支持 `kramdown`，`maruku`，`rdiscount`，`redcarpet` 等 Markdwon 渲染引擎

感谢：
    
* [Jekyll 0.12.0新特性简介][1]    
* [为 Jekyll 装上瑞士军刀 Pandoc][2]

  [1]: http://www.soimort.org/posts/130/
  [2]: http://yangzetian.github.io/2012/04/15/jekyll-pandoc/