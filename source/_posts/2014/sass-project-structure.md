---
title: Sass 学习笔记 - 项目文件结构
date: 2014-06-02 09:00:00
description: 项目中使用了 Sass，对于项目文件管理心得，做个记录。
category: Tech
tags: 
  - Sass
  - Compass
  - Bootstrap
---
项目开始之前接触过 Sass，这次开始开发之前已经下定决心，将 Sass 引入开发环境中。

起初大致是这么安排的：

```html
sass /
├── _variables.scss
├── _mixins.scss
├── _reset.scss
├── _grid.scss
├── _buttons.scss
├── _iconfont.scss
......
├── _home.scss
├── _index.scss
......
├── style.scss
```

其中 `_grid.scss` 这种命名带有 `_`，都是当做 `@import` 导入使用的，这也是告诉 Sass 在编译的时候，不会编译输出这类文件。因为在使用 Grunt 时会监控目录整个目录的变化，从而再编译输出。

但是在使用中，后期发现随着代码量的增多，最终输出的是根目录下的 `style.scss -> style.css` 文件体积越来越大，虽然最终输出的时候加了 `compressed` 但是 200k 的体积还是不太令人满意。

而且你或许会觉得，sass 文件夹里文件杂乱，因为我们单个文件都是分门别类的命名，难免文件数目会逐渐增多。按照相应的使用场景，按需调用 `@import`，再输出。比如专题页里，`_home.scss, _news.scss` 这类的，页面所需的只是部分样式模块，这时这种拆分式的代码安排，然后再按需组织，是不是觉得非常得心应手呢。

比如 reset 这部分，我后来改成 [normalize.css](http://necolas.github.io/normalize.css/) 并且还是 [staticfile](http://www.staticfile.org/) 的 CDN 服务；iconfont 这部分改为 [Font-Awesome](http://fortawesome.github.io/Font-Awesome/)，也使用了 CDN 服务。

后来在逐渐使用中，发现了项目中的文件结构，不管怎么安排还是应该尽可能的让大家在组织起来都觉得顺手。所以在起初构建的时候，按照需求大致分配一下相应的功能模块，然后再具体实施，毕竟各个项目都有特性，但是基础部分应该都适用。

其实还有个更好的途径，就是去 Github 上观摩大神的项目，就能从中慢慢领悟。列举几个前端框架，大家可以参考学习。特别是 twbs，大概是从 3.1 之后加入了 Official Sass port，之前一直只有 Less。当然了也有别的朋友的 Repo，是由 Less -> Sass，现在已经有了官方版。

1. [bootstrap-sass](https://github.com/twbs/bootstrap-sass)
2. [foundation](https://github.com/zurb/foundation)