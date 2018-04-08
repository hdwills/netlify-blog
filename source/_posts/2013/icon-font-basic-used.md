---
title: Icon Font基础使用
date: 2013-07-07 22:49:56
description: icon font基础使用，icomoon
category: Tech
tags: 
  - icon font
  - icomoon
  - twitter bootstrap
  - Font Awesome
---

目前说到Icon Font已经不是什么新鲜词了，本站当初在构建时就考虑到想要尝试这一新鲜技术。还有就是想在下次项目中得到使用。

主要还是CSS3里自定义字体(@font-face)，现在各大浏览器都已经支持，即使是古老的IE系列，也已经得到了很好的支持。

至于优缺点，大致说说我们之前用图片时的经历。先是将各种小图标拼合处理（其实使用了一些autosprite工具之后，好像这些也都能够很容易的处理完），图标尺寸，颜色，文件格式等一系列问题。当你使用字体图标之后，文件大小得到了有效的控制，图标自身的属性（颜色、大小、透明）修改起来非常方便。但是制作字体图标，也增加了重构的成本，相比后期维护会省时省力很多。

下面大概就说一下Icon Font的基础使用，至于Icon Font的制作部分打算在今后的文章里做深入介绍，因为要用到字体制作软件，其实也就是将Icon的矢量图导入字体里即可。

## 开始使用

- 在CSS中先使用font-face字体声明，以下使用的code均来自[icomoon](http://icomoon.io/app/ "icomoon")，其中在icon是的使用上，有两种方法，一是在标签上用`data-`属性，另一种是使用`class`

```css
@font-face {
    font-family: 'icomoon';    
    src:url('fonts/icomoon.eot');    
    src:url('fonts/icomoon.eot?#iefix') format('embedded-opentype'),    
        url('fonts/icomoon.woff') format('woff'),    
        url('fonts/icomoon.ttf') format('truetype'),    
        url('fonts/icomoon.svg#icomoon') format('svg');    
    font-weight: normal;    
    font-style: normal;    
}

/* Use the following CSS code if you want to use data attributes for inserting your icons */    
[data-icon]:before {    
    font-family: 'icomoon';    
    content: attr(data-icon);    
    speak: none;    
    font-weight: normal;    
    font-variant: normal;    
    text-transform: none;    
    line-height: 1;    
    -webkit-font-smoothing: antialiased;    
}

/* Instead of a list of all class selectors, you can use the generic selector below, but it's slower:[class*="icon-"] { */
.icon-newspaper, .icon-office, .icon-pencil {    
    font-family: 'icomoon';    
    speak: none;    
    font-style: normal;    
    font-weight: normal;    
    font-variant: normal;    
    text-transform: none;    
    line-height: 1;    
    -webkit-font-smoothing: antialiased;    
}    
.icon-newspaper:before {    
    content: "\e000";    
}    
.icon-office:before {    
    content: "\e001";    
}    
.icon-pencil:before {    
    content: "\e002";    
}
```

- 页面中的使用

```html
<div class="fs1" aria-hidden="true" data-icon=""></div>
<span aria-hidden="true" class="icon-newspaper"></span>
```

以上示例来自于[icomoon](http://icomoon.io/app/ "icomoon")，再赞叹此服务如此NB的时候，自己不禁又……上手容易，而且本身提供了好几个图标库，日常使用基本足够，要是难以满足，还可以导入自己的SVG文件之后，重新打包生成，这些都是没问题的。

![icon-font-basic-used-1](http://7xtjgk.com1.z0.glb.clouddn.com/icon-font-basic-used-1.png)

![icon-font-basic-used-2](http://7xtjgk.com1.z0.glb.clouddn.com/icon-font-basic-used-2.png)

![icon-font-basic-used-3](http://7xtjgk.com1.z0.glb.clouddn.com/icon-font-basic-used-3.png)

![icon-font-basic-used-4](http://7xtjgk.com1.z0.glb.clouddn.com/icon-font-basic-used-4.png)

经过简单的几部之后，基本就可以按照你的要求，打包下载。还有一些细节部分，就不一一说明了，在你尝试之后绝对会有惊喜！

说到Font Icon，还有一个不得不提的就是[Font Awesome](http://fortawesome.github.io/Font-Awesome/ "Font Awesome")，这套图标是[Twitter Bootstrap](http://twitter.github.io/bootstrap/ "Twitter Bootstrap")中用到的，现在除了自身起初用到的部分，已经扩充到361 Icons，各种特点和使用就不一一介绍了，有兴趣的可以去官网一探究竟！