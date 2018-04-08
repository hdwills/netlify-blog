---
title: SASS 快速上手
date: 2013-09-04 02:22:22
description: CSS 预处理 - SASS 快速上手
category: Tech
tags: 
  - SASS
  - SCSS
  - CSS
  - Ruby
---

之前就知道有 CSS 预处理器（css preprocessor）这么个概念，但是却一直从未上手实践过，现在看来自己专业技能进步的如此缓慢，嗯，一定是实践能力太弱导致的。

CSS 其实只是个设计工具，个人感觉和编程还是有点距离的，难怪后端程序员们看到这个会觉得处理这些是件很麻烦的事情。只能一行行的如实描述，没有变量、常量、编程语法等，有时候真的感觉难以组织和维护。比如我们项目中产品的迭代，有时候用户需求改变之后，产品要快速的做出响应，有时样式文件做出大的改动，在后期代码的维护上就比较头疼了，因为代码本身耦合度就较高。

> 这时 CSS 预处理器就应运而生了。CSS 预处理器定义了一种新的语言将 CSS 作为目标生成文件，然后开发者就只要使用这种语言进行编码工作了。预处理器通常可以实现浏览器兼容，变量，结构体等功能，代码更加简洁易于维护。

在本地安装好之后，顺着 SASS 官方文档，她的 Features 足以让我惊呆！下面就来挨个做个简单亮相吧！

