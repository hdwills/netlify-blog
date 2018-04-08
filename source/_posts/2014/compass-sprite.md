---
title: Compass Sprite
date: 2014-08-05 22:00:00
description: Compass Sprite 小试牛刀
category: Tech
tags:
  - Compass
  - Sprite
  - Sass
---
相信大家在前端开发这条路不归路上，都为此头疼过，干这件事情不仅费时费力还费眼睛，各种辅助线打上、网格划上，处处精确到像素级别，长期下来想必大家也都有像素对齐强迫症了，唉……说多了都是泪。那就是今天的主角 - 雪碧图（image spriting），至于他的介绍，他的好，他的工作原理也就不必多说了。

不过今天要介绍的新东西，或许能然你不在重操这些枯燥无味的动作，简单配置之后，一切就那么随意的，先放上官方的教程 [Compass Sprite][1]，下面重点介绍我的个人实践和一些使用经验。

## 文件准备
这里我找了 5 枚 24x24 的 [Glyphicons][2] 图标，`icons` 文件夹里放要拼合的图片资源
![单个图片资源][3]
```html
images/
├── icons/
│   ├── bell.png
│   ├── tree.png
│   ├── table_tennis.png
│   ├── globe.png
└── └── briefcase.png
```

## 简单实践
```scss
// ------ style.scss ------
@import "icons/*.png"; // icons 是文件夹的名称
@include all-icons-sprites; // 这是 compass 定义好的 mixin，一次全部生成 icons 这个文件夹里的所有图片的雪碧图

// ------ style.css ------
.icons-sprite, .icons-bell, .icons-briefcase, .icons-globe, .icons-table_tennis, .icons-tree {
  background: url('../images/icons-sbf1475184a.png') no-repeat;
}
.icons-bell {
  background-position: 0 -48px;
}
.icons-briefcase {
  background-position: 0 -72px;
}
// 省略部分
```
代码部分就是这些了，看一下之前的目录结构，发现在指定的图片文件夹 `images` 里已经生成了拼合好的雪碧图 `icons-sbf1475184a.png`，文件命名是由“指定部分-hash string”，每次在源图片改变时，更新缓存编译之后会生成新的雪碧图。
![拼合好的雪碧图][4]

这里再补充一下，官方文档里还导入了 sprite 组件，个人在使用中发现会报错，就去除了。编译时通过 compass command 是没有任何问题的。[Compass Command Line Documentation][5]

大概是和当前项目配置文件有关，所以建议在没有配置文件的前提下，先用 `creat` 命令创建项目，在对配置文件进行相应的更改。我是在 grunt-compass 中处理的，没有单独进行这些操作。

关于编译时路径报错，可以在 `config.rb` 里根据自己的需求，添加或更改配置项。[Application Integration][6]

## 手动指定类名
有时候会遇到这种情况，带有雪碧图属性的 class 自身还有其余属性，就需要单独去定义。
```scss
// ------ style.scss ------
@import "icons/*.png";
.icons-bell {
  @include icons-sprite(bell); // 注意 mixin 的名称要一致
}

// ------ style.css ------
.icons-sprite, .icons-bell {
  background: url('../images/icons-sbf1475184a.png') no-repeat;
}
.icons-bell {
  margin: 0 auto 30px;
  background-position: 0 -48px;
}
// 省略部分
```
但是从生成的雪碧图，我看到的仍然将整个文件夹里的全部生成了，这里作为标记，等我去查查资料。

## 个性配置
1. 输出布局 [Sprite Layouts][7]
    从图中我们可以看到，默认输出的雪碧图是按各个图片，垂直排列的。Compass 提供了：`Vertical`, `Horizontal`, `Diagonal`, `Smart`，默认以 Vertical 输出。

    ```scss
    $icons-layout: smart; // {文件夹名称}-layout
    ```
    
2. 控制间距
    为每个图片加入空白间隙，此处指定为 `10px`

    ```scss
    $icons-spacing: 10px;
    ```
3. 自动获取当前图片的尺寸

    ```scss
    $icons-sprite-dimensions: true;
    ```
    
配置语句，都写在最前。
```scss
// ------ style.scss ------
$icons-sprite-dimensions: true;
$icons-spacing: 10px;
$icons-layout: horizontal;
@import "icons/*.png";
@include all-icons-sprites;

// Command
>>> Change detected at 23:04:38 to: sass-color.scss
remove images/icons-sfcd98984f1.png
create images/icons-s274ef0b305.png
overwrite css/sass-color.css 

// ------ style.css ------
.icons-sprite, .icons-bell, .icons-briefcase, .icons-globe, .icons-table_tennis, .icons-tree {
  background: url('../images/icons-s274ef0b305.png') no-repeat;
}
.icons-bell {
  background-position: 0 0;
  height: 24px;
  width: 24px;
}
.icons-briefcase {
  background-position: -44px 0;
  height: 21px;
  width: 24px;
}
// 省略部分
```
![带参数后输出的图片][8]

## 拓展阅读
1. [如何禁止 Compass 对生成的 Sprite 图片名字添加随机字符？][9]


  [1]: http://compass-style.org/help/tutorials/spriting/
  [2]: http://glyphicons.com/
  [3]: http://7xtjgk.com1.z0.glb.clouddn.com/compass_sprite_1.png
  [4]: http://7xtjgk.com1.z0.glb.clouddn.com/compass_sprite_2.png
  [5]: http://compass-style.org/help/tutorials/production-css/
  [6]: http://compass-style.org/help/tutorials/integration/
  [7]: http://compass-style.org/help/tutorials/spriting/sprite-layouts/
  [8]: http://7xtjgk.com1.z0.glb.clouddn.com/compass_sprite_3.png
  [9]: http://segmentfault.com/q/1010000000308179