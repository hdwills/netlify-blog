---
title: Sass 学习笔记 - @extend, placeholder
date: 2014-05-02 09:00:00
description: Sass and Compass for Designers - @extend, placeholder
category: Tech
tags: 
  - Sass
  - Compass
---
最近在读 《Sass and Compass for Designers》中文版，之前关注过，等了一阵子之后看到中文版面世了，就迫不及待的买了过来，主要是英文水平太渣，只好看中文。

现在就 Sass 中的两项常用功能，也是作为初学者容易混淆的两个概念，作为读书笔记，记录一下。

## 使用 @extend 命令拓展现有代码
    使用 @extend 命令可扩展另一种样式，可让任何样式继承其他样式定义好的属性和值。

我自己理解就是，多个元素的共有属性，就将这些共有属性定义为基础样式组建，然后其余个性化的组件，都是通过这个基础组件来扩展，之前在项目中用到的不多，通常分不清 **@extend** 和 **placeholder** 这两者的使用场景。

下面我们通过书中的代码，来进一步理解：见P76，书中列举的这个例子，经常都能用到。

对话框：基础通用提示、信息提示、错误提示、成功提示等，这个组件的共性很容易能找到，其余个性化的都是在这个基础上去拓展即可。

```css
// variables
$text-color: #666;
$default-bg: #ddd;
$state-info-border: #369;
$state-warning-border: #fcf8e3;
// default
.alert {
    color: $text-color;
    padding: 1em;
    background: $default-bg;
}
// info
.alert-info {
    @extend .alert;
    border: 1px solid $state-info-border;
}
// warning
.alert-warning {
    @extend .alert;
    border: 1px solid $state-warning-border;
}
```
先看一下编译之后的代码吧 :)
```css
.alert, .alert-info, .alert-warning {
    color: #666666;
    padding: 1em;
    background: #dddddd;
}

.alert-info {
    border: 1px solid #336699;
}

.alert-warning {
    border: 1px solid #fcf8e3;
}
```
其实，我本想试试 twbs 里的 alert 组件改写，无奈没有尝试成功。书中提示了，上述代码中，最终 定义的类 `.alert` 并没有使用，其实只充当了声明，让后续的用来拓展，这里就引出了 **placeholder** 来解决这个问题。

## 使用占位符选择器来扩展需要的样式
使用占位符选择器，来定义注视用来做扩展的那些代码。请注意开头位置的占位符选择器 `%`：
```css
// variables
$text-color: #666;
$default-bg: #ddd;
$state-info-border: #369;
$state-warning-border: #fcf8e3;
// default
%alert {
    color: $text-color;
    padding: 1em;
    background: $default-bg;
}
// info
.alert-info {
    @extend %alert;
    border: 1px solid $state-info-border;
}
// warning
.alert-warning {
    @extend %alert;
    border: 1px solid $state-warning-border;
}
```
再来看一下编译之后的代码吧 :)
```css
.alert-info, .alert-warning {
    color: #666666;
    padding: 1em;
    background: #dddddd;
}

.alert-info {
    border: 1px solid #336699;
}

.alert-warning {
    border: 1px solid #fcf8e3;
}
```
    使用占位符选择器，出了扩展的代码职位，不会生存任何冗余代码。
不同的定义方式，对应不同的调用语法来扩展，还是代码看着最直观。