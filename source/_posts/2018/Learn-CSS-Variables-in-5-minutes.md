---
title: 【译】CSS Variables 快速上手
date: 2018-03-22 23:00:00
description: CSS Variables 快速上手教程
category: Tech
tags: CSS Variables
---

## 关于如何快速开始的教程

CSS 自定义属性（也称之为变量）对于前端开发人员来说是一个制胜点。为 CSS 中引入了变量，算是为 CSS 赋予了新的力量，如此一来可以让代码变得更加简洁、灵活。

另外，CSS 原生的变量属于 DOM 的一部分，和预处理器中的不同，相比之下带来了更多的好处。所以更像是 SASS 和 LESS 中的类里的变量。在这篇文章中，我将带你快速学习这项新技术。

### 为什么要用 CSS 变量

有很多理由让你在 CSS 中使用变量，其中最引人注目的一点就是减少你样式文件中的重复代码。（单纯的描述性语言中，难免会有很多重复）

```css
/* The old way */
#title {
  color: #ff6f69;
}

.quote {
  color: #ff6f69;
}
```

在上面的例子中，我们可以通过创建一个变量，比单纯的去重复书写要好一些，就像我们的如下代码：

```css
/* The new way */
:root {
  --red: #ff6f69;
}

#title {
  color: var(--red);
}

.quote {
  color: var(--red);
}
```

这样做不仅仅使你的代码更容易阅读，而且在你要更改色值的时候也提供了更多的灵活性。

如今 SASS 和 LESS 变量确实可以继续使用下去，但是，CSS 变量有一些很大的好处。

1. 它们不需要任务转移工作，因为是浏览器原生属性。所以你不需要做任何设置就可以开始使用，就像你使用 SASS 和 LESS 一样。
2. 它们属于 DOM，从而带来了很多好处，我们加来本文和后续的课程中介绍这些。


现在让我们开始学习 CSS 变量！

### 声明你的第一个 CSS 变量

要声明一个变量，首先需要确定变量存在的范围。如果作为全局变量使用它，就要在 `:root` 这个伪类上声明。这样就能匹配到文档树中的根元素（通常是 `<html>` 标签）

随着变量被继承，整个 web 项目中的变量都可以用，因为所有的 DOM 元素都是 `<html>` 标签的后代元素。

```css
:root {
  --red: #ff6f69;
}
```

正如你所看到的，你可以像设置任何一个 CSS 属性那样去声明一个变量，变量名称必须以两个连接符开始。

要访问到变量，需要使用 `var()` 函数，并将变量名作为参数传入，如下：

```css
#title {
  color: var(--main-color);
}
```

### 声明一个局部变量

当然了，也可以声明一个局部变量，这些局部变量只能对其声明的元素及其它的子元素访问。如果知道某个变量仅用于应用程序的特定部分（或多个部分），则这是有意义的。

例如，你可能有一个 `alert box` 它用到了某个特殊的色值，这个色值不会在项目中其余的地方使用。在这种情况下，避免将其定义为全局变量是有道理的：

```css
.alert {
  --alert-color: #ff6f69;
}
```

这种变量只能用在它的子元素上：

```css
.alert p {
  color: var(--alert-color);
  border: 1px solid var(--alert-color);
}
```

如果你尝试将这个局部变量在程序的其他地方使用，比如用在 `navbar` 上没有任何作用。浏览器会活忽略这行 CSS 代码。

### 通过变量更容易相应

CSS 变量的一大优点是它可以访问 DOM。而对于 LESS、SASS，情况并非如此，它们的变量被便宜为常规的 CSS 代码。

在实践中，你可以像下面的例子这样，修改基于屏幕宽度的变量：

```css
:root {
  --main-font-size: 16px;
}

media all and (max-width: 600px) {
  :root {
    --main-font-size: 12px;
  }
}
```

通过这几行简单的代码，你可以为整个应用在小屏幕上做好字体的适配。这样的处理方式是不是很优雅？

### 通过 JavaScript 访问 CSS 变量

使用 DOM 内部的属性的另一个优点就是你可以通过 JavaScript 访问、更新变量便于交互。如果你想让你的用户能够更改你的网站（比如调整字体大小），这非常合适。

让我们继续本文开始的例子。在 JavaScript 中拿到 CSS 变量只需要如下 3 行代码。

```javascript
var root = document.querySelector(':root');
var rootStyles = getComputedStyle(root);
var mainColor = rootStyles.getPropertyValue('--main-color');

console.log(mainColor);
--> '#ffeead'
```

更新 CSS 变量，只需在你声明的变量上调用 `setProperty` 方法，将变量名作为第一个参数，并将新值作为第二个参数传入。

```javascript
root.style.setProperty('--main-color', '#88d8b0')
```

这种主色可以改变你的应用程序的外观，所以它是允许用户设置你的网站的主题的最佳途径。

### 浏览器支持情况

目前全球 77% 的网站支持 CSS 变量，美国接近 90%。我们已经在 [Scrimba.com](Scrimba.com) 上使用 CSS 变量有一段时间了，因为我们的观众大都精通技术，而且大多数都是用现代浏览器。

[Can I use CSS Variables (Custom Properties)](https://caniuse.com/#search=css%20variable)

好了，就是这样。我希望你学到了一些东西。

如果你想好好的学习一番，请务必在 [Scrimba](https://scrimba.com/g/gcssvariables) 上查看我的免费 CSS Variables 课程。

### 致谢

原作地址 - [Learn CSS Variables in 5 minutes](https://medium.freecodecamp.org/learn-css-variables-in-5-minutes-80cf63b4025d)