![http://sass-lang.com](http://sass-lang.com/assets/img/logo-235e394c.png)

## 一、 SASS    
> SASS - Syntactically Awesome Stylesheete    
它的基本思想是，用一种专门的编程语言，进行网页样式设计，然后再编译成正常的 CSS 文件。

## 二、 安装&使用

SASS 是用 Ruby 写的，所以得先安装 [Ruby](http://rubyinstaller.org/downloads/ "RubyInstaller for Windows")。OS X 和 Linux 用户可忽略此步。

```
gem install sass    
sass style.scss     
sass style.scss style.css
```

安装好之后，创建普通的文本文件 style.scss，根据官方的上手指南先写入简单的 Sass Stylesheet。

```css
/* style.scss */    
#navbar {
  width: 80%;
  height: 23px;
}
```

官方首页的 demo 里，你会发现有种语法，表面在我看来就是 `.scss`，`.sass`，扩展名的不同，然后就是书写格式的差异，介绍里是这样说的“.scss 这种扩展名对应的语法一般用的最多，她是 CSS3 的一个超级集合，所以说对 CSS3 的支持也是比较良好”，而“.sass 我猜测应该是 SASS 刚推出时默认采用的吧，她的语法就我目前看来主要体现在书写格式上，只用行缩进来指定代码块，没有和原生 CSS 所相近的 `{}`，`;`”，下面就以 SCSS 语法为主，介绍一些令我和小伙伴们都惊呆了的特性！当然了，这种语法只是在书写格式上有区别，功能上完全一样，选个自己喜欢的开始吧！

## 三、特性概览

### Nesting - 嵌套
原生 CSS 在嵌套时可不能省事儿，一层层一行行都得如实书写，而 SASS 允许你在父级里进行嵌套书写，非常省气力。注意你的括号和缩进！

```css
#navbar {
  width: 80%;
  height: 23px;

  ul { list-style-type: none; }
  li {
    float: left;
    a { font-weight: bold; }
  }
}
```

引用父级，用到了 SASS 约定好的特殊字符`&`

```css
a {
  color: #ce4dd6;
  &:hover { color: #ffb3ff; }
  &:visited { color: #c458cb; }
}
```

### Variables - 变量
SASS 中允许使用变量，她可以用来定义 CSS 属性。

```css
$main-color: #ce4dd6;
$style: solid;

a {
  color: $main-color;
  &:hover { border-bottom: $style 1px; }
}
```

### Interpolation - 嵌入
在 SASS 中定义的变量不仅可以用在属性值里，还可以用`#{}`嵌入在字符串里写在选择器下。这里需要注意的是对`#{}`的闭合要仔细检查，难免会和选择器自身的混淆！

```
$vert: top;
$horz: left;
$radius: 10px;

.rounder-#{$vert}-#{$horz} {
  -webkit-border-#{$vert}-#{$horz}-radius: $radius;
  border-#{$vert}-#{$horz}-radius: $radius; 
}
```

### Operations - 计算
支持标准的数学运算 (+, -, *, /, and %) 

```css
li {
  width: $navbar-width/$items - 10px;
}
```

### Functions - 函数
粗略的浏览了一遍，这些都是处理系列颜色值的函数，有了这些你也可以快速的生成系列颜色。难怪看到人家 UI 组件里的颜色过渡，渐变，同色系颜色的取值上感觉很舒服，有了这些函数让你能够快速的处理出来系列颜色，即使你不是很懂色值的微调。不过我相信，在你看到函数里的那些RGB，HSL，alpha，rgba...肯定还是会很头疼的 >_<，就简单的介绍几个函数，实在不敢继续深究了。

```javascript
rgb(26, 188, 156);
// 根据给定的rgb值，计算出一个十六进制颜色值 #1abc9c
rgba(blue, 0.8);
// rgba(0, 0, 255, 0.8)，将一个颜色根据透明度转换成rgba颜色
lighten(#1abc9c, 10%);
// #28e1bd，lighten和darken都是给颜色亮度值做调整
grayscale(#1abc9c);
// #6b6b6b 变成灰色咯
invert(#1abc9c)
// #e54363 根据颜色中的R、G、B单独进行反相，然后合并到一起
```

### Mixin
这算是 SASS 中的一大强有力的亮点，我个人理解她是一个可定义的代码块，以便用来重复使用。下面这里示例写的有点水啊 :-\

```css
@mixin border {
  $color: #1abc9c;
  $style: solid;
  $width: 1px;
  $radius: 2px;

  border-color: $color;
  border-style: $style;
  border-width: $width;
  border-radius: $radius;
}
```

#### Arguments - 参数
上面那个示例中，在自定义代码块时，起处定义了几个变量，是不是看起来略显麻烦了，而这而所介绍的功能应该就是 Mixins 的强大之处了。再来改进一下

```css
@mixin border($color, $style, $width, $radius) {
  border-color: $color;
  border-style: $style;
  border-width: $width;
  border-radius: $radius;
}

#box { @include border(#1abc9c, solid, 1px, 2px); }
```

### @import
这个功能其实在 CSS 中就有用到，引入外部文件，不过在 SASS 中引入功能更加强大！    
比如将 module 组件以单独的命名的 `_clearfix.sass` 文件存放

```css
@import "border"; //将代码块存为_border.sass，然后引入
@import border.scss //直接以文件名形式引入
@import style.css //CSS文件的引入
```

### Comment - 注释
- SASS 支持标准的 CSS 注释`/* comments */`
- 单行注释`// comments`
- 重要注释`/*! comments */`在/*后面加`!`，表示这是"重要注释"。即使是压缩模式编译，也会保留这行注释

### Output Style - 编译输出
SASS 给出了四种编译风格，[SASS - Output Style](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#output_style "SASS - Output Style")

- `:nested` 标准的嵌套缩进
- `:expanded` 没有嵌套缩进
- `:compact` 每个选择器的样式，均以单行形式输出
- `:compressed` 整个样式文件只有一行，常用于生产环境
- `sass --style expanded style.sass style.css`

## 高级功能
还有些更高级的功能，比如循环、条件语句，自定义函数之类的高级功能就先不做介绍了，等我将以上这些基础功能都实践过之后，一起起来进阶篇，THX :-)

## 开发工具
现在手上的开发工具还是以 [jetbrains](http://www.jetbrains.com/ "JetBrains")家的为主。家里笔记本屏幕小、配置低，主要用 sublime text 2，写文章都是在家里，本文里的示例也都是在这个下面编辑的，以下就简称ST2吧。

我用的版本是 2221，默认还不能支持 SASS 文件的语法高亮，需要安装插件，分别是 sass，sass build。安装好之后，就有代码高亮支持，若还是没看到效果，请手动为当前 sass 文件指定syntax，对应的菜单位置：“View - Syntax - Sass”。

另外就是在ST2中进行编译，对应的菜单位置：“Tools - Build”，快捷键为“Ctrl+B”，这时你在 Console 中就能看到编译状态，默认在当前目录生成一个编译后的同名 CSS 文件。

	overwrite D:\sass/style.css
	[Finished in 1.2s]

果然是神器啊，之前一直都在“命令提示符”里敲命令编译的这种事儿，我会随便乱说？

本文终于可以暂告，每天都说抽时间写一点，结果足足拖了两周之多，该狠狠的扇自己了！里面主要还是以官方文档为主，虽然英文的看起来吃力点，那就手头常备字典咯，遇到代码看不明白的，就再仔细去读一遍说明，这样以来能更好的理解。

工具有很多个，为了提高自己的产出而花时间来学习，还是很有必要的！让自己的代码数量在增加的同时，更应该提高质量，书写优雅的代码，带着一颗热忱的心。

我是 Harry，晚安！